FROM public.ecr.aws/lambda/python:3.8

# Basic utils for downloading and decompressing
RUN yum install -y \
    bzip2 \
    gzip \
    sqlite-devel \
    libtiff \
    libtiff-devel \
    sqlite \
    tar \
    wget \
    && yum clean all \
    && rm -rf /var/cache/yum

# Required to compile geos libraries
# Maybe just make, gcc, and autoconf?
RUN yum groupinstall -y "Development Tools" \
    && yum clean all \
    && rm -rf /var/cache/yum

RUN cd /tmp \
    && wget https://github.com/matplotlib/basemap/archive/v1.2.2rel.tar.gz \
    && tar -xvzf v1.2.2rel.tar.gz \
    && rm v1.2.2rel.tar.gz

RUN cd /tmp \
    && wget http://download.osgeo.org/geos/geos-3.8.1.tar.bz2 \
    && tar xf geos-3.8.1.tar.bz2 \
    && cd geos-3.8.1 \
    && ./configure --prefix=/usr \
    && make -j4 \
    && make install \
    && /sbin/ldconfig \
    && cd .. \
    && rm -rf geos-3.8.1 \
    && rm -rf geos-3.8.1.tar.bz2

RUN cd /tmp \
    && wget http://download.osgeo.org/proj/proj-7.2.0.tar.gz \
    && tar xf proj-7.2.0.tar.gz \
    && cd proj-7.2.0 \	
    && ./configure --prefix=/usr SQLITE3_CFLAGS=-I/usr/include SQLITE3_LIBS="-L/usr/lib -lsqlite3" TIFF_LIBS="-L/usr/lib64 -ltiff" --without-curl \
    && make -j4 \
    && make install \
    && cd .. \
    && rm -rf proj-7.2.0 proj-7.2.0.tar.gz

# pip-installable python libs
RUN pip install --upgrade \
    matplotlib==3.3.0 \
    numpy==1.17.3 \
    pandas==1.1.4 \
    s3fs==0.5.1 \
    xarray==0.16.2 \
    zarr==2.5.0

# basemap custom install
RUN cd /tmp/basemap-1.2.2rel \
    && rm lib/mpl_toolkits/__init__.py \
    && python setup.py install

COPY weather.py ./

CMD [ "weather.lambdaHandler" ]