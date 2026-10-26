ARG BASE_IMAGE=public.ecr.aws/sam/build-python3.12
FROM ${BASE_IMAGE}

RUN curl -fsSL -o /tfs-ca.crt https://trust.pki.thermofisher.com/bundles/ca-bundle.crt
ENV REQUESTS_CA_BUNDLE=/tfs-ca.crt
ENV AWS_CA_BUNDLE=/tfs-ca.crt
ENV PIP_CERT=/tfs-ca.crt
ENV SSL_CERT_FILE=/tfs-ca.crt
ENV CURL_CA_BUNDLE=/tfs-ca.crt
ENV SAM_CLI_TELEMETRY=0
ENV NODE_EXTRA_CA_CERTS=/tfs-ca.crt

ENV POETRY_VERSION 1.8.0

# Install system dependencies
RUN pip install "poetry==$POETRY_VERSION"

# Set work directory
WORKDIR /app

# Copy only requirements to cache them in docker layer
COPY poetry.lock pyproject.toml /app/

# Disable the creation of virtual environments
RUN poetry config virtualenvs.create false

# Install dependencies
RUN poetry install

# Install Chromium for Playwright. The AWS SAM base image is not Debian-based,
# so Playwright's --with-deps apt workflow is not available here.
RUN playwright install chromium

# Copy the current directory contents into the container at /app
COPY ./ /app/
