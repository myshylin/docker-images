FROM ubuntu:18.04

ARG PYTHON_VERSION="3.7"
ARG DPREP_INDEX_URL=""

ENV PATH /miniconda/bin:$PATH

LABEL maintainer="Nick <myshylin@microsoft.com>"

EXPOSE 3056
EXPOSE 3000

RUN apt-get -y update && apt-get -y upgrade \
    && apt-get -y install wget fuse \
    && mkdir -p /home/downloads \
    && wget -O /home/downloads/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && chmod +x /home/downloads/miniconda.sh \
    && /home/downloads/miniconda.sh -p /miniconda -b \
    && wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && apt-get -y update \
    && apt-get -y install blobfuse \
    && rm -f /home/downloads/miniconda.sh \
    && rm -rf /home/downloads \
    && echo ". /miniconda/etc/profile.d/conda.sh" >> ~/.bashrc \
    && conda update -y -n base -c defaults conda \
    && conda init --all \
    && conda create -y --name dprep python=${PYTHON_VERSION} \
    && echo "conda activate dprep" >> ~/.bashrc \
    && apt-get -y clean \
    && /bin/bash -c ". activate dprep \
    && pip install numpy pandas pyarrow jupyter azure-cli \
    && pip install azureml-core azureml-dataprep[fuse,pandas] \
    && [ -z \"$DPREP_INDEX_URL\" ] && echo \"Using public pypi for dprep\" || pip install --index-url ${DPREP_INDEX_URL} --pre --no-deps --force-reinstall --no-cache-dir azureml-dataprep" \
    && mkdir -p /home/src && mkdir -p /home/testfiles

COPY ./src /home/src
COPY ./testfiles /home/testfiles
COPY ./startup.sh /home

WORKDIR /home

CMD /bin/bash /home/startup.sh