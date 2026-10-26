ARG BASE_IMAGE=public.ecr.aws/sam/build-nodejs22.x
FROM ${BASE_IMAGE}

RUN curl -fsSL -o /tfs-ca.crt https://trust.pki.thermofisher.com/bundles/ca-bundle.crt
ENV REQUESTS_CA_BUNDLE=/tfs-ca.crt
ENV AWS_CA_BUNDLE=/tfs-ca.crt
ENV PIP_CERT=/tfs-ca.crt
ENV SAM_CLI_TELEMETRY=0

# Set the working directory in the container
WORKDIR /app

# Copy package.json and pnpm lockfile
COPY package.json pnpm-lock.yaml /app/

# Set the environment variable to skip Puppeteer download
ENV PUPPETEER_SKIP_DOWNLOAD=true

# Install dependencies
RUN corepack enable \
    && corepack prepare pnpm@10.32.1 --activate \
    && pnpm install --frozen-lockfile

# Copy the current directory contents into the container at /app
COPY ./ /app/

# Expose port 5173 to access the server
EXPOSE 5173

# Command to run the application
CMD ["pnpm", "dev", "--host", "0.0.0.0"]
