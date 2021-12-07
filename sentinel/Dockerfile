# Always add commit hash for reproducability
FROM node:14.18.1-alpine@sha256:557f00fb5d780597b0e7bcdc6d93abeb7e73599bcbfeba5832dc5646a8d3f120
ENV NODE_ENV=production
RUN apk add --no-cache git
ARG UPSTREAM_VERSION

WORKDIR /opt/sentinel
RUN git clone -b ${UPSTREAM_VERSION} --recurse-submodules -j8 https://github.com/superfluid-finance/superfluid-sentinel.git .
# Enable prod optimizations
RUN chown node:node /opt/sentinel

RUN npm ci --only=production

COPY docker-entrypoint.sh /usr/sbin/docker-entrypoint.sh
RUN chmod +x /usr/sbin/docker-entrypoint.sh
# Add a simple init system so that Node would respect process signals
ENTRYPOINT ["sh", "/usr/sbin/docker-entrypoint.sh"]

# Don't run as root
USER node
