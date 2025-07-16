# Custom Branding Guide

This guide will help you customize the branding of your Open WebUI instance, including changing the application name, logo, and favicon.

## Table of Contents

- [Custom Application Name](#custom-application-name)
- [Custom Logo and Favicon](#custom-logo-and-favicon)
- [File Requirements](#file-requirements)
- [Troubleshooting](#troubleshooting)

## Custom Application Name

To customize the application name displayed in your Open WebUI instance:

### Step 1: Locate the Configuration File

Navigate to the following file in your Open WebUI installation:
```
backend/open_webui/env.py
```

### Step 2: Modify the WebUI Name Configuration

Find the following code block in the `env.py` file:

```python
WEBUI_NAME = os.environ.get("WEBUI_NAME", "Open WebUI")
if WEBUI_NAME != "Open WebUI":
    WEBUI_NAME += " (Open WebUI)"
```

### Step 3: Comment Out the Suffix Addition

To prevent "(Open WebUI)" from being automatically appended to your custom name, comment out the if statement:

```python
WEBUI_NAME = os.environ.get("WEBUI_NAME", "Open WebUI")
# if WEBUI_NAME != "Open WebUI":
#     WEBUI_NAME += " (Open WebUI)"
```

### Step 4: Set Your Custom Name

You can now set your custom name using the `WEBUI_NAME` environment variable:

```bash
export WEBUI_NAME="Your Custom App Name"
```

Or add it to your Docker configuration:

```yaml
environment:
  - WEBUI_NAME=Your Custom App Name
```

## Custom Logo and Favicon

### Prerequisites

Before proceeding, ensure you have:
- Your custom logo/splash image (recommended: PNG format)
- Your custom favicon files:
  - `favicon-96x96.png` (96x96 pixels)
  - `favicon.svg` (vector format)

### File Requirements

| File Type | Recommended Size | Format |
|-----------|-----------------|---------|
| Splash/Logo | 512x512px or larger | PNG |
| Favicon (raster) | 96x96px | PNG |
| Favicon (vector) | Scalable | SVG |

### Installation Steps

1. **Prepare Your Files**: Ensure your custom branding files are ready and accessible from your terminal.

2. **Identify Your Container**: Find your Open WebUI container name:
   ```bash
   docker ps | grep open-webui
   ```

3. **Copy Files to Container**: Replace `open-webui` with your actual container name and run the following commands:

   ```bash
   # Copy splash/logo image
   docker cp ./splash.png open-webui:/app/backend/open_webui/static/splash.png
   
   # Copy favicon files
   docker cp ./favicon-96x96.png open-webui:/app/build/favicon/favicon-96x96.png
   docker cp ./favicon.svg open-webui:/app/build/favicon/favicon.svg
   ```

4. **Restart the Container**: For changes to take effect:
   ```bash
   docker restart open-webui
   ```

### Alternative Method: Using Docker Compose

If you're using Docker Compose, you can mount your custom files as volumes:

```yaml
version: '3.8'
services:
  open-webui:
    # ... other configuration ...
    volumes:
      - ./custom-branding/splash.png:/app/backend/open_webui/static/splash.png
      - ./custom-branding/favicon-96x96.png:/app/build/favicon/favicon-96x96.png
      - ./custom-branding/favicon.svg:/app/build/favicon/favicon.svg
```

## Troubleshooting

### Common Issues

**Issue**: Changes not visible after copying files
- **Solution**: Restart the container and clear your browser cache

**Issue**: Container name not found
- **Solution**: Use `docker ps` to find the correct container name

**Issue**: Permission denied when copying files
- **Solution**: Ensure Docker has proper permissions and the container is running

### Verifying Changes

1. **Check if files were copied successfully**:
   ```bash
   docker exec open-webui ls -la /app/backend/open_webui/static/splash.png
   docker exec open-webui ls -la /app/build/favicon/
   ```

2. **View container logs** for any errors:
   ```bash
   docker logs open-webui
   ```

### Reverting Changes

To revert to the original branding:

1. **Remove custom files from container**:
   ```bash
   docker exec open-webui rm /app/backend/open_webui/static/splash.png
   docker exec open-webui rm /app/build/favicon/favicon-96x96.png
   docker exec open-webui rm /app/build/favicon/favicon.svg
   ```

2. **Restart the container**:
   ```bash
   docker restart open-webui
   ```

---

> **Note**: These changes are applied to the running container. If you recreate the container, you'll need to reapply the customizations unless you use the Docker Compose volume method.