# mainly copied from parity/scripts repo
FROM debian:bullseye-slim

RUN set -eux; \
  apt-get -y update; \
  apt-get install -y --no-install-recommends openssl

# apt clean up
RUN  apt-get autoremove -y && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*