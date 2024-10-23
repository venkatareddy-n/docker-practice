FROM almalinux:9
RUN dnf install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
ADD https://www.w3schools.com/html/html_editors.asp /usr/share/nginx/html/index.html
RUN chmod +r /usr/share/nginx/html/index.html 
ADD sample-1.tar /tmp/
CMD ["nginx", "-g", "daemon off;"]