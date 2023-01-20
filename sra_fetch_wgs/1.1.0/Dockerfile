FROM ubuntu:focal as app
ENV DEBIAN_FRONTEND=noninteractive

LABEL base.image="ubuntu:focal"
LABEL container.version="1.0.0"
LABEL software="sra_fetch_eutils"
LABEL software.version="1.1.0"
LABEL description="sra_fetch with wsg accession compatibility"

# run update and install wget curl perl libs
RUN apt update && apt install -y wget curl libssl-dev build-essential libio-socket-ssl-perl libxml-simple-perl python3 python3-pip python3-setuptools

# download sra-toolkit
RUN wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/3.0.2/sratoolkit.3.0.2-ubuntu64.tar.gz &&\
   tar -xzf sratoolkit.3.0.2-ubuntu64.tar.gz &&\
   rm -r sratoolkit.3.0.2-ubuntu64.tar.gz &&\
   mkdir /data

RUN sh -c "$(curl -fsSL ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/install-edirect.sh)"

RUN wget https://github.com/rvalieris/parallel-fastq-dump/archive/refs/tags/0.6.7.tar.gz &&\
   tar -xvzf 0.6.7.tar.gz && \
   rm 0.6.7.tar.gz && \
   cd parallel-fastq-dump-0.6.7 && \
   pip3 install .

ENV PATH="/root/edirect:/sratoolkit.3.0.2-ubuntu64/bin:${PATH}"

WORKDIR /data

FROM app as test

RUN esearch -db sra -query PNUSAS196151 | efetch -format docsum | xtract -pattern DocumentSummary -element Run@acc | tr '\t' '\n'