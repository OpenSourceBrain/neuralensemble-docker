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

RUN pip install git+https://github.com/NeuralEnsemble/libNeuroML.git@development
RUN pip install git+https://github.com/NeuroML/pyNeuroML

RUN git clone https://github.com/OpenSourceBrain/osb-model-validation.git
RUN cd osb-model-validation; python setup.py install


