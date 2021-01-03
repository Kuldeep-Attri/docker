# As discussed before always start from a base Image(look on DockerHub)
FROM node:8.16.0-alpine
# This will create a directory in the container with name 'app' and make it PWD
WORKDIR /app
# Add already build packeges
ADD package*.json ./
# Install libraries
RUN npm install
# Add all files from the current directory to '/app' directory
ADD ./ ./
# CMD to run commands 
CMD node index.js
# Done