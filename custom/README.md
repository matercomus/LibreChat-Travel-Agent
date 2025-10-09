# Custom Configuration for Tian-Jiang LibreChat Fork

This directory contains all the custom configuration files for this specific fork of LibreChat. This approach allows for easy customization without modifying the core application files, which simplifies pulling updates from the original upstream repository.

## How It Works

All customizations are contained in a **single, self-contained deployment file** (`deploy-compose.custom.yml` in the root directory) that is completely independent from upstream. This file is git-ignored to avoid conflicts when merging updates from the original LibreChat repository.

### Configuration Files

-   **`deploy-compose.custom.yml`** (root directory): The main deployment file with all custom configurations baked in. This is the only file you need to run the application - no layering or merging required. **This file is git-ignored and maintained only locally/on deployment server.**

-   **`custom/librechat.custom.yaml`**: Custom settings for the LibreChat application (welcome messages, MCP servers, endpoints, etc.). Referenced by deploy-compose.custom.yml.

-   **`custom/nginx.custom.conf`**: Custom Nginx configuration for the web server. Referenced by deploy-compose.custom.yml.

-   **`custom/env-custom`**: Custom environment variables and secrets (API keys, database credentials, etc.). Referenced by deploy-compose.custom.yml.

## Deployment

1.  **Populate Secrets**: Fill in your actual secrets and API keys in the `custom/env-custom` file.

2.  **Run Docker Compose**: To start the application with your custom configuration, run the following command from the project root:
    ```bash
    docker compose -f deploy-compose.custom.yml up -d
    ```

3.  **Stop the application**:
    ```bash
    docker compose -f deploy-compose.custom.yml down
    ```

## Merge Strategy - Completely Conflict-Free

This setup is designed to be **100% merge-safe** when pulling updates from the upstream LibreChat repository:

✅ **Custom config files isolated in `custom/` directory** - Upstream never touches this folder

✅ **Deploy file is git-ignored** - Lives in root for simple paths, never committed to avoid conflicts

✅ **No file overlays or merging** - Simple, single-file approach with standard relative paths

✅ **Automatic GitHub Actions deployment** - Uses `deploy-compose.custom.yml` automatically

### What happens during upstream merges:

- ✅ Upstream can modify `deploy-compose.yml` - **No conflict** (we use deploy-compose.custom.yml)
- ✅ Upstream can delete `docker-compose.override.yml` - **No conflict** (we don't use it)  
- ✅ Upstream can add new services or change configs - **No conflict** (our file is git-ignored)
- ✅ You manually review upstream changes and selectively adopt them into your custom file

### Adopting Upstream Changes

When you want to adopt new features from upstream:

1. Review changes: `git diff main -- deploy-compose.yml`
2. Manually apply desired changes to `custom/deploy-compose.custom.yml`
3. Test locally before deploying
4. Commit your updated custom configuration

This gives you **full control** while maintaining **zero merge conflicts**.
