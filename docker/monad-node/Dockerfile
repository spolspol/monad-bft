FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Update system and install dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    curl \
    nvme-cli \
    aria2 \
    jq \
    ca-certificates \
    gnupg \
    && rm -rf /var/lib/apt/lists/*

# Configure APT repository
RUN mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://pkg.category.xyz/keys/public-key.asc | gpg --dearmor --yes -o /etc/apt/keyrings/category-labs.gpg && \
    echo "Types: deb\nURIs: https://pkg.category.xyz/\nSuites: noble\nComponents: main\nSigned-By: /etc/apt/keyrings/category-labs.gpg" > /etc/apt/sources.list.d/category-labs.sources

# Install monad package
ARG MONAD_VERSION=0.12.2-rpc-hotfix2
RUN apt-get update && apt-get install -y monad=${MONAD_VERSION} && \
    apt-mark hold monad && \
    rm -rf /var/lib/apt/lists/*

# Create monad user and directories
RUN useradd -m -s /bin/bash monad && \
    mkdir -p /home/monad/monad-bft/config \
             /home/monad/monad-bft/ledger \
             /home/monad/monad-bft/config/forkpoint \
             /home/monad/monad-bft/config/validators && \
    chown -R monad:monad /home/monad

USER monad
WORKDIR /home/monad
