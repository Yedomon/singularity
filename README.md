# singularity

[Schedule: Using containers in bioinformatics](https://docs.google.com/document/d/1X62Ka2jTG_iDAO90mKnbQ6IGsUuBJxkAedzhu8fOSTg/edit)

[Workshop: Singularity Containers for Bioinformatics](https://pawseysc.github.io/containers-bioinformatics-workshop/1.prep1-ssh/index.html) | [Introduction to Containers](https://youtu.be/eE-dwkfyiEE) | [Basics of singularity](https://www.youtube.com/watch?v=fFpBJF5nopg&t=45s) | [Build a contenair image](https://youtu.be/Piml8jE2naM) | [video1:  Contenainerising a pipeline](https://youtu.be/U1jMSOhkgvA) | [video2: Setting Up Graphical Applications](https://youtu.be/nr5oM4UIyDI) | [video3: Building Containers](https://www.youtube.com/watch?v=k-jP3nkaqC0)

[set up](https://pawseysc.github.io/containers-bioinformatics-workshop/setup.html)

[conda singularity installation](https://anaconda.org/conda-forge/singularity)

[Singularity](https://nanopype.readthedocs.io/en/latest/installation/singularity/)


##### How I installed singularity in Centos 7 OS?


```
# Install singularity on Centos 7

# Install Dependencies


$ sudo yum update -y && \
     sudo yum groupinstall -y 'Development Tools' && \
     sudo yum install -y \
     openssl-devel \
     libuuid-devel \
     libseccomp-devel \
     wget \
     squashfs-tools \
     cryptsetup

# Install go

Do like 

$ export VERSION=1.15.6 OS=linux ARCH=amd64 && \
wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
sudo tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
rm go$VERSION.$OS-$ARCH.tar.gz

 or 
#-- What I did


wget https://golang.org/dl/go1.15.6.linux-amd64.tar.gz
sudo tar -C /usr/local go1.15.6.linux-amd64.tar.gz
rm go1.15.6.linux-amd64.tar.gz


# set path

echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
source ~/.bashrc


# Download
$ export VERSION=3.7.0 && # adjust this as necessary \
    wget https://github.com/hpcng/singularity/releases/download/v${VERSION}/singularity-${VERSION}.tar.gz && \
    tar -xzf singularity-${VERSION}.tar.gz && \
    cd singularity


# compile and install
./mconfig && \
    make -C ./builddir && \
    sudo make -C ./builddir install

# Configure
sudo sed -i 's/^ *mount *home *=.*/mount home = no/g' /home/kplee/program/singularity/builddir/singularity.conf 
echo ". /home/kplee/program/singularity/builddir/etc/bash_completion.d/singularity" >> $(eval echo ~"$USER")/.bashrc


```
