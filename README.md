## Route53 plugin for Let's Encrypt client which requires less privileges

### Before you start

- Ensure your domain is already managed by route53
- Ensure you have IAM with the policy, described in [examples/sample-aws-policy.json](examples/sample-aws-policy.json).
  - Note that it requires less privileges than [the original dns-route53 plugin](https://github.com/certbot/certbot/blob/master/certbot-dns-route53/examples/sample-aws-policy.json).
- Prepare correspondences the hosted zone ids and domains which you want to obtain certificates.

### Setup

1. Create a virtual environment

2. Update its pip and setuptools (`VENV/bin/pip install -U setuptools pip`)
to avoid problems with cryptography's dependency on setuptools>=11.3.

3. Make sure you have libssl-dev and libffi (or your regional equivalents)
installed. You might have to set compiler flags to pick things up (I have to
use `CPPFLAGS=-I/usr/local/opt/openssl/include
LDFLAGS=-L/usr/local/opt/openssl/lib` on my macOS to pick up brew's openssl,
for example).

4. Install this package.

### How to use it

Make sure you have access to AWS's Route53 service, either through IAM roles or
via `.aws/credentials`. Check out
[sample-aws-policy.json](examples/sample-aws-policy.json) for the necessary permissions.

To generate a certificate:
```
certbot certonly \
  -n --agree-tos --email DEVOPS@EXAMPLE.COM \
  --authenticator 'dns-route53-less' \
  '--dns-route53-less-zone-ids=example.com=(hosted zone id),example.org=(hosted zone id)'
  -d 'example.com' -d '*.example.com' \
  -d 'example.org' -d '*.example.org'
```
