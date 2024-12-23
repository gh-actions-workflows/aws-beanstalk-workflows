# AWS Elastic Beanstalk Deploy Workflow

Publish your Docker image to AWS Elastic Beanstalk.

## How to use?

```yaml
name: Deploy to AWS EB
on: [push]

jobs:
  publish:
    uses: gh-actions-workflows/docker-workflows/.github/workflows/docker-publish.yaml@master
    with:
      app_name: my_app_name # The application name will be used to name the image
      docker_hub_user: ${{ vars.DOCKER_HUB_USER }}
    secrets:
      docker_hub_password: ${{ secrets.DOCKER_HUB_PASSWORD }}

  deploy:
    needs: publish
    uses: gh-actions-workflows/aws-beanstalk-workflows/.github/workflows/beanstalk.yaml@master
    secrets:
      aws_key_id: ${{ secrets.AWS_KEY_ID }}
      aws_secret_key: ${{ secrets.AWS_SECRET_KEY }}
      aws_region: ${{ secrets.AWS_REGION }}
      aws_bucket_name: ${{ secrets.AWS_BUCKET_NAME }}
    with:
      app_name: my_app_name
      docker_image: ${{ needs.publish.outputs.image_name }}
      container_port: 8080
```

For more details on how it works, see the file: [beanstalk.yaml](https://github.com/gh-actions-workflows/aws-beanstalk-workflows/blob/master/.github/workflows/beanstalk.yaml).
