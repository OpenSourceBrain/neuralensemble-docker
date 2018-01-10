#
# A Docker image for running neuronal models from Open Source Brain
#
# This image extends the basic "simulationx" image by adding support
# for libraries required for OSB models
#

# Use tagged version of neuralensemble/simulationx, e.g. Python 2, PyNN 0.9.1
FROM neuralensemble/simulationx:py2_pynn091
MAINTAINER p.gleeson@gmail.com

USER root

RUN apt-get update 
RUN apt-get install -y default-jdk python-tk python-lxml octave maven
RUN apt-get install -y htop

RUN /bin/bash -c "source ~/env/neurosci/bin/activate"


# Install NeuroML Python libraries

RUN pip install git+https://github.com/NeuralEnsemble/libNeuroML.git@development
RUN pip install git+https://github.com/NeuroML/pyNeuroML


# Install OMV

RUN git clone https://github.com/OpenSourceBrain/osb-model-validation.git
RUN cd osb-model-validation && python setup.py install && cd -
RUN sed -i -e s/'\/usr\/bin\/python'/'\/home\/docker\/env\/neurosci\/bin\/python'/g /usr/local/bin/omv


# Install jNeuroML, PyLEMS & NeuroML2

RUN omv install jNeuroML
ENV JNML_HOME=$HOME/jnml/jNeuroMLJar

RUN omv install jLEMS
ENV LEMS_HOME=$HOME/jLEMS

RUN omv install PyLEMS_NeuroML2

ENV PATH=$PATH:$JNML_HOME:$LEMS_HOME


# Set up NEURON (should be updated in neuralensemble/simulationx:py2...)

ENV PATH=$PATH:$HOME/env/neurosci/x86_64/bin
ENV NEURON_HOME=$HOME/env/neurosci/x86_64


# Set up NEST

ENV NEST_INSTALL_DIR=$HOME/env/neurosci


# Install NetPyNE

RUN omv install NetPyNE


# Install GENESIS

RUN apt-get install -y bison flex
RUN omv install genesis
ENV PATH=$PATH:$HOME/genesis/genesis2.4gamma-master/src


# Install OSB_API

RUN git clone https://github.com/OpenSourceBrain/OSB_API.git
RUN cd OSB_API/python; python setup.py install; cd -


# Install Brian2

RUN omv install Brian2


# Install neuroConstruct

RUN git clone git://github.com/NeuralEnsemble/neuroConstruct.git
RUN cd neuroConstruct; ./updatenC.sh; ./nC.sh -make; cd -


# Install Moose

RUN omv install Moose


# Update PyNN

RUN git clone https://github.com/NeuralEnsemble/PyNN.git
RUN cd PyNN; $HOME/env/neurosci/bin/python setup.py install; cd -


# Update Brian

RUN git clone https://github.com/brian-team/brian.git
RUN cd brian; $HOME/env/neurosci/bin/python setup.py install; cd -

# Some aliases

RUN echo '\n\nalias cd..="cd .."\nalias h=history\nalias ll="ls -alt"' >> ~/.bashrc

RUN echo "Built the main OSB Docker image!"



