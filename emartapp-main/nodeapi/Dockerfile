FROM node:14-alpine

WORKDIR /usr/src/app

# Install dependencies first for better layer caching.
COPY package*.json ./
RUN npm install --omit=dev

COPY . .

EXPOSE 5000
CMD ["npm", "start"]
