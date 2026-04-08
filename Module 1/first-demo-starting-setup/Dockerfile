# Use node 14 base image
FROM node:14

# Create and use /app as working directory just to segregate our app
WORKDIR /app

# Copy package.json file to working directory
COPY package.json .

# Since it is a node base image, we have npm available which we use to install third-party libraries
RUN npm install

# Copy rest of the file/ our code/ app to current working directory
COPY . .

# Expose 3000 port to external world from container
EXPOSE 3000

# Run command to start application inside container
CMD [ "node", "app.mjs" ]