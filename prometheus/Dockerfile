
# base image
FROM prom/prometheus:v2.33.1

# copy configuration file
RUN mkdir -p /etc/prometheus
COPY ./config/prometheus-config.yml /etc/prometheus/prometheus-config.yml