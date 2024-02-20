# Extended GovCMS Docker Images

This repository contains Docker images designed for testing and development with GovCMS. These images are tailored for specific PHP versions and use cases, providing an environment suitable for testing GovCMS projects.

## Overview

- **Images**: Each Docker image is tagged with the respective PHP version and use case.
- **Use Cases**: Images are optimized for specific use cases such as CLI, Apache, or FPM usage, catering to different requirements of GovCMS testing projects.
- **Testing Environment**: Use these Docker images to create a reliable testing environment for your GovCMS projects, ensuring compatibility and stability.

## Usage

1. **Pull Image**: Pull the desired Docker image from Docker Hub.

    ```bash
    docker pull govcmsextras/php:8.2-nginx
    ```

2. **Run Container**: Run a Docker container using the pulled image, adjusting options as needed.

    ```bash
    docker run --name govcms-testing -d govcmsextras/php:8.2-nginx
    ```

3. **Access Container**: Access the running container to perform testing and development tasks.

    ```bash
    docker exec -it govcms-testing /bin/sh
    ```

## Available Images

- `govcmsextras/php:8.2-nginx`
- `govcmsextras/php:8.2-nginx-alpine`
- `govcmsextras/php:8.2-apache`
- `govcmsextras/php:8.2-apache-alpine`
- `govcmsextras/php:8.2-cli-alpine`
- `govcmsextras/dnsmasq:latest`

## Contribution

Contributions are welcome! If you encounter any issues or have suggestions for improvement, please feel free to open an issue or submit a pull request on GitHub.

## License

This project is licensed under the [GNU General Public License](LICENSE.md) - see the [LICENSE.md](LICENSE.md) file for details.
