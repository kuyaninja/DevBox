FROM nginx:alpine

COPY ./Docker/webserver/config/nginx.conf /etc/nginx/nginx.conf

# ports
EXPOSE 80 443

# stop sig
STOPSIGNAL SIGTERM

# start
CMD ["nginx", "-g", "daemon off;"]