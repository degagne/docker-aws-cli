ARG DISTRO_VERSION

# ------------------- Stage 1: Pre-build Stage -------------------
FROM python:3.10-alpine${DISTRO_VERSION} AS python-alpine

# Include global args for the pre-build stage
ARG CA_CERTIFICATE

# Download the CA certificates bundle
ADD ${CA_CERTIFICATE} /usr/local/share/ca-certificates/ca-certificates.crt

# Add Python script to extract certificates into individual files.
COPY ../extract-certs-bundle.py /usr/local/bin/extract-certs-bundle

# Register the new CA certificates bundle
RUN chmod +x /usr/local/bin/extract-certs-bundle \
    && python3 /usr/local/bin/extract-certs-bundle \
    && rm -f /usr/local/share/ca-certificates/ca-certificates.crt \
    && update-ca-certificates

# ------------------- Stage 2: Final Stage -------------------
FROM python-alpine AS final-image

# Copy the CA certificates from the pre-build stage.
COPY --from=python-alpine /usr/local/share/ca-certificates /usr/local/share/ca-certificates

# Copy the script to extract certificates into individual files.
COPY --from=python-alpine /usr/local/bin/extract-certs-bundle /usr/local/bin/extract-certs-bundle

# Update CA certificates in the final image.
RUN update-ca-certificates

# Install AWS CLI dependencies and utilities.
RUN apk add --no-cache --no-check-certificate aws-cli

# Create a non-root user for running AWS CLI commands.
RUN adduser -D -u 1000 awsuser

WORKDIR /home/awsuser

ENV HOME=/home/awsuser

USER awsuser

ENTRYPOINT ["/usr/bin/aws"]
