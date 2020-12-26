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




seems that we have bioperl issues


I will install it in the system using




```

sudo yum install perl-devel

sudo yum install cpanminus

sudo cpanm Bio::Perl # to install bioperl tools

sudo cpanm --force Bio::Perl # force failed installation module

```



### Try to generate control files

```
singularity exec ./maker_2.31.10--pl526h61907ee_17.sif maker -CTL
```


got the control files





```

[kplee@localhost maker]$ ll
total 884144
-rwxrwxr-x. 1 kplee kplee 905347072 Dec 22 23:14 maker_2.31.10--pl526h61907ee_17.sif
-rw-rw-r--. 1 kplee kplee      1413 Dec 27 00:30 maker_bopts.ctl
-rw-rw-r--. 1 kplee kplee      1214 Dec 27 00:30 maker_exe.ctl
-rw-rw-r--. 1 kplee kplee      4458 Dec 27 00:30 maker_opts.ctl


```









####### SO now I WANT TO INSTALL some other images for the second annotation pipeline including PASA, GeneMarkHMM, FGENESH, Augustus, and SNAP, GlimmerHMM  GeneWise EVM


[PASA tuto 3 octobre 2020](http://kazumaxneo.hatenablog.com/entry/2020/10/03/144141)   [turo pasa](https://www.cnblogs.com/zhanmaomao/p/12456073.html)



```
cd /home/kplee/program/pasa

singularity pull docker://quay.io/biocontainers/pasa:2.4.1--he1b5a44_0

```


log file

```
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Getting image source signatures
Copying blob a3ed95caeb02 done
Copying blob b0dc45cd432d done
Copying blob 9466b3513669 done
Copying blob ddd482ea7b54 done
Copying blob 4d69f833b9d8 done
Copying blob e7c454e5167d done
Copying blob e38092b005c0 done
Copying blob a3ed95caeb02 skipped: already exists
Copying blob a3ed95caeb02 skipped: already exists
Copying blob f879b42dfe2b done
Copying blob a3ed95caeb02 skipped: already exists
Copying blob 30194d518e49 done
Copying config e3fe1b11c0 done
Writing manifest to image destination
Storing signatures
2020/12/23 00:38:09  info unpack layer: sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4
2020/12/23 00:38:09  info unpack layer: sha256:b0dc45cd432d14fb6df7d3239dc15d09c63906f8e7bfd373a4647b107fc3746c
2020/12/23 00:38:09  warn rootless{dev/console} creating empty file in place of device 5:1
2020/12/23 00:38:09  info unpack layer: sha256:9466b3513669459396338a74673ede0166f534ab64923f66ecca58176d1ffe5e
2020/12/23 00:38:09  info unpack layer: sha256:ddd482ea7b54727ff2b6748188290b75f6441ba4091d15a5e62d2a0ed47c81dd
2020/12/23 00:38:09  info unpack layer: sha256:4d69f833b9d8db2f02cc784f9bab7317317e4062129cafe65660c9eb2cd1c115
2020/12/23 00:38:09  info unpack layer: sha256:e7c454e5167ddab5101c53d104c90c0a037c3ee3930b9a74a14da40c852134cc
2020/12/23 00:38:09  info unpack layer: sha256:e38092b005c08dab4ae80615d29944107dc3d07307e37c4558914998e0e61827
2020/12/23 00:38:09  info unpack layer: sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4
2020/12/23 00:38:09  info unpack layer: sha256:f879b42dfe2b253544b0dc6834eff097a2a994cecda76b3c2950465fef9bfdfc
2020/12/23 00:38:09  info unpack layer: sha256:30194d518e4994c6b155e452a6a4d2a362ddecc40c2c4f24efb06e6def3e0de6
INFO:    Creating SIF file...

```

ls #check the presence of the image


got


```
pasa_2.4.1--he1b5a44_0.sif

```


######  EVM Tool



cd /home/kplee/program/EVM

singularity pull docker://quay.io/biocontainers/evidencemodeler:1.1.1--2

log

```
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Getting image source signatures
Copying blob 1dbcab28ce46 done
Copying blob cfb1ba34637d done
Copying blob ace2d8a63dd5 done
Copying blob 75c080ef15eb done
Copying blob 316957f8baaf done
Copying blob dbd31e1d863d done
Copying blob 2f8531d5a6ec done
Copying blob 1dbcab28ce46 skipped: already exists
Copying blob 2e178fd72baf done
Copying blob fd59fdaa7753 done
Copying config dff695e838 done
Writing manifest to image destination
Storing signatures
2020/12/23 01:05:13  info unpack layer: sha256:1dbcab28ce46b65c0174e5e82658492107396fead31e9144c343e6bc96e471c7
2020/12/23 01:05:13  info unpack layer: sha256:cfb1ba34637d96787f6e780192bc5f09c04fe0b40c4e1acdcbc88953bce25b5b
2020/12/23 01:05:13  warn rootless{dev/console} creating empty file in place of device 5:1
2020/12/23 01:05:13  info unpack layer: sha256:ace2d8a63dd59063206661355f22da9e1c75c56d6cae5116d91db7a65f640667
2020/12/23 01:05:13  info unpack layer: sha256:75c080ef15ebb28daeadc91eeda0ca95a893fe1f4e55845da3feb58069d42fdc
2020/12/23 01:05:13  info unpack layer: sha256:316957f8baafcefd4c9f72b7e3f3fafb7fcb3ca805dc69d242aef656e0736490
2020/12/23 01:05:13  info unpack layer: sha256:dbd31e1d863d4a41565c08ad1d419c3a3538abfdd4a7971de63b8c5c8a43caec
2020/12/23 01:05:13  info unpack layer: sha256:2f8531d5a6ece34772cf99a2d30e3057c636f1ad05cdec0c8009eab401e616a7
2020/12/23 01:05:13  info unpack layer: sha256:1dbcab28ce46b65c0174e5e82658492107396fead31e9144c343e6bc96e471c7
2020/12/23 01:05:13  info unpack layer: sha256:2e178fd72baf28bc587b5d274574f1a3d3e25ac802fdaf154af258a3c5891831
2020/12/23 01:05:13  info unpack layer: sha256:fd59fdaa7753c713e42c24f11e061867f8bb0f2d1778bbb9bef208f8cdc13f4a
INFO:    Creating SIF file...




```

singularity exec ./evidencemodeler_1.1.1--2.sif evidence_modeler.pl --help


```



log



```

WARNING: Skipping mount /usr/local/var/singularity/mnt/session/etc/resolv.conf [files]: /etc/resolv.conf doesn't exist in container
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
        LANGUAGE = (unset),
        LC_ALL = (unset),
        LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").

################# Evidence Modeler ##############################
#
#  parameters:
#
#  Required:
#
#  --genome              genome sequence in fasta format
#  --weights              weights for evidence types file
#  --gene_predictions     gene predictions gff3 file
#
#  Optional but recommended:
#  --protein_alignments   protein alignments gff3 file
#  --transcript_alignments       transcript alignments gff3 file
#
#  Optional and miscellaneous
#
#  --repeats               gff3 file with repeats masked from genome file
#
#
#  --terminalExons         supplementary file of additional terminal exons to consider (from PASA long-orfs)
#
#  --stop_codons             list of stop codons (default: TAA,TGA,TAG)
#                               *for Tetrahymena, set --stop_codons TGA
#  --min_intron_length       minimum length for an intron (default 20 bp)
#  --exec_dir                directory that EVM cd's to before running.
#
# flags:
#
#  --forwardStrandOnly   runs only on the forward strand
#  --reverseStrandOnly   runs only on the reverse strand
#
#  -S                    verbose flag
#  --debug               debug mode, writes lots of extra files.
#  --report_ELM          report the eliminated EVM preds too.
#
#  --search_long_introns  <int>  when set, reexamines long introns (can find nested genes, but also can result in FPs) (default: 0 (off))
#
#
#  --re_search_intergenic <int>  when set, reexamines intergenic regions of minimum length (can add FPs) (default: 0  (off))
#  --terminal_intergenic_re_search <int>   reexamines genomic regions outside of the span of all predicted genes (default: 10000)
#
#################################################################################################################################



```


Le reste [ici](https://www.nature.com/articles/s42003-020-0770-2#Sec11)




