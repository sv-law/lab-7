# Base this image on an official Node.js long term support image.
FROM node:18.16.0-alpine

# Install some additional packages that we need.
RUN apk add --no-cache tini curl bash sudo

# Use Tini as the init process. Tini will take care of important system stuff
# for us, like forwarding signals and reaping zombie processes.
ENTRYPOINT ["/sbin/tini", "--"]

# Install nodemon and json-server globally
RUN npm --silent install -g nodemon json-server@0.8.14

# Create a working directory for our application.
RUN mkdir -p /app
WORKDIR /app

# Copy our application files into the image.
COPY . /app

# Switch to a non-privileged user for running commands inside the container.
RUN chown -R node:node /app \
 && echo "node ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-node
USER node

# Expose the port specified in the environment variable, defaulting to 3000.
ARG PORT=3000
EXPOSE $PORT

# Start the server on the specified port using nodemon
CMD ["sh", "-c", "npx nodemon --watch . --ext js,json --exec 'json-server --watch db.json --port ${PORT}'"]
