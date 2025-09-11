# Custom Configuration for LibreChat Fork

This directory contains all the custom configuration files for this specific fork of LibreChat. This approach allows for easy customization without modifying the core application files, which simplifies pulling updates from the original upstream repository.

## How It Works

The customization is handled by Docker Compose's ability to merge multiple configuration files. The `docker-compose.override.yml` file in the root of the project is the control center for applying these custom configurations.

-   **`custom/librechat.custom.yaml`**: This file contains your custom settings for the LibreChat application itself. It is volume-mounted into the `api` service, replacing the default `librechat.yaml`.

-   **`custom/nginx.custom.conf`**: This is your custom Nginx configuration. It gets mounted into the `client` service, overwriting the default `default.conf`.

-   **`custom/.env.custom`**: This file holds your custom environment variables and secrets. It is loaded by the services defined in the override file, and its values will take precedence over any variables defined in a default `.env` file.

## Deployment

1.  **Populate Secrets**: Fill in your actual secrets and API keys in the `custom/.env.custom` file.
2.  **Run Docker Compose**: To start the application with your custom configuration, run the following command from the project root:
    ```bash
    docker compose -f deploy-compose.yml -f docker-compose.override.yml up -d
    ```
