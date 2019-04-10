#
# OSB based on OMV installs
#

FROM neurodebian:xenial
MAINTAINER p.gleeson@ucl.ac.uk

ENV DEBIAN_FRONTEND noninteractive
ENV LANG=C.UTF-8

RUN apt-get update; apt-get install -y automake libtool build-essential openmpi-bin libopenmpi-dev git vim  \
                       wget python libpython-dev libncurses5-dev libreadline-dev libgsl0-dev cython \
                       python-pip python-numpy python-scipy python-matplotlib python-jinja2 python-mock \
                       python-virtualenv ipython python-httplib2 python-docutils python-yaml \
                       subversion python-mpi4py python-tables cmake

RUN useradd -ms /bin/bash docker
USER docker
ENV HOME=/home/docker
RUN mkdir $HOME/env; mkdir $HOME/packages

ENV VENV=$HOME/env/neurosci

RUN virtualenv --system-site-packages $VENV

RUN $VENV/bin/pip install --upgrade pip
RUN $VENV/bin/pip install parameters quantities neo django django-tagging future hgapi gitpython sumatra
RUN $VENV/bin/pip install --upgrade nose ipython

USER root

RUN apt-get update 
RUN apt-get install -y default-jdk python-tk python-lxml octave maven
RUN apt-get install -y htop

RUN /bin/bash -c "source ~/env/neurosci/bin/activate"


# Install latest jNeuroML

RUN cd /home/docker && git clone https://github.com/NeuroML/jNeuroML.git && cd jNeuroML && python getNeuroML.py development


# Install NeuroML Python libraries

RUN pip install git+https://github.com/NeuralEnsemble/libNeuroML.git@development
RUN pip install git+https://github.com/NeuroML/pyNeuroML


# Install OMV

RUN git clone https://github.com/OpenSourceBrain/osb-model-validation.git
RUN cd osb-model-validation && python setup.py install && cd -
RUN sed -i -e s/'\/usr\/bin\/python'/'\/home\/docker\/env\/neurosci\/bin\/python'/g /usr/local/bin/omv


# Install Brian2

RUN omv install Brian2


# Update Brian1

RUN git clone https://github.com/brian-team/brian.git
RUN cd brian && $HOME/env/neurosci/bin/python setup.py install && cd -


USER docker

ENV JNML_HOME=$HOME/jNeuroML
ENV LEMS_HOME=$HOME/jLEMS
ENV PATH=$PATH:$JNML_HOME:$LEMS_HOME

# Some aliases

RUN echo '\n\nalias cd..="cd .."\nalias h=history\nalias ll="ls -alt"' >> ~/.bashrc


RUN echo "Built the main OSB (OMV based) Docker image!"


