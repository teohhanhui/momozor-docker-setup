FROM momozor/sbcl
MAINTAINER Momozor

ENV HOME /root

WORKDIR /tmp/
RUN wget http://beta.quicklisp.org/quicklisp.lisp
RUN apt-get update -y
RUN apt-get install git apt-utils make wget bzip2  -y
RUN sbcl 
ADD install.lisp /tmp/install.lisp

RUN sbcl --non-interactive --load install.lisp

WORKDIR /root/quicklisp/local-projects

RUN git clone https://github.com/momozor/FCC-timestamp-microservice timestamp-microservice
CMD cd timestamp-microservice; sbcl --load app.lisp --eval '(ql:quickload :timestamp-microservice)' --eval '(timestamp-microservice:start :port (uiop:getenv "PORT")'

