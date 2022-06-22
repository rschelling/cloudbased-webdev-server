# base image
FROM authelia/authelia:4

# copy config files into container
RUN mkdir -p /config
COPY ./config/configuration.yml /config/configuration.yml
COPY ./config/users_database.yml /config/users_database.yml

# add secrets to container
RUN mkdir -p /secrets
RUN --mount=type=secret,id=AUTHELIA_JWT_SECRET cat /run/secrets/AUTHELIA_JWT_SECRET > /secrets/jwt
RUN --mount=type=secret,id=AUTHELIA_SESSION_SECRET cat /run/secrets/AUTHELIA_SESSION_SECRET > /secrets/session
RUN --mount=type=secret,id=AUTHELIA_STORAGE_ENCRYPTION_KEY cat /run/secrets/AUTHELIA_STORAGE_ENCRYPTION_KEY > /secrets/encryption
RUN --mount=type=secret,id=AUTHELIA_NOTIFIER_SMTP_PASSWORD cat /run/secrets/AUTHELIA_NOTIFIER_SMTP_PASSWORD > /secrets/smtp