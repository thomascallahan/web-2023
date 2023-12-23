FROM node:20

# Create app directory and copy all source into it
WORKDIR /code
COPY package*.json ./

# Install app dependencies
RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . . 

EXPOSE 3000
CMD [ "node", "server.js" ]
