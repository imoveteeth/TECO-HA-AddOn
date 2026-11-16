# TECO Billing add-on image.
# Slim Python base + Chromium only — installed right after the pip package so
# Playwright and its browser are always the same version (the full
# mcr.microsoft.com/playwright image pins an older browser and is ~6 GB).

FROM python:3.12-slim-bookworm

ENV PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    PLAYWRIGHT_BROWSERS_PATH=/ms-playwright

WORKDIR /app

# Python deps, then the matching Chromium + its OS libraries
COPY app/requirements.txt ./

RUN apt-get update && apt-get install -y \
    libatomic1 \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir -r requirements.txt \
    && playwright install chromium

# app code (vendored from sidecar/ via ./sync.sh)
COPY app/ ./
RUN chmod +x run.sh

# ingress port (must match config.yaml ingress_port)
EXPOSE 8089

CMD [ "./run.sh" ]
