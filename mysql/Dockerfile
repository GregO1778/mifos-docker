FROM mysql:5.5

RUN MAJOR_VERSION=$(echo $NODE_VERSION | cut -d '.' -f 1) && \
    if [ "$MAJOR_VERSION" = "12" ] ; then \
    echo "deb [trusted=yes] http://archive.debian.org/debian stretch main non-free contrib" > /etc/apt/sources.list && \
    echo 'deb-src [trusted=yes] http://archive.debian.org/debian/ stretch main non-free contrib'  >> /etc/apt/sources.list && \
    echo 'deb [trusted=yes] http://archive.debian.org/debian-security/ stretch/updates main non-free contrib'  >> /etc/apt/sources.list; \
    fi

RUN sed -i s/deb.debian.org/archive.debian.org/g /etc/apt/sources.list
RUN sed -i s/security.debian.org/archive.debian.org/g /etc/apt/sources.list
RUN sed -i s/stretch-updates/stretch-proposed-updates/g /etc/apt/sources.list

RUN apt-get update \
	&& apt-get install -y wget unzip \
	&& apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /docker-entrypoint-initdb.d 	&& cd / 

COPY ./00-create-databases.sql /docker-entrypoint-initdb.d/00-create-databases.sql

COPY ./01-mifospltaform-tenants-first-time-install.sql /docker-entrypoint-initdb.d/01-mifospltaform-tenants-first-time-install.sql

# ADD ./02-load_sample_data.sql /docker-entrypoint-initdb.d/02-load_sample_data.sql

VOLUME /docker-entrypoint-initdb.d
