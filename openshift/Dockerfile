# --- Build stage ---
    FROM registry.access.redhat.com/ubi8/python-312 AS builder

<<<<<<< HEAD
    USER 1001
    WORKDIR /opt/CTFd
    
    RUN yum install -y gcc git make libffi-devel openssl-devel && \
        python -m venv /opt/venv && \
        yum clean all
    
    ENV PATH="/opt/venv/bin:$PATH"
    
    COPY ../. /opt/CTFd
    
    RUN pip install --upgrade pip && pip install -r requirements.txt
    
    # --- Runtime stage ---
    FROM registry.access.redhat.com/ubi8/python-312
    
    USER 1001
    WORKDIR /opt/CTFd
    
    COPY --from=builder /opt/CTFd /opt/CTFd
    COPY --from=builder /opt/venv /opt/venv
    
    ENV PATH="/opt/venv/bin:$PATH"
    
    EXPOSE 8000
    
    CMD ["gunicorn", "-b", "0.0.0.0:8000", "CTFd:create_app()"]
=======
USER 1001

WORKDIR /opt/CTFd

# hadolint ignore=DL3008
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libffi-dev \
        libssl-dev \
        git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && python -m venv /opt/venv

ENV PATH="/opt/venv/bin:$PATH"

COPY ../. /opt/CTFd

RUN pip install --no-cache-dir -r requirements.txt \
    && for d in CTFd/plugins/*; do \
        if [ -f "$d/requirements.txt" ]; then \
            pip install --no-cache-dir -r "$d/requirements.txt";\
        fi; \
    done;


FROM python:3.12-slim-bookworm AS release

USER 1001

WORKDIR /opt/CTFd

# hadolint ignore=DL3008
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libffi8 \
        libssl3 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --chown=1001:1001 ../. /opt/CTFd

RUN useradd \
    --no-log-init \
    --shell /bin/bash \
    -u 1001 \
    ctfd \
    && mkdir -p /var/log/CTFd /var/uploads \
    && chown -R 1001:1001 /var/log/CTFd /var/uploads /opt/CTFd \
    && chmod +x /opt/CTFd/docker-entrypoint.sh

COPY --chown=1001:1001 --from=build /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

USER 1001
EXPOSE 8000
ENTRYPOINT ["/opt/CTFd/docker-entrypoint.sh"]
>>>>>>> refs/remotes/origin/master
