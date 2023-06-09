# Base ##################################################################
ARG base_image_version=jammy
FROM ubuntu:$base_image_version AS ubuntu-builder-base
WORKDIR /app

## Not sure why we have to repeat this, but apparently the arg is no
## longer set after FROM.
ARG base_image_version=jammy
ARG lua_version=5.4
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -q --no-allow-insecure-repositories update \
  && if [ $base_image_version = "focal" ]; then \
         apt-get install --assume-yes --no-install-recommends \
                 software-properties-common \
         && add-apt-repository ppa:hvr/ghc \
         && apt-get install --assume-yes --no-install-recommends \
                    ghc-8.8.4=\* \
                    cabal-install-3.0=\* \
         && ln -s /opt/ghc/bin/ghc /usr/bin/ghc \
         && ln -s /opt/cabal/bin/cabal /usr/bin/cabal; \
     else \
         apt-get install --assume-yes --no-install-recommends \
                 ghc=* \
                 cabal-install=*; \
     fi \
  && apt-get install --assume-yes --no-install-recommends \
       build-essential=* \
       ca-certificates=* \
       curl=* \
       fakeroot=* \
       git \
       libgmp-dev=2:6.* \
       liblua$lua_version-dev=* \
       pkg-config=* \
       zlib1g-dev=1:1.2.11.* \
  && rm -rf /var/lib/apt/lists/*

COPY cabal.root.config /root/.cabal/config
RUN cabal --version \
  && ghc --version \
  && cabal v2-update

# Builder ###############################################################
FROM ubuntu-builder-base as ubuntu-builder
ARG pandoc_commit=main
RUN git clone --branch=$pandoc_commit --depth=1 --quiet \
  https://github.com/jgm/pandoc /usr/src/pandoc

# Remove the settings that ship with pandoc.
RUN rm -f cabal.project

COPY ./ubuntu/freeze/pandoc-$pandoc_commit.project.freeze \
     /usr/src/pandoc/cabal.project.freeze

# Install Haskell dependencies
WORKDIR /usr/src/pandoc
# Add pandoc-crossref to project
ARG without_crossref=
ARG extra_packages="pandoc-cli pandoc-crossref"
RUN test -n "$without_crossref" || \
    printf "extra-packages: pandoc-crossref\n" > cabal.project.local;

# Build pandoc and pandoc-crossref. The `allow-newer` is required for
# when pandoc-crossref has not been updated yet, but we want to build
# anyway.
RUN cabal v2-update \
  && cabal v2-build \
      --allow-newer 'lib:pandoc' \
      --disable-tests \
      --disable-bench \
      --jobs \
      . $extra_packages

# Cabal's exec stripping doesn't seem to work reliably, let's do it here.
RUN find dist-newstyle \
         -name 'pandoc*' -type f -perm -u+x \
         -exec strip '{}' ';' \
         -exec cp '{}' /usr/local/bin/ ';'

# Minimal ###############################################################
FROM ubuntu:$base_image_version AS ubuntu-minimal
ARG pandoc_version=edge
ARG lua_version=5.4
LABEL maintainer='Albert Krewinkel <albert+pandoc@zeitkraut.de>'
LABEL org.pandoc.maintainer='Albert Krewinkel <albert+pandoc@zeitkraut.de>'
LABEL org.pandoc.author "John MacFarlane"
LABEL org.pandoc.version "$pandoc_version"

WORKDIR /data
ENTRYPOINT ["/usr/local/bin/pandoc"]

COPY --from=ubuntu-builder \
  /usr/local/bin/pandoc \
  /usr/local/bin/

# Reinstall any system packages required for runtime.
RUN apt-get -q --no-allow-insecure-repositories update \
  && DEBIAN_FRONTEND=noninteractive \
     apt-get install --assume-yes --no-install-recommends \
       ca-certificates=\* \
       liblua$lua_version-0=\* \
       lua-lpeg=\* \
       libatomic1=\* \
       libgmp10=\* \
       libpcre3=\* \
       libyaml-0-2=\* \
       zlib1g=\* \
  && rm -rf /var/lib/apt/lists/*

FROM rocker/r-ver:4.1.2

RUN apt-get update
RUN apt-get install -y \
    build-essential \ 
    libcurl4-gnutls-dev \
    libxml2-dev \
    libssl-dev \
    libxml2 \
    libxt6 \
    zlib1g-dev \
    libbz2-dev \
    liblzma-dev \
    libpcre3-dev \
    libicu-dev \
    libjpeg-dev \
    libpng-dev \
    libxml2-dev \
    libglpk-dev

RUN R -e "install.packages('remotes');" \
  && R -e "install.packages('BiocManager');" \
  && R -e "BiocManager::install('RiboseQC'); if (!library(RiboseQC, logical.return=T)) quit(status=10)" \
  && R -e "BiocManager::install('ORFquant'); if (!library(ORFquant, logical.return=T)) quit(status=10)"
