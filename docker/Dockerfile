FROM hysds/isce2:latest-es1

# Set to root user
USER root

# Install necessary libraries
RUN /opt/conda/bin/conda install -y -c conda-forge jq parallel \
 && /opt/conda/bin/pip install opencv-python

# Get source code for topsStack code which is not in install dir
# May remove this block if /opt/isce2/src/isce2 is already present in hysds/isce2 docker
RUN git clone https://github.com/isce-framework/isce2 /opt/isce2/src/isce2 \
 && git --git-dir=/opt/isce2/src/isce2/.git reset --hard $(cat /opt/isce2/version.json | /opt/conda/bin/jq -r .object.sha)

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
