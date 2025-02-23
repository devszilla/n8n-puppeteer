# Use build argument for N8N version
ARG N8N_VERSION

# Use the specified n8n version
FROM n8nio/n8n:${N8N_VERSION}

# Switch to root user for package installation to have sufficient permissions
USER root

# Install Chromium and necessary dependencies for Puppeteer to function correctly
RUN apk add --no-cache \
    # Headless browser required for Puppeteer
    chromium \
    # Security libraries for SSL/TLS
    nss \
    # Font rendering library
    freetype \
    # Development files for font rendering
    freetype-dev \
    # Text shaping engine
    harfbuzz \
    # CA certificates for secure HTTPS connections
    ca-certificates \
    # Free TrueType fonts
    ttf-freefont \
    # Device management
    udev \
    # Node.js runtime environment
    nodejs \
    # Node.js package manager
    npm

# Switch back to the node user, as n8n runs under the 'node' user
USER node

# Create the 'nodes' directory if it doesn't already exist in the n8n home directory
RUN mkdir -p /home/node/.n8n/nodes

# Copy the 'package.json' for community nodes into the container for npm installation
COPY community-nodes/package.json /home/node/.n8n/nodes/

# Install the necessary npm dependencies inside the 'nodes' directory
RUN npm install --prefix /home/node/.n8n/nodes

# Set Puppeteer to use the installed Chromium browser for rendering
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser