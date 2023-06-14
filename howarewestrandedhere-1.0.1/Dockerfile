FROM ubuntu:latest

WORKDIR /usr/src

RUN apt-get -qq update && apt-get -y upgrade \
	&& apt-get install -y python3-pip python3-dev build-essential locales python3.10-venv wget zip default-jre \
    && apt-get clean \
    && apt-get purge \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install kallisto 0.44
RUN wget -q https://github.com/pachterlab/kallisto/releases/download/v0.44.0/kallisto_linux-v0.44.0.tar.gz -O kallisto-0.44.tar.gz \
    && tar -xvzf kallisto-0.44.tar.gz \
    && ln -s /usr/src/kallisto_linux-v0.44.0/kallisto /usr/local/bin/ \
    && rm kallisto-0.44.tar.gz

# install howarewestrandedhere
RUN pip3 install virtualenv  \
    && virtualenv howarewestrandedhere-venv \
    && howarewestrandedhere-venv/bin/pip install --upgrade pip \
    && howarewestrandedhere-venv/bin/pip install how_are_we_stranded_here \
    && virtualenv rseqc-venv \
    && rseqc-venv/bin/pip install --upgrade pip \
    && rseqc-venv/bin/pip install RSeQC

RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8 
ENV PATH=$PATH:/usr/src/kallisto_linux-v0.44.0/:/usr/src/howarewestrandedhere-venv/bin/:/usr/src/rseqc-venv/bin/
