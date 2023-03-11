# pull image from node version 14.17.3 alpine on docker hub 
FROM node:14.17.3-alpine3.14

# set work directory at /usr/src/app
WORKDIR /usr/src/app

# copy package.json and package-lock.json to image
COPY package.json package-lock.json ./

# execute command for install node_modules
RUN npm i -y

# copy server.js to image 
COPY ./server.js ./

# execute command for start application
CMD ["npm","start"]
