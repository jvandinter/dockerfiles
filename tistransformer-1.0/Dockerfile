FROM ubuntu:latest

WORKDIR /usr/src

RUN apt-get -qq update && apt-get -y upgrade \
	&& apt-get install -y python3-pip python3-dev build-essential locales python3.10-venv default-jre \
    && apt-get clean \
    && apt-get purge \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install pytorch
RUN RUN pip3 install virtualenv  \
    && virtualenv pytorch-venv \
    && pytorch-venv/bin/pip install --upgrade pip \
    && pytorch-venv/bin/pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu \
    && virtualenv tistransformer-venv \
    && tistransformer-venv/bin/pip install --upgrade pip \
    && tistransformer-venv/bin/pip install tistransformer

# install tistransformer

RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8
ENV PATH=$PATH:/usr/src/pytorch-venv/bin/:/usr/src/tistransformer-venv/bin/
