#
# A Docker image containing a number of core models from Open Source Brain
#
# This image extends the basic "simulationx" image by adding support
# for libraries required for OSB models
#

# Version from Dockerhub
FROM opensourcebrain/simulation:osb-v0.7.1

# Local build
#FROM osb                                   

MAINTAINER p.gleeson@gmail.com

USER root


# Update to latest OMV

RUN ls -alt
RUN cd osb-model-validation/utilities/ && git pull && cd -


# Clone core set of models

RUN osb-model-validation/utilities/getcoreosbprojects.sh

# Pre-compile mod files for some models

RUN cd coreprojects/SmithEtAl2013-L23DendriticSpikes/NEURON/test && nrnivmodl ../mod.files && cd -

RUN cd coreprojects/ca1/NeuroML2 && nrnivmodl .. && cd -

RUN cd coreprojects/SadehEtAl2017-InhibitionStabilizedNetworks && ./make_tests.sh && cd -


RUN cd coreprojects/PotjansDiesmann2014/ && ./make_tests.sh && cd -



RUN echo "Built OSB models Docker image!"



