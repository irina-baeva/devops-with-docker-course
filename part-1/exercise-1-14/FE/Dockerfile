# Start from the ubuntu image
FROM node:14

WORKDIR /app

ENV REACT_APP_BACKEND_URL=http://localhost:8080

EXPOSE 5000

# Copy all of the content from the project to the image
COPY . .

# Install node  could be done also RUN curl -sL https://deb.nodesource.com/setup_14.x | bash RUN apt install -y nodejs

RUN npm install -g serve
RUN npm install
RUN npm run build


CMD [ "serve", "-s", "-l", "5000", "build" ]