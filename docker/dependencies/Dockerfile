# docker build --build-arg COQ_TAG=8.16 -t smtcoq/smtcoq:latest-coq-8.16 .
# docker run -it smtcoq/smtcoq:latest-coq-8.16
# docker push smtcoq/smtcoq:latest-coq-8.16
# docker run -w /home/coq/smtcoq -v "$(pwd):/home/coq/smtcoq" -it smtcoq/smtcoq:latest-coq-8.16

############################################################
# Dockerfile to build SMTCoq dependencies
############################################################

ARG COQ_TAG="dev"
FROM coqorg/coq:${COQ_TAG}

USER coq

################### SYSTEM PREREQUISITES ###################

RUN sudo apt-get -y update && \
    sudo apt-get -y install wget atool bison flex

#################### OPAM PREREQUISITES ####################

WORKDIR /home/coq

RUN opam repo add coq-released https://coq.inria.fr/opam/released && \
    opam update && \
    opam upgrade -y && \
    opam install -y num && \
    opam install -y coq-trakt

###################### COMPILE ZChaff ######################

WORKDIR /home/coq

RUN wget http://www.princeton.edu/~chaff/zchaff/zchaff.2008.10.12.zip && \
    aunpack zchaff.2008.10.12.zip && \
    cd zchaff && \
    sed -i -re 's/\/\/ #define VERIFY_ON/#define VERIFY_ON/' zchaff_solver.cpp && \
    make

###################### COMPILE veriT #######################

WORKDIR /home/coq

RUN wget https://www.lri.fr/~keller/Documents-recherche/Smtcoq/veriT9f48a98.tar.gz && \
    aunpack veriT9f48a98.tar.gz && \
    cd veriT9f48a98 && \
    autoconf && \
    ./configure && \
    make

###################### DOWNLOAD CVC4 #######################

WORKDIR /home/coq

RUN wget http://cvc4.cs.stanford.edu/downloads/builds/x86_64-linux-opt/cvc4-1.6-x86_64-linux-opt && \
    chmod ugo+x cvc4-1.6-x86_64-linux-opt

##################### LFSC SIGNATURES ######################

WORKDIR /home/coq

RUN git clone https://github.com/smtcoq/smtcoq.git smtcoq-sig

ENV LFSCSIGS="/home/coq/smtcoq-sig/src/lfsc/tests/signatures/"

ENV DONTSHOWVERIT="yes"

###################### DOWNLOAD cvc5 #######################

WORKDIR /home/coq

RUN wget https://github.com/cvc5/cvc5/releases/download/cvc5-1.0.7/cvc5-Linux && \
    chmod ugo+x cvc5-Linux

######################### BINARIES #########################

WORKDIR /home/coq/bin

RUN ln -s /home/coq/zchaff/zchaff && \
    ln -s /home/coq/veriT9f48a98/veriT && \
    ln -s /home/coq/cvc4-1.6-x86_64-linux-opt cvc4 && \
    ln -s /home/coq/cvc5-Linux cvc5

ENV PATH="/home/coq/bin:${PATH}"

WORKDIR /home/coq
