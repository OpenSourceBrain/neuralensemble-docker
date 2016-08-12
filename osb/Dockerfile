#
# A Docker image for running neuronal models from Open Source Brain
#
# This image extends the basic "simulationx" image by adding support
# for libraries required for OSB models
#

FROM neuralensemble/simulationx:py2
MAINTAINER p.gleeson@gmail.com

USER root

RUN apt-get update 
RUN apt-get install -y default-jdk python-tk python-lxml octave
RUN apt-get install -y htop

RUN /bin/bash -c "source ~/env/neurosci/bin/activate"


# Install NeuroML Python libraries

RUN pip install git+https://github.com/NeuralEnsemble/libNeuroML.git@development
RUN pip install git+https://github.com/NeuroML/pyNeuroML


# Install OMV

RUN git clone https://github.com/OpenSourceBrain/osb-model-validation.git
RUN cd osb-model-validation; python setup.py install
RUN sed -i -e s/'\/usr\/bin\/python'/'\/home\/docker\/env\/neurosci\/bin\/python'/g /usr/local/bin/omv


# Install jNeuroML

RUN omv install jNeuroML
ENV JNML_HOME=$HOME/jnml/jNeuroMLJar
ENV PATH=$PATH:$JNML_HOME



