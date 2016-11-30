FROM logstash:2

RUN logstash-plugin install logstash-output-mongodb

COPY logstash-q.conf /etc/logstash/

CMD logstash -f /etc/logstash/logstash-q.conf --debug
