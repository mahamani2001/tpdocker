FROM node:lts-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
ENV MONGO_URI=mongodb://mongo:27017/employees
CMD ["npm", "start"]


