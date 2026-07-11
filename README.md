# Docker image for PreTeXt

Two versions of the PreTeXt-Docker image are available: `base` and `full`. The `base` image is smaller and contains only the essential tools needed to build PreTeXt documents. The `full` image includes additional tools and libraries for more advanced features.  These are published on Docker Hub at [https://hub.docker.com/r/pretextbook/pretext](https://hub.docker.com/r/pretextbook/pretext) as `pretext/pretext` and `pretextbook/pretext-full`.  You can specify a version tag for either image, such as `pretextbook/pretext-full:1.12`, or use `pretextbook/pretext-full:latest` to get the most recent version.  

In the base image, all the software to build basic PreTeXt documents, including python, nodejs, prefigure, and of course the PreTeXt-CLI is included.  You also get a small LaTeX installation, which should be enough to build most PDF outputs and use tikz images as in a `<latex-image>` element.

The full image adds asymptote and sagemath in case you want to use these programs to generate images.

## Building and testing the full image locally

To build the `full` image from source (from the repository root, since the build context needs both `base/` and `full/`):

```
docker build -f full/Dockerfile -t pretext-full:test .
```

This pulls the published `pretextbook/pretext:latest` base image by default (`full/Dockerfile` builds `FROM pretextbook/pretext:${BASE_IMAGE_TAG}`). To test against a `base/Dockerfile` you've also modified locally but not yet published, build and tag `base` under that same repository name first, then point `full` at the tag:

```
docker build -f base/Dockerfile -t pretextbook/pretext:test .
docker build -f full/Dockerfile --build-arg BASE_IMAGE_TAG=test -t pretext-full:test .
```

### Testing Asymptote

Asymptote is built from source so it gets Vulkan-based 3D rendering (see the comments in `full/Dockerfile` for why). `asy -version` should list `Vulkan` under `ENABLED OPTIONS`, not `DISABLED OPTIONS`:

```
docker run --rm pretext-full:test asy -version
```

To confirm it actually renders (not just that the binary reports the right options), render a 2D and a 3D test file inside the container:

```
docker run --rm pretext-full:test bash -c '
cd /tmp
cat > test2d.asy << "EOF"
size(200);
draw(unitcircle);
fill(shift(2,0)*unitsquare, lightblue);
EOF
cat > test3d.asy << "EOF"
import three;
size(200);
currentprojection = perspective(5,4,3);
draw(unitsphere, lightblue);
draw(unitcube);
EOF
asy -f pdf test2d.asy && ls -la test2d.pdf
asy -f png -render=4 test3d.asy && ls -la test3d.png
'
```

Both commands should report non-trivial file sizes (a few KB or more); the 3D render exercises Mesa's software Vulkan renderer (lavapipe), which is what makes this work in Codespaces/CI containers with no real GPU.

### Testing Sage

```
docker run --rm pretext-full:test sage --version
docker run --rm pretext-full:test sage -c "print(factor(2024)); print(integrate(sin(x)^2, x))"
```

The second command should print `2^3 * 11 * 23` and `1/2*x - 1/4*sin(2*x)` — confirming Sage isn't just launching, but actually computing.

### Testing on ARM64 without ARM hardware

Docker on Linux can emulate `arm64` via QEMU. One-time setup:

```
docker run --privileged --rm tonistiigi/binfmt --install arm64
```

Then build/run with `--platform linux/arm64`, e.g.:

```
docker build --platform linux/arm64 -f full/Dockerfile -t pretext-full:arm64-test .
docker run --rm --platform linux/arm64 pretext-full:arm64-test sage -c "print(2+2)"
```

Emulated builds are much slower than native (QEMU has to emulate every instruction), so this is best for occasional manual verification rather than routine use. For CI, prefer GitHub's native `ubuntu-24.04-arm` hosted runners, which build and run natively.

