FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04


### tensorflow

# Pick up some TF dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cuda-command-line-tools-8-0 \
        cuda-cublas-dev-8-0 \
        cuda-cudart-dev-8-0 \
        cuda-cufft-dev-8-0 \
        cuda-curand-dev-8-0 \
        cuda-cusolver-dev-8-0 \
        cuda-cusparse-dev-8-0 \
        curl \
        libnccl2 \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        rsync \
        software-properties-common \
        unzip

#RUN apt-get update && \
#        apt-get install nvinfer-runtime-trt-repo-ubuntu1604-4.0.1-ga-cuda9.0 && \
#        apt-get update && \
#        apt-get install libnvinfer4=4.1.2-1+cuda9.0


RUN apt-get install -y curl grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb

RUN apt-get install -y libatlas-base-dev libgflags-dev libgoogle-glog-dev cmake libprotobuf-dev protobuf-compiler vim

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

### miniconda
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda2-4.5.12-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrcs

### package
RUN conda create --name env python=2.7
RUN source activate env
RUN conda install numpy scipy matplotlib dill tqdm cython pandas jupyter scikit-image h5py leveldb -y -n env
RUN conda install -c anaconda tornado opencv pyyaml boost lmdb -y -n env

### start command
RUN echo "conda activate env" >> ~/.bashrc

ENTRYPOINT [ "/usr/bin/tini", "--" ]
CMD [ "/bin/bash" ]
