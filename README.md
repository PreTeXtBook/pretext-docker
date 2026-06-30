# Docker image for PreTeXt

Two versions of the PreTeXt-Docker image are available: `base` and `full`. The `base` image is smaller and contains only the essential tools needed to build PreTeXt documents. The `full` image includes additional tools and libraries for more advanced features.  These are published on Docker Hub at [https://hub.docker.com/r/pretextbook/pretext](https://hub.docker.com/r/pretextbook/pretext) as `pretext/pretext` and `pretextbook/pretext-full`.  You can specify a version tag for either image, such as `pretextbook/pretext-full:1.12`, or use `pretextbook/pretext-full:latest` to get the most recent version.  

In the base image, all the software to build basic PreTeXt documents, including python, nodejs, prefigure, and of course the PreTeXt-CLI is included.  You also get a small LaTeX installation, which should be enough to build most PDF outputs and use tikz images as in a `<latex-image>` element.

The full image adds asymptote and sagemath in case you want to use these programs to generate images.

