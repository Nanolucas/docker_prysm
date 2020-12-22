ARG BASE
FROM debian:unstable-slim as builder
USER root
RUN apt-get update && apt-get install -y curl ca-certificates && \
  curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/master/contrib/install.sh | sh -s -- -b /usr/local/bin && \
  apt-get autoremove -y curl libldap-2.4 &&  \
  trivy filesystem --exit-code 0 --no-progress / > /scan_result 2>&1 && \
  rm /usr/local/bin/trivy && \
  rm -rf /root/.cache/trivy  