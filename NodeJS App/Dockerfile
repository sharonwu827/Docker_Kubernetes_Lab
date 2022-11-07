# build own image with a dockerfile
FROM node

# tell docker to run in app folder
WORKDIR /app

# which file in the local machine should go into the image
COPY . /app

RUN npm install

RUN node server.js