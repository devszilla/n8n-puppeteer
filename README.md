# n8n-puppeteer

This repository contains Docker configurations for deploying different versions of n8n with Puppeteer. You can choose between 3 versions depending on your system and requirements. Below is a guide to help you set up and deploy your desired version.

## Directory Structure

```bash
n8n-puppeteer/
├── n8n-v0.181.2/
│   ├── docker-compose.yml
│   ├── docker-entrypoint.sh
│   └── Dockerfile
├── n8n-v1.79.3/
│   ├── community-nodes/
│   │   └── package.json
│   ├── docker-compose.yml
│   └── Dockerfile
├── n8n-v1.80.2/
│   ├── community-nodes/
│   │   └── package.json
│   ├── docker-compose.yml
│   └── Dockerfile
├── .gitignore
├── README.md
└── template.env
```

### Description of Files and Folders:

- **n8n-v0.181.2/** - Contains the files and configuration to deploy n8n version 0.181.2 with Puppeteer.
- **n8n-v1.79.3/** - Contains the files and configuration to deploy n8n version 1.79.3 with Puppeteer.
- **n8n-v1.80.2/** - Contains the files and configuration to deploy n8n version 1.80.2 with Puppeteer.
- **community-nodes/** - A directory containing custom community nodes specific to versions 1.79.3 and 1.80.2.
- **docker-compose.yml** - Docker Compose configuration file to define the multi-container setup.
- **Dockerfile** - Dockerfile to build the n8n and Puppeteer container image.
- **.env** - Environment configuration file where you set sensitive information and configuration settings like encryption keys and n8n version.
- **docker-entrypoint.sh** (only in versions n8n-v0.181.2 and n8n-v1.80.2) - A script to handle the entrypoint for the Docker container.

---

## Deployment Instructions

1.  **Clone the Repository:**

    ```bash
    git clone https://github.com/devszilla/n8n-puppeteer.git
    cd n8n-puppeteer
    ```

2.  **Choose n8n Version:**

    Navigate to the directory of the n8n version you want to deploy (either `n8n-v1.79.3` or `n8n-v0.181.2` or `n8n-v1.80.2`):

    ```bash
    cd n8n-v1.79.3  # Example: Deploying v1.79.3
    # OR
    cd n8n-v0.181.2  # Example: Deploying v0.181.2
    # OR
    cd n8n-v1.80.2  # Example: Deploying v1.80.2
    ```

3.  **Configure .env:**

    Rename the `template.env` file to `.env`:

    ```bash
    mv template.env .env  # Rename template.env to .env

    # The `.env` file should be located in the same directory as your `docker-compose.yml` file.
    mv .env /path/to/n8n-version-directory
    ```

    Open the `.env` file with a text editor and set the following environment variables:

    *   `N8N_ENCRYPTION_KEY`: Generate a strong encryption key using a service like [https://randomkeygen.com/](https://randomkeygen.com/).  **This is crucial for security.**
    *   `N8N_VERSION`: Set this to the n8n version you're deploying.  It *must* match the directory you're in (e.g., `1.79.3`).  For example: `N8N_VERSION=1.79.3`

    Example `.env` (v1.79.3):

    ```
    N8N_ENCRYPTION_KEY=YOUR_GENERATED_KEY_HERE
    N8N_VERSION=1.79.3
    # ... other environment variables if needed
    ```

4.  **Start n8n with Docker Compose:**

    ```bash
    docker-compose up -d
    ```

    This will build the Docker image (if it doesn't exist) and start the n8n container in detached mode.

5.  **Access n8n:**

    Once the container is running, you can access n8n in your web browser at `http://localhost:5678`.

6.  **Stop n8n:**

    To stop the n8n container:

    ```bash
    docker-compose down
    ```

## Community Nodes (v1.79.3 and 1.80.2 Only)

If you're using n8n v1.79.3 or 1.80.2 and want to include community nodes, place the `package.json` file for those nodes in the `/community-nodes` directory.  The Dockerfile is configured to install these nodes during the image build process.

## Troubleshooting

*   If you encounter any issues, check the Docker logs:  `docker-compose logs -f`
*   Ensure that Docker Desktop or Docker Engine is installed and running.

## Tested Environments

- The older version of n8n (v0.181.2) has been tested and works on a **2015 Dell Optiplex** running **Ubuntu 24.04**.
- n8n version 1.79.3 has been tested and works on a **2025 Mac Pro M4** running **macOS Sequoia (version 15)**.
- The latest n8n version 1.80.2 has been tested and works on a **2015 Dell Optiplex 3020M** running **Windows 2025 DataCenter w/ WSL2**.

These environments have been verified for compatibility, but adjustments may be required based on your specific setup or OS version.