FROM nginx:alpine

COPY nginx.conf /etc/nginx/nginx.conf
COPY certs /etc/nginx/certs/
EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
