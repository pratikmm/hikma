FROM centos:6.9

RUN yum update -y

RUN yum install -y \
    sudo \
    xz \
    wget \
    git \
    python-setuptools \
    policycoreutils \
    selinux-policy-targeted \
    ansible \
    openssh-server

RUN yum install -y \
    java-1.8.0-openjdk-devel

WORKDIR /opt
RUN wget http://mirrors.estointernet.in/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
RUN sudo tar xzf apache-maven-3.6.0-bin.tar.gz
RUN ln -s apache-maven-3.6.0 maven
ENV M2_HOME=/opt/maven
ENV PATH=${M2_HOME}/bin:${PATH}
RUN mvn -version


RUN wget https://nodejs.org/dist/v10.14.0/node-v10.14.0-linux-x64.tar.xz
RUN mkdir node
RUN tar xvf node-v*.tar.xz --strip-components=1 -C ./node
WORKDIR /root
RUN rm -rf node-v*
RUN mkdir -p mkdir node/etc
RUN echo 'prefix=/usr/local' > node/etc/npmrc
RUN sudo mv node/ /opt/node
RUN sudo chown -R root: /opt/node
RUN sudo ln -s /opt/node/bin/node /usr/local/bin/node
RUN sudo ln -s /opt/node/bin/npm /usr/local/bin/npm

RUN npm install -g \
    bower \
    grunt-cli


RUN yum install -y \
    gcc-c++ \
    patch \
    readline \
    readline-devel \
    zlib \
    zlib-devel \
    libyaml-devel \
    libffi-devel \
    openssl-devel \
    make \
    bzip2 \
    autoconf \
    automake \
    libtool \
    bison \
    iconv-devel \
    sqlite-devel

RUN curl -sSL https://rvm.io/mpapis.asc | gpg --import -
RUN curl -L get.rvm.io | bash -s stable
ENV RVM_HOME=/usr/local/rvm
ENV PATH=${RVM_HOME}/bin:${PATH}
RUN source /etc/profile.d/rvm.sh
RUN rvm reload
RUN rvm requirements run
RUN rvm install 2.5
ENV RUBY_HOME=/usr/local/rvm/rubies/ruby-2.5.1/
ENV PATH=${RUBY_HOME}/bin:${PATH}
RUN gem install compass



#========================== Installing Bahmni ===============================#

WORKDIR /root

# Install the bahmni command line program (Choose the version you want).
#version 0.91
RUN yum install https://dl.bintray.com/bahmni/rpm/rpms/bahmni-installer-0.91-70.noarch.rpm -y

# Confirm that the bahmni command is correctly installed (you should see a help message for the command)
RUN bahmni --help

# Now setup a configuration file for bahmni command in /etc/bahmni-installer.
RUN curl -L https://goo.gl/R8ekg5 >> /etc/bahmni-installer/setup.yml

# Confirm the contents of the file. It should look like this file: (https://goo.gl/R8ekg5)
RUN cat /etc/bahmni-installer/setup.yml

# The above setup.yml, has a timezone entry. You can change it to suit your timezone if you like. For valid options
# please read this document: https://bahmni.atlassian.net/wiki/display/BAH/List+Of+Configurable+Installation+Variables

# Set the inventory file name to local in BAHMNI_INVENTORY environment variable. This way you won't need to use the '-i local' switch every time you use the 'bahmni' command
#You can also configure custom inventory file instead of local.
ENV BAHMNI_INVENTORY=local
RUN source ~/.bashrc

# Now fire the installer
RUN bahmni install

