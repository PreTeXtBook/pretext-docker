FROM oscarlevin/pretext:small

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN wget -O Miniforge3.sh "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh" \
&& bash Miniforge3.sh -b -p "/conda" \
&& rm Miniforge3.sh

SHELL ["/bin/bash", "-i", "-c"]

ENV PATH="/conda/bin:${PATH}"

RUN mamba create --yes -n sage sage python=3.12

RUN conda config --set auto_activate_base false

RUN conda init

RUN echo 'conda activate sage' >> ~/.bashrc

RUN pip install pretext[homepage,prefigure] --only-binary {greenlet}  --break-system-packages
