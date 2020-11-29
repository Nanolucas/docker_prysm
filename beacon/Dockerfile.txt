FROM alpine:latest

WORKDIR /prysm
RUN apk add --no-cache curl \
    && addgroup -S app \
	&& adduser -S app -G app \
	&& chown -R app:app /prysm

USER app
RUN curl https://prysmaticlabs.com/releases/beacon-chain-v1.0.1-linux-arm64 --output beacon_process \
    && chmod +x beacon_process
ENTRYPOINT ["./beacon_process"]
