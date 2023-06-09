FROM ubuntu:latest

WORKDIR /usr/src

RUN apt-get -qq update && apt-get -y upgrade \
	&& apt-get install -y python3-pip python3-dev build-essential \
    && apt install -y python3.10-venv \
    && apt-get install -y wget zip default-jre \
    && apt-get clean \
    && apt-get purge \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install kallisto 0.44
RUN wget -q https://github.com/pachterlab/kallisto/releases/download/v0.44.0/kallisto_linux-v0.44.0.tar.gz -O kallisto-0.44.tar.gz \
    && tar -xvzf kallisto-0.44.tar.gz \
    && rm kallisto-0.44.tar.gz

# install howarewestrandedhere
RUN pip3 install virtualenv  \
    && virtualenv howarewestrandedhere-venv \
    && howarewestrandedhere-venv/bin/pip install --upgrade pip \
    && howarewestrandedhere-venv/bin/pip install how_are_we_stranded_here

ENV PATH=$PATH:/usr/src/kallisto/:/usr/src/howarewestrandedhere-venv/bin/
