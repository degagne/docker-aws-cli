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
docker pull ddegagne/aws-cli-python:3.12-alpine3.20
```

You can then run the AWS CLI commands using the following command:

```bash
docker run --rm -it ddegagne/aws-cli-python:3.12-alpine3.20 /usr/bin/aws --version
```

You can also utilize the AWS CLI with your AWS credentials by mounting your credentials file:

```bash
docker run --rm -it -v ~/.aws:/root/.aws ddegagne/aws-cli-python:3.12-alpine3.20 /usr/bin/aws --profile UAT s3 ls
```

## Custom CA Certificates

To integrate custom CA certificates, you can mount a directory containing your CA certificates into the container.
The AWS CLI will automatically use these certificates for HTTPS requests.
