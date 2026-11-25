FROM debian:13-slim

# Use bash for the shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Create a script file sourced by both interactive and non-interactive bash shells
ENV BASH_ENV=/root/.bash_env
RUN touch "${BASH_ENV}"
RUN echo '. "${BASH_ENV}"' >> ~/.bashrc

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
                    locales \
                    python3 \
                    python3-pip \
                    python3-venv \
&& apt-get autoremove -y \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* \
&& rm -rf /tmp/* /var/tmp/*

ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# Generate the specific locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen

# Set environment variables
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Exact pretext version to install (e.g. "2.17.3"). The deploy workflow sets
# this when triggered by a pretext-cli release, so the image is guaranteed to
# contain that release (and a stale PyPI index fails loudly rather than
# silently installing the previous version). Empty means "latest".
ARG PRETEXT_VERSION=

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
                    git \
                    wget \
                    curl \
                    less \
                    python3-dev \
                    python3-louis \
                    python3-cairo \
                    libcairo2-dev \
                    libglut3.12 \
                    librsvg2-bin \
                    libssl-dev \
                    ghostscript \
                    jing \
                    gcc \
                    build-essential \
                    ca-certificates \
                    gnupg \
                    xz-utils \
                    fop \
# perl, the perl modules, and ImageMagick needed to process WeBWorK
# problems with a local copy of PG (see "Local WeBWorK rendering" in
# README.md). The module list is the one from the PreTeXt Guide plus
# pg's own cpanfile, and libfuture-asyncawait-perl, which core's
# script/webwork/pg-ptx.pl needs for "use Mojo::Base -async_await".
                    perl \
                    imagemagick \
                    libclass-accessor-perl \
                    libclass-tiny-perl \
                    libdbi-perl \
                    libencode-perl \
                    libfuture-asyncawait-perl \
                    libgd-perl \
                    libgd-text-perl \
                    libhtml-parser-perl \
                    liblocale-maketext-lexicon-perl \
                    libmojolicious-perl \
                    libtie-ixhash-perl \
                    libtypes-serialiser-perl \
                    libuuid-tiny-perl \
                    libyaml-libyaml-perl \
&& NODE_MAJOR=22 \
# Create a directory for the new repository's keyring, if it doesn't exist
&& mkdir -p /etc/apt/keyrings \
# Download the new repository's GPG key and save it in the keyring directory
&& curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
# Add the new repository's source list with its GPG key for package verification
&& echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_${NODE_MAJOR}.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list \
&& apt-get update \
&& apt-get install -y --no-install-recommends nodejs \
&& node -v \
&& pip install pip --upgrade --no-cache-dir \
&& pip install "pretext[all]${PRETEXT_VERSION:+==$PRETEXT_VERSION}" codechat-server citeproc-py-styles --prefer-binary --no-cache-dir \
&& apt-get install  -y --no-install-recommends libnspr4 libnss3 libdbus-1-3 libatk1.0-0 libatk-bridge2.0-0 libcups2 libxkbcommon0 libatspi2.0-0 libxcomposite1 libxdamage1 libxfixes3 libxrandr2 libgbm1 libasound2 \
&& playwright install chromium \
&& echo '/usr/lib/python3/dist-packages' > /usr/local/lib/python3.13/dist-packages/louis.pth \
&& echo '/usr/lib/python3/dist-packages' > /opt/venv/lib/python3.13/site-packages/louis.pth \
&& prefig init \
&& npm install @mermaid-js/mermaid-cli -g \
&& apt-get remove -y \
                    python3-dev \
                    gcc \
                    build-essential \
&& apt-get autoremove -y \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* \
&& rm -rf /tmp/* /var/tmp/*

# make shell prompt pretty
RUN echo 'export PS1="\[\e[1;32m\]\u@\h:\[\e[1;34m\]\w \[\e[0m\]\$ "' >> ~/.bashrc

# Set TMPDIR to avoid permission issues on ARM architectures
ENV TMPDIR=/

RUN wget -O TinyTex.sh "https://yihui.org/tinytex/install-bin-unix.sh" \
&& bash TinyTex.sh \
&& rm TinyTex.sh

#TinyTeX installs links to the executable in /root/bin, so we need to add that to the PATH
ENV PATH="/root/bin:$PATH"

RUN tlmgr update --self

RUN tlmgr install adjustbox amscdx appendix asymptote beamer bold-extra braket bussproofs cancel carlisle cases chessfss circuitikz colorprofiles colortbl comment dvisvgm eepic elsarticle enumitem extpfeil extsizes fancyhdr fontawesome5 fontaxes footnotehyper gensymb hobby imakeidx jknapltx kastrup l3experimental lambda-lists latex-lab libertinus-fonts listings listingsutf8 luamml makeindex marginnote marvosym mathalpha mathtools menukeys mhchem microtype multirow musicography musixtex-fonts ncctools newpx newtx nicematrix overpic parskip pdfcol pdfpages pdflscape pgfplots phaistos physics pict2e polyglossia polynom pstricks qrcode realscripts relsize siunitx setspace skak skaknew smartdiagram snapshot stackengine standalone stmaryrd sttools tagpdf tcolorbox threeparttable tikz-3dplot tikz-cd tikzfill titlesec tkz-euclide txfonts ulem upquote was wrapfig xfrac xltxtra xpatch xstring \
&& tlmgr path add

#  Ensure fonts provided by TinyTeX are available, as suggested in the pretext guide

RUN fontconfig_path="/etc/fonts/conf.d/09-texlive-fonts.conf" && \
    fontconfig="<?xml version=\"1.0\"?>\n<!DOCTYPE fontconfig SYSTEM \"fonts.dtd\">\n<fontconfig>\n  <dir>~/.TinyTeX/texmf-dist/fonts</dir>\n  <dir>~/.TinyTeX/texmf-local/fonts</dir>\n</fontconfig>" && \
    if [ ! -f "$fontconfig_path" ]; then \
        echo "Creating fontconfig file at $fontconfig_path"; \
        echo -e "$fontconfig" > "$fontconfig_path"; \
    else \
        echo "Fontconfig file already exists at $fontconfig_path"; \
    fi && \
    fc-cache -f -v

# Local WeBWorK rendering: a copy of the PG repository so that a publication
# file with <webwork static-processing="local"/> can process WeBWorK problems
# in the container instead of on a remote webwork2 server. This follows the
# "Using a Local Copy of PG" section of the PreTeXt Guide
# (https://pretextbook.org/doc/guide/html/webwork-local-pg-static-processing.html):
# pg is cloned to the Guide's default location (/opt/webwork/pg, so no
# @pg-location is needed in the publication file) and PG_ROOT points at it.
# PG_VERSION is a release tag of https://github.com/openwebwork/pg; PreTeXt
# needs 2.19 or later.
ARG PG_VERSION="PG-2.21"
ENV PG_ROOT=/opt/webwork/pg

RUN mkdir -p /opt/webwork \
&& git clone --depth 1 --branch "${PG_VERSION}" https://github.com/openwebwork/pg "${PG_ROOT}" \
&& grep -q "PG_VERSION" "${PG_ROOT}/VERSION" \
# pg_config.yml holds only what differs from pg_config.dist.yml (PG merges
# the two). The external programs must be absolute paths: PreTeXt starts
# the PG helper with a minimal environment that has no PATH, and the TeX
# binaries live in TinyTeX's /root/bin, not /usr/bin as the defaults assume.
&& printf '%s\n' \
    '# Settings for the pretext-docker image; everything else comes from pg_config.dist.yml.' \
    'directories:' \
    "  root: ${PG_ROOT}" \
    'externalPrograms:' \
    '  latex: /root/bin/latex --no-shell-escape' \
    '  latex2pdf: /root/bin/xelatex --no-shell-escape' \
    '  dvisvgm: /root/bin/dvisvgm' \
    '  convert: /usr/bin/convert' \
    > "${PG_ROOT}/conf/pg_config.yml" \
# PG makes the PNG version of each PGtikz.pl / PGlateximage.pl image by
# having ImageMagick read the PDF through Ghostscript. Some ImageMagick
# policies forbid reading PDF; the Guide says to allow it, so patch whichever
# policy file exists (ImageMagick 6 or 7). A lone PDF rule becomes "read";
# PDF is dropped from a combined {PS,PDF,...} rule, which leaves it at the
# default (permitted) rights.
&& for policy in /etc/ImageMagick-6/policy.xml /etc/ImageMagick-7/policy.xml; do \
     if [ -f "$policy" ]; then \
       sed -i -E \
         -e 's|(<policy domain="coder" rights=")none(" pattern="PDF" */>)|\1read\2|' \
         -e 's|(<policy domain="coder" rights="none" pattern="\{[^}]*)PDF,|\1|' \
         -e 's|(<policy domain="coder" rights="none" pattern="\{[^}]*),PDF|\1|' \
         "$policy"; \
     fi; \
   done \
# Prove the PDF -> PNG step works (ImageMagick can read a PDF via
# Ghostscript) rather than trusting the policy edit; fail the build if not.
&& cd /tmp \
&& printf '%%!PS\n/Helvetica findfont 24 scalefont setfont 72 72 moveto (pg) show showpage\n' > policy-test.ps \
&& gs -q -dBATCH -dNOPAUSE -sDEVICE=pdfwrite -sOutputFile=policy-test.pdf policy-test.ps \
&& convert -density 72 policy-test.pdf policy-test.png \
&& test -s policy-test.png \
&& rm -f policy-test.ps policy-test.pdf policy-test.png \
# Confirm that the helper script PreTeXt uses for local processing
# (script/webwork/pg-ptx.pl, bundled inside the CLI's copy of core) finds
# every perl module it needs against this pg.
&& python3 -c 'import importlib.resources, sys, zipfile; from pretext import CORE_COMMIT; z = zipfile.ZipFile(importlib.resources.files("pretext.resources") / "core.zip"); sys.stdout.buffer.write(z.read(f"pretext-{CORE_COMMIT}/script/webwork/pg-ptx.pl"))' > /tmp/pg-ptx.pl \
&& perl -c /tmp/pg-ptx.pl \
&& rm -f /tmp/pg-ptx.pl \
&& cd /
