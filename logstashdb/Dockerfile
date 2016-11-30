FROM logstash:2

RUN logstash-plugin install logstash-output-mongodb

COPY logstash-db.conf /etc/logstash/

CMD ["-f", "/etc/logstash/logstash-db.conf", "--debug"]
