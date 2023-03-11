# pull image from nginx version 1.21.6 on docker hub 
FROM nginx:1.21.6
# execute command for remove old config
RUN rm /etc/nginx/conf.d/default.conf
# copy new config to image
COPY nginx.conf /etc/nginx/conf.d/default.conf
