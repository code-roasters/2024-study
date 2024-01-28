# Setup base image
From node:21-alpine3.18

# Set working directory
WORKDIR /usr/src/app

# Copy app file
COPY . .

# Install http server
RUN npm install -g http-server

# Expose port
EXPOSE 8080

# Run http server
CMD [ "http-server", "-p 8080" ]
