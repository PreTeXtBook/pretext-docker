FROM oscarlevin/pretext:small

ARG TARGETPLATFORM

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
                    python3-dev \
                    bzip2 \
                    gcc \
                    build-essential \
                    tox \
                    libboost-dev \
                    libboost-regex-dev \
                    pkg-config \
                    m4 \
                    flex \
                    bison \
                    bc \
                    libbz2-dev \
                    libffi-dev \
                    libbrial-dev \
                    libbrial-groebner-dev \
                    libcdd-dev \
                    libcdd-tools \
                    cliquer \
                    libcliquer-dev \
                    cmake \
                    curl \
                    4ti2 \
                    libcurl4-openssl-dev \
                    ecl \
                    libec-dev \
                    ninja-build \
                    patchelf \
                    libmpfr-dev \
                    ppl-dev && \
if [ "$TARGETPLATFORM" = "linux/arm64" ]; then \
    echo "Installing additional dependencies for ARM64 architecture" && \
    export SAGE_CONF_TARGETS="fpylll gmpy2 lrcalc_python memory_allocator pplpy primecountpy" && \
    export SAGE_NUM_THREADS=1 && \
    pip install -v passagemath-conf && \
    export PIP_FIND_LINKS="$(sage-config SAGE_SPKG_WHEELS)" && \
    export PIP_PREFER_BINARY=1; \
fi && \
pip install --prefer-binary passagemath-standard --no-cache-dir && \
apt-get remove -y \
        python3-dev \
        bzip2 \
        gcc \
        build-essential \
        tox \
        libboost-dev \
        libboost-regex-dev \
        pkg-config \
        m4 \
        flex \
        bison \
        bc \
        libbz2-dev \
        libffi-dev \
        libbrial-dev \
        libbrial-groebner-dev \
        libcdd-dev \
        libcdd-tools \
        cliquer \
        libcliquer-dev \
        cmake \
        curl \
        4ti2 \
        libcurl4-openssl-dev \
        ecl \
        libec-dev \
        ninja-build \
        patchelf \
        libmpfr-dev \
        python3-trove-classifiers \
        python3-fpylll \
        libmpc-dev \
        python3-typing-extensions \
        libfplll-dev \
        gfortran \
        libzmq3-dev \
        libopenblas-dev \
        python3-tomli \
        flit \
        python3-flit-scm \
        ppl-dev \
        cython3 \
        fplll-tools && \
apt-get autoremove -y && apt-get clean && \
rm -rf /var/lib/apt/lists/* && rm -rf /tmp/* /var/tmp/*

#apt-get update && apt-get install -y --no-install-recommends \
#    binutils \
#    make \
#    m4 \
#    flex \
#    bison \
#    perl \
#    python3 \
#    python3-dev \
#    bc \
#    gcc \
#    g++ \
#    ca-certificates \
#    libbz2-dev \
#    bzip2 \
#    xz-utils \
#    libffi-dev \
#&& export SAGE_CONF_TARGETS="fpylll gmpy2 lrcalc_python memory_allocator pplpy primecountpy" \
#&& pip install --force-reinstall -v --prefer-binary passagemath-conf \
#&& export PIP_FIND_LINKS="$(sage-config SAGE_SPKG_WHEELS)" \
#&& export PIP_PREFER_BINARY=1 \

#\
#&& apt-get purge --auto-remove -y binutils make m4 flex bison gcc g++ libbz2-dev libffi-dev python3-dev \
#&& apt-get clean && rm -rf /var/lib/apt/lists/*