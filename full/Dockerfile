
ARG BASE_IMAGE_TAG="latest"

FROM pretextbook/pretext:${BASE_IMAGE_TAG}

RUN apt-get update && \
pip install --upgrade pip --no-cache-dir && \
pip install --prefer-binary passagemath-standard --no-cache-dir && \
# clean up
apt-get autoremove -y && \
apt-get clean && \
rm -rf /var/lib/apt/lists/* && \
rm -rf /tmp/* /var/tmp/*
