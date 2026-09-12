# Semantic Security sandbox base — no secrets, no Docker-in-Docker.
FROM mcr.microsoft.com/devcontainers/base:debian

# Keep the image minimal. Add packages only when required for this repo.
# RUN apt-get update \
#  && apt-get install -y --no-install-recommends <packages> \
#  && rm -rf /var/lib/apt/lists/*

USER vscode
WORKDIR /workspaces/llm-semantic-security
