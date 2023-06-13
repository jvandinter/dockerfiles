FROM ubuntu:latest

WORKDIR /usr/src

RUN apt-get -qq update && apt-get -y upgrade \
	&& apt-get install -y python3-pip python3-dev build-essential \
    && apt install -y python3.10-venv \
    && apt-get install -y wget zip default-jre \
    && apt-get clean \
    && apt-get purge \
    && export LC_ALL=en_US.UTF-8 \
    && export LANG=en_US.UTF-8 \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN wget https://github.com/jgm/pandoc/releases/download/3.1.3/pandoc-3.1.3-linux-amd64.tar.gz -O pandoc.tar.gz && \
   tar -xvfz pandoc.tar.gz && \
   rm pandoc.tar.gz

FROM rocker/r-ver:4.1.2

RUN apt-get update \
  && apt-get install --no-install-recommends -y \
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
  && R -e "install.packages('BiocManager')" \
  && R -e "remotes::install_version('rmarkdown', '2.11',repos = c('http://cran.us.r-project.org')); if (!library(rmarkdown, logical.return=T)) quit(status=10)" \
  && R -e "remotes::install_github(repo = 'damhof/RiboseQC'); if (!library(RiboseQC, logical.return=T)) quit(status=10)" \
  && R -e "remotes::install_github(repo = 'damhof/ORFquant'); if (!library(ORFquant, logical.return=T)) quit(status=10)" \
  && R -e "BiocManager::install('rtracklayer'); if (!library(rtracklayer, logical.return=T)) quit(status=10)"
