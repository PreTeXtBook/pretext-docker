FROM oscarlevin/pretext:lite

RUN wget -qO- "https://yihui.org/tinytex/install-bin-unix.sh" | sh \
&& tlmgr install adjustbox amscdx bold-extra braket bussproofs cancel carlisle cases chessfss circuitikz colortbl enumitem extpfeil fontawesome5 fontaxes gensymb imakeidx jknapltx kastrup lambda-lists listings listingsutf8 makeindex marvosym mathalpha mathtools menukeys mhchem microtype musicography newpx newtx nicematrix pdfcol pdfpages pdflscape pgfplots phaistos physics polyglossia pstricks realscripts relsize siunitx skak skaknew smartdiagram snapshot stmaryrd tcolorbox tikz-cd tikzfill titlesec txfonts ulem upquote was xfrac xltxtra xpatch xstring \
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

#RUN apt update && apt install -y --no-install-recommends \
#    texlive \
#    texlive-plain-generic \
#    texlive-science \
#    texlive-xetex \
#    texlive-fonts-extra \
#    ghostscript \
#    asymptote \
#&& apt-get clean && rm -rf /var/lib/apt/lists/*
