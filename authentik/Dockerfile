FROM ghcr.io/goauthentik/server:2025.10.2
USER root

RUN \
    echo "Update Packages List" && \
    apt update && \
    \
    echo "Install Package: Patch" && \
    apt install -y patch  && \
    \
    echo "Overwriting License: Internal Users" && \
    sed -i "s/internal_users: int = 0/internal_users: int = 1024/" /authentik/enterprise/license.py && \
    \
    echo "Overwriting License: External Users" && \
    sed -i "s/external_users: int = 0/external_users: int = 65536/" /authentik/enterprise/license.py

USER authentik