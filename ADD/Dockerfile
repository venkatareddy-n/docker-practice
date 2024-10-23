FROM almalinux:9
RUN dnf install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
ADD https://github.com/venkatswan/docker-practice/blob/main/docker-notes.md /usr/share/nginx/html/index.html
CMD ["nginx", "-g", "daemon off;"]