
ARG BASE_IMAGE_TAG

FROM oscarlevin/pretext:${BASE_IMAGE_TAG}

RUN apt-get update && \
apt-get install -y --no-install-recommends \
            python3-dev \
            build-essential \
            gcc && \
apt-get install -y  --no-install-recommends\
            cysignals-tools \
            python3-cysignals-pari \
            python3-gmpy2 \
            lrcalc \
            python3-memory-allocator \
            python3-ppl \
            ppl-dev \
            python3-primecountpy && \
pip install --prefer-binary passagemath-standard --no-cache-dir && \
# clean up
apt-get remove -y \
            python3-dev \
            build-essential \
            gcc && \
apt-get autoremove -y && \
apt-get clean && \
rm -rf /var/lib/apt/lists/* && \
rm -rf /tmp/* /var/tmp/*
