# --- Build stage ---
    FROM registry.access.redhat.com/ubi8/python-312 AS builder

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