FROM kalilinux/kali-linux-docker

RUN apt-get update && apt-get install -y metasploit-framework tmux python3-pandas python3-docopt python3-msgpack python3-jinja2  python3-urllib3 python3-w3lib  python3-pip  && apt-get clean && pip3 install scrapy && rm -rf /var/lib/apt/lists/*

##Setup MSF
RUN curl -sSL https://github.com/devafatek/GyoiThon/raw/master/docker/msf_setup/db.sql --output /tmp/db.sql
RUN /etc/init.d/postgresql start && su postgres -c "psql -f /tmp/db.sql"
RUN curl -sSL https://github.com/devafatek/GyoiThon/raw/master/docker/msf_setup/database.yml --output /usr/share/metasploit-framework/config/database.yml

#Setup Gyoithon
RUN git clone https://github.com/devafatek/GyoiThon /opt/gyiothon
RUN curl -sSL https://github.com/devafatek/GyoiThon/raw/master/docker/msf_setup/meterpreter.rc --output /tmp/meterpreter.rc
RUN sed -i "s/192.168.220.144/127.0.0.1/g" /opt/gyiothon/classifier4gyoithon/config.ini
RUN sed -i "s/test1234/wh0isyourpwn/g" /opt/gyiothon/classifier4gyoithon/config.ini
RUN sed -i "s/test/secr3tz/g" /opt/gyiothon/classifier4gyoithon/config.ini

##Clean up a few things broken in the code due to suboptimal development
RUN find /opt/gyiothon -name "*.py" -type f | xargs sed -i -e '/OKGREEN + banner + ENDC/d'

WORKDIR "/opt/gyiothon/"

CMD /etc/init.d/postgresql start; tmux new-session -d -s cool 'msfconsole -r /tmp/meterpreter.rc'; sleep 180; curl localhost:55553; python3 /opt/gyiothon/gyoithon.py

########How to run
#Prepopulate your host file with the targets (IE testaspnet.vulnweb.com 80 / )
#docker run -v $PWD/host.txt:/opt/gyiothon/host.txt <container name>
#docker run -v /home/k/Docker/GyoiThon-master/docker/host.txt:/opt/gyiothon/host.txt gyoithon.
# docker run -v /home/k/Docker/GyoiThon-master/docker/host.txt:/opt/gyiothon/host.txt  -v  /home/k/Docker/GyoiThon-master/:/opt/gyiothon gyoithon
