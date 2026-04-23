FROM node:14-alpine AS web-build

WORKDIR /usr/src/app

# Install dependencies first to improve build cache reuse.
COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build --prod

FROM nginx:1.27-alpine

COPY --from=web-build /usr/src/app/dist/client/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 4200
CMD ["nginx", "-g", "daemon off;"]
