FROM rocker/r-ver:4.1.2

RUN apt-get update \
  && apt-get install --no-install-recommends -y \
    build-essential wget \ 
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
    libglpk-dev \
    && wget https://github.com/jgm/pandoc/releases/download/2.11.4/pandoc-2.11.4-linux-amd64.tar.gz \
    && mkdir '/usr/src/pandoc' \
	  && tar xvzf pandoc-2.11.4-linux-amd64.tar.gz --strip-components 1 -C '/usr/src/pandoc' \
	  && rm pandoc-2.11.4-linux-amd64.tar.gz

RUN R -e "install.packages('remotes');" \
  && R -e "install.packages('BiocManager')" \
  && R -e "remotes::install_version('rmarkdown', '2.11',repos = c('http://cran.us.r-project.org')); if (!library(rmarkdown, logical.return=T)) quit(status=10)" \
  && R -e "remotes::install_github(repo = 'damhof/RiboseQC'); if (!library(RiboseQC, logical.return=T)) quit(status=10)" \
  && R -e "remotes::install_github(repo = 'damhof/ORFquant'); if (!library(ORFquant, logical.return=T)) quit(status=10)" \
  && R -e "BiocManager::install('rtracklayer'); if (!library(rtracklayer, logical.return=T)) quit(status=10)"
