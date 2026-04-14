FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

ENV PORT=8000

EXPOSE $PORT

# VOLUME [ "/app/node_modules" ]

CMD [ "npm", "start" ]