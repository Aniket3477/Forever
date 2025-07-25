# Stage 1: Build the frontend
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./

# Forcefully clear npm cache and then install dependencies
RUN npm cache clean --force && npm install

# Change starts here: Copy index.html and then other source files
COPY index.html ./ 
COPY . .           

RUN npm run build

# Stage 2: Serve with nginx
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]