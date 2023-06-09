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

RUN apt-get install python3-pip \
    && pip3 install virtualenv  \
    && virtualenv cutadapt-venv \
    && cutadapt-venv/bin/pip install --upgrade pip \
    && cutadapt-venv/bin/pip install cutadapt==3.4

#Get fastqc
RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip \
	&& unzip fastqc_v0.11.9.zip \
	&& rm fastqc_v0.11.9.zip \
	&& chmod a+x FastQC/fastqc

#Get Trim Galore
RUN wget https://github.com/FelixKrueger/TrimGalore/archive/refs/tags/0.6.6.tar.gz -O trimgalore.tar.gz \
	&& tar xvzf trimgalore.tar.gz \
    && rm trimgalore.tar.gz

ENV PATH=$PATH:/usr/src/FastQC/:/usr/src/TrimGalore-0.6.6/:/usr/src/cutadapt-venv/bin/
