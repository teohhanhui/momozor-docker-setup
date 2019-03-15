FROM ubuntu:18.04
MAINTAINER Momozor

# Install dependencies from Debian repositories
RUN apt-get update && apt-get install -y make wget bzip2 tar && apt-get clean

# Install SBCL from the tarball binaries.
RUN wget -c http://prdownloads.sourceforge.net/sbcl/sbcl-1.5.0-x86-64-linux-binary.tar.bz2
RUN mkdir /tmp/sbcl
RUN mv sbcl-1.5.0-x86-64-linux-binary.tar.bz2  /tmp 
RUN tar jxvf /tmp/sbcl-1.5.0-x86-64-linux-binary.tar.bz2 -C /tmp/sbcl/ 
RUN cd /tmp/sbcl/sbcl-1.5.0-x86-64-linux && sh install.sh
RUN cd /tmp  && rm -rf /tmp/sbcl/
