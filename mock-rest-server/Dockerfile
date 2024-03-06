# Base this image on an official Node.js long term support image.
FROM node:18.16.0-alpine

# Install some additional packages that we need.
RUN apk add --no-cache tini curl bash sudo

# Use Tini as the init process. Tini will take care of important system stuff
# for us, like forwarding signals and reaping zombie processes.
ENTRYPOINT ["/sbin/tini", "--"]

# Install simple REST server module
RUN npm --silent install -g json-server@0.8.14

# Create a working directory for our application.
RUN mkdir -p /app
WORKDIR /app

# Copy our application files into the image.
COPY . /app

# Switch to a non-privileged user for running commands inside the container.
RUN chown -R node:node /app \
 && echo "node ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-node
USER node

# Start the server on exposed port 3000.
EXPOSE 3000
CMD [ "json-server", "--watch", "db.json" ]

# Start the server on exposed port 3000.