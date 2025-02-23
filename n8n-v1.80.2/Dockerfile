FROM node:18.17.0-alpine

ARG N8N_VERSION

RUN if [ -z "$N8N_VERSION" ] ; then echo "The N8N_VERSION argument is missing!" ; exit 1; fi

# Update everything and install needed dependencies
RUN apk add --update graphicsmagick tzdata git tini su-exec

# Set a custom user to not have n8n run as root
USER root

# Install n8n and also temporary all the packages
# it needs to build it correctly.
RUN apk --update add --virtual build-dependencies python3 build-base ca-certificates && \
    npm_config_user=root npm install -g full-icu n8n@${N8N_VERSION} && \
    apk del build-dependencies \
    && rm -rf /root /tmp/* /var/cache/apk/* && mkdir /root;

# Installs latest Chromium (100) package.
RUN apk add --no-cache \
      chromium \
      nss \
      freetype \
      harfbuzz \
      ttf-freefont \
      yarn

# Tell Puppeteer to skip installing Chrome. We'll be using the installed package.
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true \
    PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# Create the 'nodes' directory if it doesn't already exist in the n8n home directory
RUN mkdir -p /home/node/.n8n/nodes

# Copy the 'package.json' for community nodes into the container for npm installation
COPY community-nodes/package.json /home/node/.n8n/nodes/

# Install the necessary npm dependencies inside the 'nodes' directory
RUN npm install --prefix /home/node/.n8n/nodes

# Install fonts
RUN apk --no-cache add --virtual fonts msttcorefonts-installer fontconfig && \
	update-ms-fonts && \
	fc-cache -f && \
	apk del fonts && \
	find  /usr/share/fonts/truetype/msttcorefonts/ -type l -exec unlink {} \; \
	&& rm -rf /root /tmp/* /var/cache/apk/* && mkdir /root

ENV NODE_ICU_DATA /usr/local/lib/node_modules/full-icu

WORKDIR /data

# Copy the docker-entrypoint.sh script and set permissions
COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh \
    && chown root:root /docker-entrypoint.sh

# Set entry point with tini
ENTRYPOINT ["tini", "--", "/docker-entrypoint.sh"]

EXPOSE 5678/tcp
