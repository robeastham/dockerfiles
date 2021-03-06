## Start from my alpine-pandoc image

FROM conoria/alpine-pandoc

MAINTAINER Conor I. Anderson <conor.anderson@mail.utoronto.ca>

## Cabal install only works on alpine:latest, but the R packages are in alpine:edge

RUN echo http://dl-cdn.alpinelinux.org/alpine/edge/main >> /etc/apk/repositories && \
    echo http://dl-cdn.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories

## Install R and a few build tools, patch and install httpuv then install bookdown and clean-up

RUN apk update && apk add --no-cache R R-dev curl openssl-dev curl-dev gcc g++ git coreutils libxml2-dev && \
    R -q -e "install.packages('Rcpp', repo='https://cran.rstudio.com')" && \
    curl -L -O https://cran.r-project.org/src/contrib/httpuv_1.3.3.tar.gz \
	&& tar xvf httpuv_1.3.3.tar.gz \
	&& sed -i -e 's/__USE_MISC/_GNU_SOURCE/g' httpuv/src/libuv/src/fs-poll.c \
	&& tar -cf httpuv_1.3.3.tar.gz httpuv \
	&& R CMD INSTALL httpuv_1.3.3.tar.gz && \
    R -q -e 'install.packages("bookdown", repo="https://cran.rstudio.com/")' && \
    rm -rf httpuv_1.3.3.tar.gz httpuv /tmp/*

CMD ["R"]
