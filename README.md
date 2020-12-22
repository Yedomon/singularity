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



##### Now installation and running of RepeatModeler and Repeat Masker

#####---https://github.com/Dfam-consortium/TETools for running instruction using singularity


```

$ curl -sSLO https://github.com/Dfam-consortium/TETools/raw/master/dfam-tetools.sh
$ chmod +x dfam-tetools.sh
$ ./dfam-tetools.sh

```

WAIT FOR THE SIF FILE CREATION THEN RUN


```
BuildDatabase --help

RepeatModeler --help

RepeatMasker --help

runcoseg.pl --help

```
Should work

try to use this code for your run



```



$ BuildDatabase -name genome1 genome1.fa
$ RepeatModeler -database genome1 [-LTRStruct] [-pa 8]

$ RepeatMasker genome1.fa [-lib library.fa] [-pa 8]

$ runcoseg.pl -d -m 50 -c ALU.cons -s ALU.seqs -i ALU.ins


```


#### Now try to install and dry run maker image


### Type conda install maker ON GOOGLE 

### Go https://bioconda.github.io/recipes/maker/README.html

### Intetrested in quai.io link at the bottom



> or use the docker container:

> docker pull quay.io/biocontainers/maker:<tag>

(see [`*maker/tags*`](https://quay.io/repository/biocontainers/maker?tab=tags) for valid values for <tag>)


### By doing this, just check the available tag anfd select one


### Now let'us pull the maker image in our system

cd /home/kplee/program/maker

singularity pull docker://quay.io/biocontainers/maker:2.31.10--pl526h61907ee_17


## Getting this log


```

INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Getting image source signatures
Copying blob 929ec067dd64 done
Copying blob 6d9e7a283257 done
Copying blob d483744c9fb6 done
Copying config 69f4134944 done
Writing manifest to image destination
Storing signatures
2020/12/22 23:11:41  info unpack layer: sha256:929ec067dd64d4ee237275e2222e04358db954ee38be44fb29100330d280bd6c
2020/12/22 23:11:43  warn rootless{usr/local/man} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2020/12/22 23:11:43  warn rootless{dev/full} creating empty file in place of device 1:7
2020/12/22 23:11:43  warn rootless{dev/zero} creating empty file in place of device 1:5
2020/12/22 23:11:43  warn rootless{dev/tty} creating empty file in place of device 5:0
2020/12/22 23:11:43  warn rootless{dev/null} creating empty file in place of device 1:3
2020/12/22 23:11:43  warn rootless{dev/random} creating empty file in place of device 1:8
2020/12/22 23:11:43  warn rootless{dev/urandom} creating empty file in place of device 1:9
2020/12/22 23:11:43  info unpack layer: sha256:6d9e7a2832576734b34ee23343008d6fe444844cb3d25321a7452389c40f24cb
2020/12/22 23:11:44  info unpack layer: sha256:d483744c9fb62de8a93fcce07e536abe5f289b377b474fe70a4c4e8ec8440473
INFO:    Creating SIF file...


## check 

```
ls
```



got this

```
maker_2.31.10--pl526h61907ee_17.sif

```


So we have the image of maker in our system 

let us try to run it




```


singularity exec ./maker_2.31.10--pl526h61907ee_17.sif maker --help

```


I got

```


Possible precedence issue with control flow operator at /usr/local/lib/site_perl/5.26.2/Bio/DB/IndexedBase.pm line 805.

MAKER version 2.31.10

Usage:

     maker [options] <maker_opts> <maker_bopts> <maker_exe>


Description:

     MAKER is a program that produces gene annotations in GFF3 format using
     evidence such as EST alignments and protein homology. MAKER can be used to
     produce gene annotations for new genomes as well as update annotations
     from existing genome databases.

     The three input arguments are control files that specify how MAKER should
     behave. All options for MAKER should be set in the control files, but a
     few can also be set on the command line. Command line options provide a
     convenient machanism to override commonly altered control file values.
     MAKER will automatically search for the control files in the current
     working directory if they are not specified on the command line.

     Input files listed in the control options files must be in fasta format
     unless otherwise specified. Please see MAKER documentation to learn more
     about control file  configuration.  MAKER will automatically try and
     locate the user control files in the current working directory if these
     arguments are not supplied when initializing MAKER.

     It is important to note that MAKER does not try and recalculated data that
     it has already calculated.  For example, if you run an analysis twice on
     the same dataset you will notice that MAKER does not rerun any of the
     BLAST analyses, but instead uses the blast analyses stored from the
     previous run. To force MAKER to rerun all analyses, use the -f flag.

     MAKER also supports parallelization via MPI on computer clusters. Just
     launch MAKER via mpiexec (i.e. mpiexec -n 40 maker). MPI support must be
     configured during the MAKER installation process for this to work though


Options:

     -genome|g <file>    Overrides the genome file path in the control files

     -RM_off|R           Turns all repeat masking options off.

     -datastore/         Forcably turn on/off MAKER's two deep directory
      nodatastore        structure for output.  Always on by default.

     -old_struct         Use the old directory styles (MAKER 2.26 and lower)

     -base    <string>   Set the base name MAKER uses to save output files.
                         MAKER uses the input genome file name by default.

     -tries|t <integer>  Run contigs up to the specified number of tries.

     -cpus|c  <integer>  Tells how many cpus to use for BLAST analysis.
                         Note: this is for BLAST and not for MPI!

     -mpi                Forces MAKER to use MPI (overriding MPI autodetection).
                         Useful if MAKER doesn't correctly detect that it was
                         launched using an MPI process manager, or if running
                         in a Singularity container.

     -force|f            Forces MAKER to delete old files before running again.
                         This will require all blast analyses to be rerun.

     -again|a            recaculate all annotations and output files even if no
                         settings have changed. Does not delete old analyses.

     -quiet|q            Regular quiet. Only a handlful of status messages.

     -qq                 Even more quiet. There are no status messages.

     -dsindex            Quickly generate datastore index file. Note that this
                         will not check if run settings have changed on contigs

     -nolock             Turn off file locks. May be usful on some file systems,
                         but can cause race conditions if running in parallel.

     -TMP                Specify temporary directory to use.

     -CTL                Generate empty control files in the current directory.

     -OPTS               Generates just the maker_opts.ctl file.

     -BOPTS              Generates just the maker_bopts.ctl file.

     -EXE                Generates just the maker_exe.ctl file.

     -MWAS    <option>   Easy way to control mwas_server for web-based GUI

                              options:  STOP
                                        START
                                        RESTART

     -version            Prints the MAKER version.

     -help|?             Prints this usage statement.
```



seems that we have bioperl issues


I will install it in the system using




```

sudo yum install perl-devel

sudo yum install cpanminus

sudo cpanm Bio::Perl # to install bioperl tools

sudo cpanm --force Bio::Perl # force failed installation module


```

