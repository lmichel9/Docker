FROM opensuse/leap:15.4

#

# Spécifier la commande par défaut à exécuter au lancement du conteneur
#CMD ["/bin/bash"]


#Description and maintainer

LABEL desc="This dockerfile will automate the configuration and installation of prometheus and grafana on a openSUSE image"
LABEL maintainer="MICHEL Louis <louis.michel@pasapas.com>"

#update & upgrade 

RUN zypper refresh
RUN zypper update -y
RUN zypper lp


#packages install
ARG INSTALL="zypper --non-interactive in"
RUN ${INSTALL} nano 
RUN ${INSTALL} wget 
RUN ${INSTALL} gzip
RUN ${INSTALL} tar


#edit CREATE_MAIL_SPOOl 
RUN sed -i 's/^CREATE_MAIL_SPOOL=yes/CREATE_MAIL_SPOOL=no/' /etc/default/useradd


# Create user, group prometheus & grafana
RUN useradd -m prometheus

RUN useradd -m grafana

# add group
RUN groupadd prometheus
RUN usermod -a -G prometheus prometheus

RUN groupadd grafana
RUN usermod -a -G grafana grafana

# Switch root to prometheus
USER prometheus
WORKDIR /home/prometheus

#get prometheus

RUN wget https://github.com/prometheus/prometheus/releases/download/v2.45.0-rc.0/prometheus-2.45.0-rc.0.linux-amd64.tar.gz
RUN gzip -d prometheus-2.45.0-rc.0.linux-amd64.tar.gz
RUN tar -xvf prometheus-2.45.0-rc.0.linux-amd64.tar
RUN rm -rf prometheus-2.45.0-rc.0.linux-amd64.tar
RUN chown prometheus:prometheus prometheus-2.45.0-rc.0.linux-amd64


#Switch root to grafana 
USER grafana 
WORKDIR /home/grafana

#get grafana
RUN wget https://dl.grafana.com/enterprise/release/grafana-enterprise-10.0.0.linux-amd64.tar.gz
RUN tar -xvzf grafana-enterprise-10.0.0.linux-amd64.tar.gz
RUN rm -rf grafana-enterprise-10.0.0.linux-amd64.tar.gz
RUN chown grafana:grafana grafana-10.0.0 

USER root

WORKDIR /home

RUN echo -e '#!/bin/bash\n\ 
cd /home/prometheus/prometheus-2.45.0-rc.0.linux-amd64\n\
./prometheus &\n\
cd /home/grafana/grafana-10.0.0/bin\n\
./grafana-server\
' > script.sh

RUN chmod 700 script.sh


ENTRYPOINT ["./script.sh"]






















