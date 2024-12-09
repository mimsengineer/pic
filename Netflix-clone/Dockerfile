FROM node:16.17.0-alpine as builder
WORKDIR /app
COPY Netflix-clone/package.json .
RUN cat /app/package.json

COPY Netflix-clone/yarn.lock .
RUN yarn install
COPY . .
ARG TMDB_V3_API_KEY
ENV VITE_APP_TMDB_V3_API_KEY=${TMDB_V3_API_KEY}
ENV VITE_APP_API_ENDPOINT_URL="https://api.themoviedb.org/3"
RUN echo $VITE_APP_TMDB_V3_API_KEY
RUN cd /app/package.json
RUN yarn run build

FROM nginx:stable-alpine
WORKDIR /usr/share/nginx/html
RUN rm -rf ./*
COPY --from=builder /app/dist .
EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]
