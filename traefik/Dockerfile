# base image
FROM traefik:2.4

# create file to store letsancrypt keys
RUN mkdir -p /etc/traefik
RUN touch /etc/traefik/acme.json
RUN chmod 600 /etc/traefik/acme.json