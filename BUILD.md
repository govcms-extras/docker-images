# Building Docker Images

This document provides instructions for building the Docker images.

## Prerequisites

- Docker installed on your system
- Basic knowledge of Docker commands

## Build Instructions/Examples

1. **Clone Repository**: Clone the repository containing the Dockerfile for govcmsextras/php:8.2-cli-alpine.

    ```bash
    git clone <repository_url>
    ```

2. **Navigate to Directory**: Navigate to the directory containing the Dockerfile.

    ```bash
    cd <repository_directory>
    ```

3. **Build Docker Image**: Execute the following command to build the Docker image.

    ```bash
    docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7,linux/arm/v8 -t govcmsextras/php:8.2-cli-alpine --push .
    ```

    This command will build the Docker image for the specified platforms and tag it as `govcmsextras/php:8.2-cli-alpine`.


## Additional Notes

- Ensure that you have Docker Buildx installed and enabled on your system to support multi-platform builds.
- You may need appropriate permissions to push the Docker image to a registry. Make sure you have the necessary credentials configured.