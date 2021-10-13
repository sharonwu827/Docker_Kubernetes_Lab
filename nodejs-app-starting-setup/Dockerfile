
# this file contails the setup instructions for our own image
# the node is a name docker will be able to find
# and there would be a image named node
FROM node

# tell docker all commands should be executed in the folder
# setting the working directory of the docker container
WORKDIR /app

# tell docker which files that live on local machine should go into the image
COPY package.json /app

RUN npm install

COPY . /app

EXPOSE 80

CMD ["node","server.js"]