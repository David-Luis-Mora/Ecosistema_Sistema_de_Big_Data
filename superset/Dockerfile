FROM apache/superset:latest

USER root
RUN /usr/local/bin/pip install --target /app/.venv/lib/python3.10/site-packages "SQLAlchemy==1.4.54" \
 && /usr/local/bin/pip install --target /app/.venv/lib/python3.10/site-packages --no-deps "trino[sqlalchemy]"
USER superset