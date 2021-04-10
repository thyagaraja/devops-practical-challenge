# Pull base image 
From nginx 
EXPOSE 80
RUN rm -v /etc/nginx/nginx.conf
COPY nginx.conf /etc/nginx/
RUN rm -v /usr/share/nginx/html
COPY static-site /usr/share/nginx/html
RUN nginx -t
RUN service nginx start 
# Maintainer 
MAINTAINER "b.t.raja225@gmail.com" 

