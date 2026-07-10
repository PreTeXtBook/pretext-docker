
ARG BASE_IMAGE_TAG="latest"
# The asymptote package installed via tlmgr in the base image lags behind
# upstream releases, so the full image builds the latest release from source.
ARG ASYMPTOTE_VERSION="3.12"

FROM pretextbook/pretext:${BASE_IMAGE_TAG}

RUN apt-get update && \
apt-get install -y --no-install-recommends \
            python3-dev \
            build-essential \
            gcc \
            g++ && \
pip install --upgrade pip --no-cache-dir && \
pip install cysignals==1.12.6 && \
pip install --prefer-binary passagemath-standard --no-cache-dir && \
# clean up
apt-get remove -y \
            python3-dev \
            build-essential \
            gcc \
            g++ && \
apt-get autoremove -y && \
apt-get clean && \
rm -rf /var/lib/apt/lists/* && \
rm -rf /tmp/* /var/tmp/*

ARG ASYMPTOTE_VERSION

# Build the latest Asymptote release from source, using its classic
# ./configure build (not the newer CMake build, which currently hardcodes
# Vulkan into the core library with no graceful fallback). The autotools
# build compiles the Vulkan and OpenGL renderers into separate shim
# libraries (libasyvulkan.so / libasyopengl.so) that asy dlopen()s lazily,
# only when a script actually needs 3D rendering, and configure happily
# degrades (with a notice, not a failure) if a renderer's dependencies are
# missing -- matching how Debian's own asymptote package is built.
#
# Since Codespaces/CI containers have no real GPU, mesa-vulkan-drivers and
# libgl1-mesa-dri provide Mesa's software implementations (lavapipe and
# llvmpipe) so whichever renderer asy picks at runtime still works.
#
# The library "-dev" packages are left installed (not autoremoved) because
# asy links against their runtime .so files; only the pure build tools are
# safe to drop afterwards.
RUN apt-get update && \
apt-get install -y --no-install-recommends \
            bison \
            flex \
            build-essential \
            zlib1g-dev \
            libsigsegv-dev \
            libreadline-dev \
            libncurses-dev \
            libcurl4-openssl-dev \
            libfftw3-dev \
            libeigen3-dev \
            libgsl-dev \
            libtirpc-dev \
            libglfw3-dev \
            libgl-dev \
            libvulkan-dev \
            glslang-dev \
            spirv-tools \
            mesa-vulkan-drivers \
            libgl1-mesa-dri \
            libglx-mesa0 && \
curl -fsSL --retry 5 --retry-all-errors --retry-delay 5 "https://sourceforge.net/projects/asymptote/files/${ASYMPTOTE_VERSION}/asymptote-${ASYMPTOTE_VERSION}.src.tgz/download" -o /tmp/asymptote.tgz && \
mkdir -p /tmp/asymptote-src && \
tar -xzf /tmp/asymptote.tgz -C /tmp/asymptote-src --strip-components=1 && \
cd /tmp/asymptote-src && \
# Debian's glslang-dev no longer ships libOGLCompiler.a, which configure's
# Vulkan detection asks for as a fallback link flag. In practice this never
# matters on this distro: configure's *first*, simpler linking attempt
# (-lglslang alone) already succeeds against Debian's glslang-dev, so the
# -lOGLCompiler fallback path is never reached. This LIBS hint is just a
# harmless extra safety net in case that ever changes.
# install-asy depends on the "sty" target, which rebuilds doc/asy-latex.pdf
# via pdflatex -- that needs LaTeX packages (e.g. hypdoc) the base image's
# TinyTeX doesn't carry. We don't need that PDF (base/Dockerfile's tlmgr
# install already provides asymptote.sty for \usepackage{asymptote}), so
# turn "sty"'s recipe into a no-op before configure generates the real
# Makefile from this template. (A dummy "sty" file doesn't work here:
# its prerequisite doc/version.texi is unconditionally regenerated on
# every run, which makes "sty" look stale again regardless of its mtime.)
sed -i 's|^\(\s\)cd doc && \$(MAKE) asy-latex\.pdf$|\1true|' Makefile.in && \
LIBS="-lglslang-default-resource-limits" ./configure --prefix=/usr/local --disable-lsp && \
make -j"$(nproc)" asy && \
make install-asy && \
cd / && \
apt-get remove -y bison flex build-essential && \
apt-get autoremove -y && \
apt-get clean && \
rm -rf /var/lib/apt/lists/* /tmp/asymptote* /tmp/* /var/tmp/* && \
# Use the explicit path rather than relying on PATH: the base image's
# ENV PATH="/root/bin:$PATH" still puts tlmgr's older asy first at this
# point in the build (the PATH override below only applies to later
# instructions), so a bare "asy -version" here would silently check the
# wrong binary. Fail the build if Vulkan didn't actually compile in,
# since that was the whole point of building from source.
/usr/local/bin/asy -version 2>&1 | tee /tmp/asy-version.log && \
grep -q "^Vulkan" /tmp/asy-version.log && \
rm -f /tmp/asy-version.log

# Make sure the freshly-built asy takes precedence over the older copy
# that tlmgr placed earlier on PATH in the base image.
ENV PATH="/usr/local/bin:${PATH}"
# Codespaces/CI containers have no GPU device nodes; force Mesa's
# llvmpipe software rasterizer instead of probing for hardware.
ENV LIBGL_ALWAYS_SOFTWARE=1
