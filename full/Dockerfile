
ARG BASE_IMAGE_TAG="latest"

FROM pretextbook/pretext:${BASE_IMAGE_TAG}

RUN apt-get update && \
apt-get install -y --no-install-recommends \
            python3-dev \
            build-essential \
            gcc && \
apt-get install -y  --no-install-recommends\
            cysignals-tools \
            python3-cysignals-pari \
            python3-gmpy2 \
            libgmp-dev \
            libmpfr-dev \
            libmpc-dev \
            lrcalc \
            fflas-ffpack \
            m4 \
            libm4ri-dev \
            pari-gp \
            gsl-bin \
            libgsl-dev \
            python3-memory-allocator \
            python3-ppl \
            ppl-dev \
            python3-primecountpy && \
pip install --upgrade pip --no-cache-dir && \
pip install --force-reinstall --prefer-binary -v passagemath-conf && \
export PIP_FIND_LINKS=$(sage-config SAGE_SPKG_WHEELS) && \
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
