FROM oscarlevin/pretext:small

RUN pip install --prefer-binary passagemath-standard --no-cache-dir

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