
FROM node:lts-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5000
ENV MONGO_URI=mongodb+srv://ihebbaccouch1999:IEhlPE6OnMjWcneW@cluster0.luagi.mongodb.net/devops?retryWrites=true&w=majority


CMD ["npm", "start"]
