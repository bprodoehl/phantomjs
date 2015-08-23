FROM ubuntu:14.04

RUN apt-get update && apt-get dist-upgrade -y

RUN apt-get install -y gcc g++ git make flex bison gperf ruby perl python

RUN echo "deb http://archive.ubuntu.com/ubuntu/ trusty multiverse" >> /etc/apt/sources.list
RUN echo "deb-src http://archive.ubuntu.com/ubuntu/ trusty multiverse" >> /etc/apt/sources.list
RUN echo "deb http://archive.ubuntu.com/ubuntu/ trusty-updates multiverse" >> /etc/apt/sources.list
RUN echo "deb-src http://archive.ubuntu.com/ubuntu/ trusty-updates multiverse" >> /etc/apt/sources.list
RUN echo "ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true" | debconf-set-selections

RUN apt-get update
RUN apt-get install -y ttf-dejavu-core ttf-indic-fonts ttf-kochi-gothic \
                      ttf-kochi-mincho ttf-mscorefonts-installer xfonts-mathml

# For Ubuntu shared libs
RUN apt-get install -y make gcc g++ nasm curl tar bzip2

#RUN apt-get install -y libsqlite3-dev libfontconfig1-dev \
#                       libicu-dev libfreetype6 libssl-dev libpng-dev \
#                       libjpeg-dev zlib1g-dev

# Extra deps pointed out by WebKit config
#RUN apt-get install -y libxslt1-dev libxml2-dev libwebp-dev libxcomposite-dev \
#                       libxrender-dev


RUN mkdir -p /src
WORKDIR /src

ENV NUM_CORES=8
# zlib
ENV ZLIB_VER=1.2.8
RUN curl -sL http://zlib.net/zlib-${ZLIB_VER}.tar.gz | tar -xz
RUN cd zlib-${ZLIB_VER} && ./configure --prefix=/usr --static && make all -j${NUM_CORES} && make install

# libpng
ENV LIBPNG_VER=1.6.17
RUN curl -sL http://download.sourceforge.net/libpng/libpng-${LIBPNG_VER}.tar.gz | tar -xz
RUN cd libpng-${LIBPNG_VER} && ./configure --prefix=/usr --enable-static --disable-shared && make all -j${NUM_CORES} && make install

# libjpeg
#ENV LIBJPEG_VER=6b
#RUN curl -sL http://download.sourceforge.net/libjpeg/${LIBJPEG_VER}/libjpeg-v${LIBJPEG_VER}.tar.gz | tar -xz
#RUN cd libjpeg && ./configure --prefix=/usr && make all -j${NUM_CORES} && make install

# libjpeg-turbo
ENV LIBJPEG_TURBO_VER=1.4.0
RUN curl -sL http://sourceforge.net/projects/libjpeg-turbo/files/${LIBJPEG_TURBO_VER}/libjpeg-turbo-${LIBJPEG_TURBO_VER}.tar.gz/download | tar -xz
RUN cd libjpeg-turbo-${LIBJPEG_TURBO_VER} && ./configure --prefix=/usr --enable-static --disable-shared && make all -j${NUM_CORES} && make install

# libwebp
ENV LIBWEBP_VER=0.4.3
RUN curl -sL http://downloads.webmproject.org/releases/webp/libwebp-${LIBWEBP_VER}.tar.gz | tar -xz
RUN cd libwebp-${LIBWEBP_VER} && ./configure --prefix=/usr --enable-static --disable-shared && make all -j${NUM_CORES} && make install

# OpenSSL
ENV OPENSSL_VER=1.0.2d
RUN curl -sL http://openssl.org/source/openssl-${OPENSSL_VER}.tar.gz | tar -xz
RUN cd openssl-${OPENSSL_VER} && ./config --prefix=/usr --openssldir=/etc/ssl --libdir=lib && make all && make install

RUN apt-get install -y pkg-config

# libxml2
ENV LIBXML2_VER=2.9.2
RUN curl -sL ftp://xmlsoft.org/libxml2/libxml2-sources-${LIBXML2_VER}.tar.gz | tar -xz
RUN cd libxml2-${LIBXML2_VER} && ./configure --help && ./configure --prefix=/usr --enable-static --disable-shared --with-python=no && make all -j${NUM_CORES} && make install

# libxslt
ENV LIBXSLT_VER=1.1.28
RUN curl -sL ftp://xmlsoft.org/libxml2/libxslt-${LIBXSLT_VER}.tar.gz | tar -xz
RUN cd libxslt-${LIBXSLT_VER} && ./configure --prefix=/usr --enable-static --disable-shared && make all -j${NUM_CORES} && make install

# Freetype
ENV FREETYPE_VER=2.5.5
RUN curl -sL http://sourceforge.net/projects/freetype/files/freetype2/${FREETYPE_VER}/freetype-${FREETYPE_VER}.tar.bz2/download | tar -xj
RUN cd freetype-${FREETYPE_VER} && ./configure --prefix=/usr --enable-static --disable-shared && make all -j${NUM_CORES} && make install

# expat
#ENV EXPAT_VER=2.1.0
#RUN curl -sL http://sourceforge.net/projects/expat/files/expat/${EXPAT_VER}/expat-${EXPAT_VER}.tar.gz/download | tar -xz
#RUN cd expat-${EXPAT_VER} && ./configure --prefix=/usr --enable-static --disable-shared && make all -j${NUM_CORES} && make install

# Fontconfig
ENV FONTCONFIG_VER=2.11.1
RUN curl -sL http://www.freedesktop.org/software/fontconfig/release/fontconfig-${FONTCONFIG_VER}.tar.gz | tar -xz
RUN cd fontconfig-${FONTCONFIG_VER} && ./configure --prefix=/usr --enable-static --disable-shared --enable-libxml2 && make all -j${NUM_CORES} && make install

# libicu
ENV ICU_VER=55.1
ENV ICU_FILE_VER=55_1
RUN curl -sL http://download.icu-project.org/files/icu4c/${ICU_VER}/icu4c-${ICU_FILE_VER}-src.tgz | tar -xz
RUN cd icu/source && ./configure --prefix=/usr --enable-static --disable-shared && make all -j${NUM_CORES} && make install

ENV PHANTOMJS_TAG=master
RUN apt-get install -y git
#RUN git clone https://github.com/ariya/phantomjs.git
RUN git clone https://github.com/bprodoehl/phantomjs.git

CMD cd phantomjs && git checkout ${PHANTOMJS_TAG} && ./build.sh --confirm && cp bin/phantomjs /output/.

