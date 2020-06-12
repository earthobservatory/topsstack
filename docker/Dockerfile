FROM hysds/isce2:latest-es1

# Set to root user
USER root

# Install necessary libraries
#RUN cd /etc/yum.repos.d/ && \
#    wget http://download.opensuse.org/repositories/home:/tange/CentOS_CentOS-6/home:tange.repo && \
RUN yum update -y && \
    yum install parallel -y && \
    yum clean all
    
RUN /opt/conda/bin/conda install -y -c conda-forge jq \
 && /opt/conda/bin/pip install opencv-python

# Get source code for topsStack code which is not in install dir
# May remove this block if /opt/isce2/src/isce2 is not removed in building the isce2 docker
RUN git clone https://github.com/isce-framework/isce2 /opt/isce2/src/isce2 \
 && git --git-dir=/opt/isce2/src/isce2/.git reset --hard $(cat /opt/isce2/version.json | /opt/conda/bin/jq -r .object.sha)

## Install ISCE2
#RUN source /opt/conda/bin/activate root \
# && conda install -c conda-forge isce2=2.3.3
#
## Install directory
#ENV ISCE_HOME /opt/conda/lib/python3.7/site-packages/isce
#ARG ISCE_APPS_DIR=${ISCE_HOME}/applications
#ARG STACK_INSTALL_DIR=${ISCE_HOME}/components/contrib/stack
#ARG TOPSSTACK_INSTALL_DIR=${STACK_INSTALL_DIR}/topsStack
#ARG TOPSSTACK_COPY_DIR=/root/topsStack
#ENV PATH ${PATH}:${ISCE_APPS_DIR}:${TOPSSTACK_INSTALL_DIR}
#
## Copy stack processor code into docker image
#COPY topsStack $TOPSSTACK_COPY_DIR
#
## Copy stack processor code into install directory; Adjust group permissions
## so stack processor and associated logs can be written.
#RUN mkdir -p $STACK_INSTALL_DIR \
# && cp -r $TOPSSTACK_COPY_DIR $STACK_INSTALL_DIR \
# && rm -rf $TOPSSTACK_COPY_DIR


# Set to ops user
USER ops

# Copy topsStack PGE into docker image
COPY . /home/ops/verdi/ops/topsstack

# add topsStack code to path
ENV PATH /opt/isce2/src/isce2/contrib/stack/topsStack/:/home/ops/verdi/ops/topsstack/topsStack:${PATH}

# Change to work directory
WORKDIR /home/ops

# Run shell
CMD ["/bin/bash", "--login"]
