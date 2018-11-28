FROM ubuntu:18.04

USER root

RUN apt-get update

RUN apt-get install -y \
    sudo \
    curl \
    vim \
    nano \
    gnupg \
    gcc \
    make \
    wget 

RUN apt-get update
RUN apt-get install -y ruby-full


#installing nodejs and git
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install -y \
    nodejs \
    git

#install node modules
RUN npm install -g bower
RUN npm install -g grunt-cli
RUN gem install compass

#installing apache webserver
RUN apt-get install -y apache2

#COPY ./build-project.sh /
#RUN sh +x /build-project.sh

RUN mkdir -p /root/projects
WORKDIR /root/projects

# clone the default configuration (some folks may choose their project specific config like jss-config)
RUN git clone https://github.com/Bahmni/default-config.git

# clone Bahmni EMR UI project
RUN git clone https://github.com/Bahmni/openmrs-module-bahmniapps.git


#RUN cd /root/projects/openmrs-module-bahmniapps/ui/
WORKDIR /root/projects/openmrs-module-bahmniapps/ui/

RUN npm install

RUN bower install --allow-root

WORKDIR /root/projects/openmrs-module-bahmniapps/ui/app/component
RUN mkdir hustle
WORKDIR /root/projects/openmrs-module-bahmniapps/ui/app/component/hustle
RUN wget https://raw.githubusercontent.com/DWB-eHealth/hustle/master/hustle.js

WORKDIR /root/projects/openmrs-module-bahmniapps/ui/
RUN grunt clean test --force

RUN grunt --force

RUN rm -rf /var/www/html/*
RUN cp -r /root/projects/openmrs-module-bahmniapps/ui/dist/* /var/www/html/
WORKDIR /var/www/html
RUN mkdir bahmni_config
RUN cp -r /root/projects/default-config/* /var/www/html/bahmni_config/
RUN chown -R www-data:www-data /var/www/html


COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/bin/sh", "-c", "/entrypoint.sh"]


