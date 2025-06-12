FROM oscarlevin/pretext:small

RUN export SAGE_CONF_TARGETS="fpylll gmpy2 lrcalc_python memory_allocator pplpy primecountpy"

RUN pip install --force-reinstall -v passagemath-conf

RUN export PIP_FIND_LINKS="$(sage-config SAGE_SPKG_WHEELS)"

RUN export PIP_PREFER_BINARY=1

RUN pip install --prefer-binary passagemath-standard --no-cache-dir --no-compile
