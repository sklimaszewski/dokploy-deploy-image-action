# Dokploy Deploy Image Action

This repository contains GitHub Actions for triggering specific docker image deployment for applications in Dokploy.
This is useful if you want to trigger it after a build and push of your docker image in your CI.

## Features

- Update application Docker image in Dokploy
- Trigger deployment on Dokploy
- Easy integration with GitHub workflows

## Usage

Example usage in a GitHub Actions workflow file:

```yaml
name: Dokploy Deployment Workflow

on: [push]

jobs:
    deploy:
        runs-on: ubuntu-latest
        steps:
            -   name: Checkout source code
                uses: actions/checkout@v3

            -   name: Set short commit SHA
                id: vars
                run: |
                    calculatedSha=$(git rev-parse --short ${{ github.sha }})
                    echo "COMMIT_SHORT_SHA=$calculatedSha" >> $GITHUB_ENV

            -   name: Dokploy deployment
                uses: sklimaszewski/dokploy-deploy-image-action@0.0.1
                with:
                    dokploy_url: ${{ secrets.DOKPLOY_URL }}
                    auth_token: ${{ secrets.DOKPLOY_TOKEN }}
                    application_id: ${{ secrets.DOKPLOY_APP_ID }}
                    docker_image: ghcr.io/example/dokploy:${{ env.COMMIT_SHORT_SHA }}
```

## Inputs

- `dokploy_url`: Dokploy base URL, e.g. https://dokploy.example.com
- `auth_token`: Dokploy API authentication token
- `application_id`: Dokploy application ID (can be retrieved from the URL of the application in Dokploy)
- `docker_image`: Docker image

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for any bugs or feature requests.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.