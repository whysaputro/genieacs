FROM node:20-bullseye

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y sudo supervisor git
RUN mkdir -p /var/log/supervisor

WORKDIR /opt
RUN git clone https://github.com/whysaputro/genieacs.git

WORKDIR /opt/genieacs
RUN npm install
RUN npm i -D tslib
RUN npm run build

RUN useradd --system --no-create-home --user-group genieacs

RUN mkdir /opt/genieacs/ext
RUN chown genieacs:genieacs /opt/genieacs/ext

RUN mkdir /var/log/genieacs
RUN chown genieacs:genieacs /var/log/genieacs

RUN echo '/var/log/genieacs/*.log /var/log/genieacs/*.yaml {\n\
    daily\n\
    rotate 30\n\
    compress\n\
    delaycompress\n\
    dateext\n\
    }' > /etc/logrotate.d/genieacs

RUN cp docker/services/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
RUN cp docker/run_with_env.sh /usr/bin/run_with_env.sh
RUN chmod +x /usr/bin/run_with_env.sh

WORKDIR /var/log/genieacs

CMD [ "/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf" ]