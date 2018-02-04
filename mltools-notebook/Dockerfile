# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM jupyter/scipy-notebook

LABEL maintainer="Dmitry Prokhorov <dipspb@gmail.com>"

USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python3-dev \
        rsync \
        software-properties-common \
        unrar \
        tcl-dev \
        tk-dev \
        libleptonica-dev \
        libtesseract-dev \
        tesseract-ocr \
    && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy English tessercat ocr data
COPY tessdata/* /usr/share/tesseract-ocr/tessdata/
ENV TESSDATA_PREFIX="/usr/share/tesseract-ocr/"

# Set up our notebook config.
COPY jupyter_notebook_config.py /tmp/
RUN cat jupyter_notebook_config.py >> /etc/jupyter/jupyter_notebook_config.py && \
    fix-permissions /etc/jupyter/

USER $NB_USER

COPY environment-cpu.yml  /tmp/environment.yml
# RUN conda install --yes --file /tmp/requirements.txt -v && \
RUN conda env create -f /tmp/environment.yml -v && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR

# Directory for keeping the MXNet model parameters
RUN mkdir -p /home/$NB_USER/.mxnet/models

# TensorBoard
EXPOSE 6006
# Jupyter
EXPOSE 8888
# Flask Server
EXPOSE 4567

USER $NB_USER

