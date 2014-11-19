FROM {{ _base_name }}:{{ _base_tag }}
MAINTAINER Anders Åslund <aaslund.anders@gmail.com>

# update apt and install dependencies
RUN apt-get -qq update
RUN apt-get install -y python python-dev python-pip python-virtualenv
RUN apt-get install -y build-essential gcc git rsync cmake make g++ binutils automake flex bison patch wget

ENV KNXDIR /usr
ENV INSTALLDIR $KNXDIR/local
ENV SOURCEDIR  $KNXDIR/src
ENV LD_LIBRARY_PATH $INSTALLDIR/lib
#mkdir ${KNXDIR}
#cd ${KNXDIR}

# build pthsem
RUN cd $SOURCEDIR/ && wget http://downloads.sourceforge.net/sourceforge/bcusdk/pthsem_2.0.8.tar.gz
RUN cd $SOURCEDIR/ && tar -xzf pthsem_2.0.8.tar.gz
RUN cd $SOURCEDIR/pthsem-2.0.8/ && ./configure --prefix=$INSTALLDIR/ && make && make test && make install

# build linknx
RUN cd $SOURCEDIR/ && wget http://downloads.sourceforge.net/sourceforge/linknx/linknx-0.0.1.32.tar.gz
RUN cd $SOURCEDIR/ && tar -xzf linknx-0.0.1.32.tar.gz
RUN cd $SOURCEDIR/linknx-0.0.1.32/ && ./configure --without-log4cpp --without-lua --prefix=$INSTALLDIR/ --with-pth=$INSTALLDIR/ && make && make install

# build eibd
RUN cd $SOURCEDIR/ && wget http://freefr.dl.sourceforge.net/project/bcusdk/bcusdk/bcusdk_0.0.5.tar.gz
RUN cd $SOURCEDIR/ && tar -xzf bcusdk_0.0.5.tar.gz
RUN cd $SOURCEDIR/bcusdk-0.0.5/ && ./configure --enable-onlyeibd --enable-eibnetiptunnel --enable-eibnetipserver --enable-ft12 --prefix=$INSTALLDIR/ --with-pth=$INSTALLDIR/ && make && make install

#RUN groupadd -r eibd -g 433 && useradd -u 431 -r -g eibd -s /sbin/nologin -c "eibd user" eibd
RUN useradd eibd -s /bin/false -U -M
ADD eibd.sh /etc/init.d/eibd
RUN chmod +x /etc/init.d/eibd
RUN update-rc.d eibd defaults 98 02

EXPOSE 6720

#RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/src/*.tar.gz

# run hellobaby app as a runit service
#RUN mkdir /etc/service/hellobaby
#ADD runapp.sh /etc/service/hellobaby/run
#RUN chmod +x /etc/service/hellobaby/run



# Clean up APT when done.
#RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
