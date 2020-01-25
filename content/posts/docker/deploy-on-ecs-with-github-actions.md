---
title: "Deploy on AWS-ECS with Github Actions"
date: 2020-01-26T01:24:40+09:00
draft: false
tags: [ "docker", "aws", "ecs", "task-definition" ]
---

## GitHub Actionsを使ってDockerコンテナをECSにデプロイ
GitHub ActionsではAWS用のプラグインが充実しているのでそれを利用する。secretsは、repositoryのsettingsから設定できる。`wait-for-service-stability: true`にしているのは、デプロイ完了を待たないようにするため。(サービスを再起動していないので、タスクが上手く更新されないのでタイムアウトする)

```aws.yml
on:
  push:
    branches:
      - deploy-*

name: Deploy to Amazon ECS

jobs:


  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v1

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ap-northeast-1

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1

    - name: Build, tag, and push image to Amazon ECR
      id: build-image
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        ECR_REPOSITORY: ${{ secrets.AWS_ECR_REPO_NAME }}
        IMAGE_TAG: ${{ github.sha }}
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
        echo "::set-output name=image::$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG"

    - name: Fill in the new image ID in the Amazon ECS task definition
      id: task-def
      uses: aws-actions/amazon-ecs-render-task-definition@v1
      with:
        task-definition: task-definition.json
        container-name: container-name
        image: ${{ steps.build-image.outputs.image }}

    - name: Deploy Amazon ECS task definition
      uses: aws-actions/amazon-ecs-deploy-task-definition@v1
      with:
        task-definition: ${{ steps.task-def.outputs.task-definition }}
        service: service-name
        cluster: cluster-name
        # wait-for-service-stability: true

    - name: Logout of Amazon ECR
      if: always()
      run: docker logout ${{ steps.login-ecr.outputs.registry }}

```

## Task Definition
task-definition.jsonをDockerfileと同じレイヤーに設置する。imageは自動的に置き換わる。簡単な設定だと以下の感じ。

```task-definition.json
{
    "containerDefinitions": [
        {
            "name": "container-name",
            "image": "container-image",
            "portMappings": [
                {
                    "hostPort": 80,
                    "protocol": "tcp",
                    "containerPort": 80
                }
            ],
            "command": [
                "sh",
                "-c",
                "/usr/sbin/nginx; /usr/local/sbin/php-fpm; crond -f -l 2"
            ],
            "cpu": 100,
            "memory": 200,
            "essential": true,
            "readonlyRootFilesystem": false,
            "privileged": false
        }
    ],
    "executionRoleArn": "ecsTaskExecutionRole",
    "family": "service-name"
}
```