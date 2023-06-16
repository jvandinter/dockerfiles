FROM ubuntu:latest

RUN apt-get -qq update && apt-get -y upgrade \
	&& apt-get install -y python3-pip python3-dev build-essential locales python3.10-venv default-jre \
    && apt-get clean \
    && apt-get purge \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install pytorch
RUN RUN pip3 install virtualenv  \
    && virtualenv dxtools-venv \
    && dxtools-venv/bin/pip install --upgrade pip \
    && dxtools-venv/bin/pip pip install --upgrade 'git+https://github.com/dnanexus/dx-toolkit.git@master#egg=dxpy&subdirectory=src/python'

ENV PATH=$PATH:/usr/src/dxtools-venv/bin/
ENV LC_ALL="en_US.UTF-8"
ENV LC_CTYPE="en_US.UTF-8"
ENV PYTHONIOENCODING="UTF-8" 
