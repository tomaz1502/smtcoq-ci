# docker build --build-arg COQ_TAG=8.16 -t sniper-coq-8.16 .
# docker run -it sniper-coq-8.16
# docker run -w /home/coq/sniper -v "$(pwd):/home/coq/sniper" -it sniper-coq-8.16

############################################################
# Dockerfile to build Sniper dependencies
############################################################

ARG COQ_TAG="dev"
FROM coqorg/coq:${COQ_TAG}

USER coq

WORKDIR /home/coq

RUN sudo apt-get -y update && \
    sudo apt-get -y install git

RUN git clone -b coq-master https://github.com/tomaz1502/sniper.git

RUN opam update
RUN opam upgrade -y
RUN opam install -y ./sniper --deps-only
