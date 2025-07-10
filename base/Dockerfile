FROM debian:bookworm-slim

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
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
                    librsvg2-bin \
                    libssl-dev \
                    ca-certificates \
                    gcc \
                    build-essential \
&& curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash \
&& \. "$HOME/.nvm/nvm.sh" \
&& nvm install 24 \
&& pip install pip --upgrade --no-cache-dir \
&& pip install pretext[all] codechat-server --prefer-binary --no-cache-dir \
&& prefig init \
&& playwright install-deps \
&& playwright install chromium \
&& echo '/usr/lib/python3/dist-packages' > /usr/local/lib/python3.11/dist-packages/louis.pth \
&& echo '/usr/lib/python3/dist-packages' > /opt/venv/lib/python3.11/site-packages/louis.pth \
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

# If the platform is linux/arm64, we also need to install default-jre
ARG TARGETPLATFORM

RUN if [ "$TARGETPLATFORM" = "linux/arm64" ]; then \
    apt-get update && \
    apt-get install -y --no-install-recommends default-jre && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*; \
fi

# make shell prompt pretty
RUN echo 'export PS1="\[\e[1;32m\]\u@\h:\[\e[1;34m\]\w \[\e[0m\]\$ "' >> ~/.bashrc

# Set TMPDIR to avoid permission issues on ARM architectures
ENV TMPDIR=/

RUN wget -O TinyTex.sh "https://yihui.org/tinytex/install-bin-unix.sh" \
&& bash TinyTex.sh \
&& rm TinyTex.sh

#TinyTeX installs links to the executable in /root/bin, so we need to add that to the PATH
ENV PATH="/root/bin:$PATH"

SHELL ["/bin/bash", "-i", "-c"]

RUN tlmgr install adjustbox amscdx bold-extra braket bussproofs cancel carlisle cases chessfss circuitikz colortbl enumitem extpfeil fontawesome5 fontaxes gensymb hobby imakeidx jknapltx kastrup lambda-lists listings listingsutf8 makeindex marvosym mathalpha mathtools menukeys mhchem microtype musicography newpx newtx nicematrix pdfcol pdfpages pdflscape pgfplots phaistos physics polyglossia pstricks realscripts relsize siunitx skak skaknew smartdiagram snapshot stmaryrd tcolorbox tikz-3dplot tikz-cd tikzfill titlesec txfonts ulem upquote was xfrac xltxtra xpatch xstring \
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

