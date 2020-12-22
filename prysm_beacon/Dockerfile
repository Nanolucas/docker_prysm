ARG BEACON_BASE

FROM nanolucas/prysm_base:${BEACON_BASE} as builder
ARG RELEASE
RUN : "${RELEASE:?Build argument needs to be set and non-empty.}"
WORKDIR /tmp/build
RUN apt-get update && apt-get install -y wget bash && \
  wget -q https://github.com/prysmaticlabs/prysm/releases/download/${RELEASE}/beacon-chain-${RELEASE}-linux-arm64 -O beacon-chain && \
  wget -q https://github.com/prysmaticlabs/prysm/releases/download/${RELEASE}/validator-${RELEASE}-linux-arm64 -O validator

FROM nanolucas/prysm_base:${BEACON_BASE} as beacon-chain
RUN apt-get install -y bash
ARG SERVICE_USER
ARG SERVICE_HOME
ENV SERVICE_USER ${SERVICE_USER:-app}
ENV SERVICE_HOME ${SERVICE_HOME:-/opt/${SERVICE_USER}}
WORKDIR ${SERVICE_HOME}
# copy binaries
COPY --from=builder /tmp/build/beacon-chain beacon-chain
# create user
RUN adduser --home ${SERVICE_HOME} --shell /sbin/nologin --uid 2000 ${SERVICE_USER} && \
  chown -R ${SERVICE_USER}:${SERVICE_USER} ${SERVICE_HOME}
RUN chmod +x ${SERVICE_HOME}/beacon-chain
USER ${SERVICE_USER}
ENTRYPOINT ["/opt/app/beacon-chain"]

FROM nanolucas/prysm_base:${BEACON_BASE} as validator
RUN apt-get install -y bash
ARG SERVICE_USER
ARG SERVICE_HOME
ENV SERVICE_USER ${SERVICE_USER:-app}
ENV SERVICE_HOME ${SERVICE_HOME:-/opt/${SERVICE_USER}}
WORKDIR ${SERVICE_HOME}
# copy binaries
COPY --from=builder /tmp/build/validator validator
# create user
RUN adduser --home ${SERVICE_HOME} --shell /sbin/nologin --uid 2000 ${SERVICE_USER} && \
  chown -R ${SERVICE_USER}:${SERVICE_USER} ${SERVICE_HOME}
RUN chmod +x ${SERVICE_HOME}/validator
USER ${SERVICE_USER}
ENTRYPOINT ["/opt/app/validator"]