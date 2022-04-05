FROM nodered/node-red:2.2.2-12

RUN npm install node-red-dashboard
RUN npm install node-red-contrib-nordpool-api-plus
RUN npm install node-red-contrib-bluelinky
RUN npm install node-red-contrib-moment
RUN npm install node-red-node-smooth

RUN npm install full-icu

# use external storage for the user directory
#VOLUME /data

# expose port
#EXPOSE 1880

# Set the default command to execute
# when creating a new container
#CMD /usr/local/bin/node-red

