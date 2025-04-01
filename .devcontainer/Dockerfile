FROM mcr.microsoft.com/devcontainers/miniconda:1-3
COPY env.yml /tmp/env.yml
RUN umask 0002 && \
    /opt/conda/bin/conda env update --solver libmamba -n base -f /tmp/env.yml && \
    rm /tmp/env.yml
