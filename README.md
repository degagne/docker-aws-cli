# Amazon Web Services Command-Line Interface (CLI) for Python

This Docker image provides a lightweight environment for running the AWS CLI with Python and Alpine Linux.
It includes the functionality to integrate custom CA certificates, making it suitable for environments
that require specific CA trust stores.

## Tagging

The following tagging conventions are used in this repository: `<py_version>-alpine<distro_version>`.

For example, `3.12-alpine3.20` indicates Python 3.12 on Alpine Linux 3.20.

## Supported Tags and `Dockerfile` Links

- `3.13`
  - [`3.13-alpine3.20`](https://github.com/degagne/docker-aws-cli/tree/main/3.13)
  - [`3.13-alpine3.21`](https://github.com/degagne/docker-aws-cli/tree/main/3.13)
  - [`3.13-alpine3.22`](https://github.com/degagne/docker-aws-cli/tree/main/3.13)

- `3.12`
  - [`3.12-alpine3.20`](https://github.com/degagne/docker-aws-cli/tree/main/3.12)
  - [`3.12-alpine3.21`](https://github.com/degagne/docker-aws-cli/tree/main/3.12)
  - [`3.12-alpine3.22`](https://github.com/degagne/docker-aws-cli/tree/main/3.12)

- `3.11`
  - [`3.11-alpine3.20`](https://github.com/degagne/docker-aws-cli/tree/main/3.11)
  - [`3.11-alpine3.21`](https://github.com/degagne/docker-aws-cli/tree/main/3.11)
  - [`3.11-alpine3.22`](https://github.com/degagne/docker-aws-cli/tree/main/3.11)

- `3.10`
  - [`3.10-alpine3.20`](https://github.com/degagne/docker-aws-cli/tree/main/3.10)
  - [`3.10-alpine3.21`](https://github.com/degagne/docker-aws-cli/tree/main/3.10)
  - [`3.10-alpine3.22`](https://github.com/degagne/docker-aws-cli/tree/main/3.10)

## Usage

To use this Docker image, you can pull it from the Docker registry:

```bash
docker pull ddegagne/aws-cli-python:3.13-alpine3.22
```

You can then run the AWS CLI commands using the following command:

```bash
docker run --rm -it ddegagne/aws-cli-python:3.13-alpine3.22 /usr/bin/aws --version
```

You can also utilize the AWS CLI with your AWS credentials by mounting your credentials file:

```bash
docker run --rm -it -v ~/.aws:/root/.aws ddegagne/aws-cli-python:3.13-alpine3.22 /usr/bin/aws --profile UAT s3 ls
```

## Custom CA Certificates

To integrate custom CA certificates, you can mount a directory containing your CA certificates into the container.
The AWS CLI will automatically use these certificates for HTTPS requests.

To implement custom CA certificates, you can create a new image based on this one as follows:

```Dockerfile
ARG RUNTIME_VERSION
ARG DISTRO_VERSION

# ------------------- Step 1: Base Image -------------------
FROM ddegagne/aws-cli:${RUNTIME_VERSION}-alpine${DISTRO_VERSION}

# Set global variables
ARG CA_CERTIFICATE

# Add custom CA certificate.
ADD ${CA_CERTIFICATE} /usr/local/share/ca-certificates/custom-ca.crt

# Extract the CA certificate bundle and update the system's CA certificates.
# Invocation of the Python script is only necessary if the `CA_CERTIFICATE` contains a bundle of certificates.
RUN python3 /usr/local/bin/extract-certs-bundle \
    && update-ca-certificates

# Set the entrypoint to the AWS CLI
ENTRYPOINT ["/usr/bin/aws"]
```

To build this custom image, you can use the following command:

```bash
docker build \
  --build-arg RUNTIME_VERSION=3.13 \
  --build-arg DISTRO_VERSION=3.22 \
  --build-arg CA_CERTIFICATE=path/to/your/custom-ca.crt \
  -t aws-cli-3.13-alpine3.22 .
```

Then you can run it with:

```bash
docker run --rm -it aws-cli-3.13-alpine3.22 /usr/bin/aws s3 ls
```

Again, you can mount your AWS credentials directory as shown earlier to use your AWS profile.

```bash
docker run --rm -it -v ~/.aws:/root/.aws aws-cli-3.13-alpine3.22 /usr/bin/aws --profile production s3 ls
```
