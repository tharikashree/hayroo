FROM node:18-alpine
WORKDIR /client

COPY *.json .
RUN npm install
COPY . .

EXPOSE 3000

CMD [ "npm", "run" ,"start" ]