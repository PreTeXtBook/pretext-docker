FROM debian:bookworm-slim

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
                    git \
                    python3 \
                    python3-pip \
                    npm \
                    python3-dev \
                    python3-louis \
                    python3-venv \
                    libcairo2-dev \
                    librsvg2-bin \
                    wget \
                    gcc \
&& apt-get autoremove -y \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* \
&& rm -rf /tmp/* /var/tmp/*

ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

RUN pip install pip --upgrade \
&& pip install pretext[homepage,prefigure] codechat-server \
&& prefig init \
&& playwright install-deps \
&& playwright install chromium \
&& echo '/usr/lib/python3/dist-packages' > /usr/local/lib/python3.11/dist-packages/louis.pth \
&& echo '/usr/lib/python3/dist-packages' > /opt/venv/lib/python3.11/site-packages/louis.pth \
&& prefig init \
