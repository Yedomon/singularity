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



Test with real datasets

```
cd /home/kplee/analysis/0002_mr4003_maker_annotation

singularity exec /home/kplee/program/maker/maker_2.31.10--pl526h61907ee_17.sif maker -CTL

/home/kplee/analysis/0001_mr4003_repeat_analysis/sesamiMR4003.contigs.fasta.masked

/home/kplee/datafiles/003_fos.MR4003.configs/original/transcripts.fa

/home/kplee/datafiles/003_fos.MR4003.configs/original/lyc.fasta


singularity exec /home/kplee/program/maker/maker_2.31.10--pl526h61907ee_17.sif maker -cpus 20 -base mr4003_rnd1 round1_maker_opts.ctl maker_bopts.ctl maker_exe.ctl


```


got this 



```
Possible precedence issue with control flow operator at /usr/local/lib/site_perl/5.26.2/Bio/DB/IndexedBase.pm line 805.
STATUS: Parsing control files...
df: Warning: cannot read table of mounted file systems: No such file or directory
ERROR: Could not determine if RepBase is installed
--> rank=NA, hostname=localhost.localdomain


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







Le reste [ici](https://www.nature.com/articles/s42003-020-0770-2#Sec11)



### GeneMark Installation

I went [here](http://topaz.gatech.edu/GeneMark/license_download.cgi)

I fill the form and getre-orientated for downloading Genemark suite and genemark key

Now Installation in the server


### Uncompression

```
gunzip gmes_linux_64.tar.gz

tar -xvf gmes_linux_64.tar

```

### Check dependencies 

```
./check_install.bash

```

### Got this result

```
[kplee@localhost gmes_linux_64]$ ./check_install.bash
Checking GeneMark-ES installation
Can't locate Hash/Merge.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /
BEGIN failed--compilation aborted.
Error, Perl CPAN library Hash::Merge not found


```

So I need to install Hash/Merge missing dependencies


I did like this


```

sudo cpanm Hash::Merge


```



got this log


```

[sudo] password for kplee:
--> Working on Hash::Merge
Fetching http://www.cpan.org/authors/id/H/HE/HERMES/Hash-Merge-0.302.tar.gz ... OK
Configuring Hash-Merge-0.302 ... OK
==> Found dependencies: Clone::Choose
--> Working on Clone::Choose
Fetching http://www.cpan.org/authors/id/H/HE/HERMES/Clone-Choose-0.010.tar.gz ... OK
Configuring Clone-Choose-0.010 ... OK
==> Found dependencies: Test::Without::Module
--> Working on Test::Without::Module
Fetching http://www.cpan.org/authors/id/C/CO/CORION/Test-Without-Module-0.20.tar.gz ... OK
Configuring Test-Without-Module-0.20 ... OK
Building and testing Test-Without-Module-0.20 ... OK
Successfully installed Test-Without-Module-0.20
Building and testing Clone-Choose-0.010 ... OK
Successfully installed Clone-Choose-0.010
Building and testing Hash-Merge-0.302 ... OK
Successfully installed Hash-Merge-0.302
3 distributions installed

```


### Test again


```

./check_install.bash


```



Got this


```


Checking GeneMark-ES installation
Can't locate Logger/Simple.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .).
BEGIN failed--compilation aborted.
Error, Perl CPAN library Logger::Simple not found


```



Try to solve this also


```

sudo cpanm Logger::Simple

```



Got this



```

[kplee@localhost gmes_linux_64]$ sudo cpanm Logger::Simple
--> Working on Logger::Simple
Fetching http://www.cpan.org/authors/id/T/TS/TSTANLEY/Logger-Simple-2.0.tar.gz ... OK
Configuring Logger-Simple-2.0 ... OK
==> Found dependencies: Test::Pod, Object::InsideOut
--> Working on Test::Pod
Fetching http://www.cpan.org/authors/id/E/ET/ETHER/Test-Pod-1.52.tar.gz ... OK
Configuring Test-Pod-1.52 ... OK
Building and testing Test-Pod-1.52 ... OK
Successfully installed Test-Pod-1.52
--> Working on Object::InsideOut
Fetching http://www.cpan.org/authors/id/J/JD/JDHEDDEN/Object-InsideOut-4.05.tar.gz ... OK
Configuring Object-InsideOut-4.05 ... OK
Building and testing Object-InsideOut-4.05 ... OK
Successfully installed Object-InsideOut-4.05
Building and testing Logger-Simple-2.0 ... OK
Successfully installed Logger-Simple-2.0
3 distributions installed



```

check again


```

./check_install.bash

```


got this


```
Checking GeneMark-ES installation
Can't locate Parallel/ForkManager.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .).
BEGIN failed--compilation aborted.
Error, Perl CPAN library Parallel::ForkManager not found


```


Try to solve this by runining


```

sudo cpanm Parallel::ForkManager

```


got this


```


[kplee@localhost gmes_linux_64]$ sudo cpanm Parallel::ForkManager
--> Working on Parallel::ForkManager
Fetching http://www.cpan.org/authors/id/Y/YA/YANICK/Parallel-ForkManager-2.02.tar.gz ... OK
Configuring Parallel-ForkManager-2.02 ... OK
==> Found dependencies: Moo::Role, Moo
--> Working on Moo::Role
Fetching http://www.cpan.org/authors/id/H/HA/HAARG/Moo-2.004004.tar.gz ... OK
Configuring Moo-2.004004 ... OK
==> Found dependencies: Role::Tiny, Sub::Defer, Sub::Quote, Class::Method::Modifiers
--> Working on Role::Tiny
Fetching http://www.cpan.org/authors/id/H/HA/HAARG/Role-Tiny-2.001004.tar.gz ... OK
Configuring Role-Tiny-2.001004 ... OK
Building and testing Role-Tiny-2.001004 ... OK
Successfully installed Role-Tiny-2.001004
--> Working on Sub::Defer
Fetching http://www.cpan.org/authors/id/H/HA/HAARG/Sub-Quote-2.006006.tar.gz ... OK
Configuring Sub-Quote-2.006006 ... OK
Building and testing Sub-Quote-2.006006 ... OK
Successfully installed Sub-Quote-2.006006
--> Working on Class::Method::Modifiers
Fetching http://www.cpan.org/authors/id/E/ET/ETHER/Class-Method-Modifiers-2.13.tar.gz ... OK
Configuring Class-Method-Modifiers-2.13 ... OK
Building and testing Class-Method-Modifiers-2.13 ... OK
Successfully installed Class-Method-Modifiers-2.13
Building and testing Moo-2.004004 ... OK
Successfully installed Moo-2.004004
Building and testing Parallel-ForkManager-2.02 ... OK
Successfully installed Parallel-ForkManager-2.02
5 distributions installed


```




check



```
Checking GeneMark-ES installation
Can't locate MCE/Mutex.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .).
BEGIN failed--compilation aborted.
Error, Perl CPAN library MCE::Mutex not found
```



solve


```
sudo cpanm MCE::Mutex

```




got this


```
--> Working on MCE::Mutex
Fetching http://www.cpan.org/authors/id/M/MA/MARIOROY/MCE-1.874.tar.gz ... OK
Configuring MCE-1.874 ... OK
Building and testing MCE-1.874 ... OK
Successfully installed MCE-1.874
1 distribution installed


```



check 

do the same thing for Math::Utils, 


check again



```
[kplee@localhost gmes_linux_64]$ ./check_install.bash
Checking GeneMark-ES installation
GeneMark.hmm eukaryotic 3
License key ".gm_key" not found.
This file is neccessary in order to use GeneMark.hmm eukaryotic 3.
Error, installation key is missing or expired


```


So I need to set the key



```
# extract the key file
zcat gm_key_64.gz > ~/program/GeneMark/gmes_linux_64/.gm_key

```


check again

```
./check_install.bash

```

I got this 






```
Checking GeneMark-ES installation
All required components for GeneMark-ES were found

```


So It is okay Now. GeneMark suite and dependencies are well installed.



Let's check by runing a help command



```
./gmes_petap.pl

```


got this



```

# -------------------
Usage:  ./gmes_petap.pl  [options]  --sequence [filename]

GeneMark-ES Suite version 4.62_lic
Suite includes GeneMark.hmm, GeneMark-ES, GeneMark-ET and GeneMark-EP algorithms.

Input sequence/s should be in FASTA format.

Select one of the gene prediction algorithm

To run GeneMark-ES self-training algorithm
  --ES

To run GeneMark-ET with hints from transcriptome splice alignments
  --ET           [filename]; file with intron coordinates from RNA-Seq read splice alignment in GFF format
  --et_score     [number]; default 10; minimum score of intron in initiation of the ET algorithm

To run GeneMark-EP with hints from protein splice alignments
  --EP
  --dbep         [filename]; file with protein database in FASTA format
  --ep_score     [number,number]; default 4,0.25; minimum score of intron in initiation of the EP algorithm
or
  --EP           [filename]; file with intron coordinates from protein splice alignment in GFF format

To run GeneMark.hmm predictions using previously derived model
  --predict_with [filename]; file with species specific gene prediction parameters

To run ES, ET or EP with branch point model. This option is most useful for fungal genomes
  --fungus

To run hmm, ES, ET or EP in PLUS mode (prediction with hints)
  --evidence     [filename]; file with hints in GFF format

Masking option
  --soft_mask    [number] or [auto]; default auto; to indicate that lowercase letters stand for repeats;
                 masks only lowercase repeats longer than specified length
                 In 'auto' mode length is adjusted based on the size of the input genome

Run options
  --cores        [number]; default 1; to run program with multiple threads
  --pbs          to run on cluster with PBS support
  --v            verbose

Optional sequence pre-processing parameters
  --max_contig   [number]; default 5000000; will split input genomic sequence into contigs shorter then max_contig
  --min_contig   [number]; default 50000 (10000 fungi); will ignore contigs shorter then min_contig in training
  --max_gap      [number]; default 5000; will split sequence at gaps longer than max_gap
                 Letters 'n' and 'N' are interpreted as standing within gaps
  --max_mask     [number]; default 5000; will split sequence at repeats longer then max_mask
                 Letters 'x' and 'X' are interpreted as results of hard masking of repeats

Optinal algorithm parameters
  --max_intron            [number]; default 10000 (3000 fungi); maximum length of intron
  --max_intergenic        [number]; default 50000 (10000 fungi); maximum length of intergenic regions
  --min_contig_in_predict [number]; default 500; minimum allowed length of contig in prediction step
  --min_gene_in_predict   [number]; default 300 (120 fungi); minimum allowed gene length in prediction step
  --gc_donor              [value];  default 0.001; transition probability to GC donor in the range 0..1; 'auto' mode detects probability from training; 'off' switches GC donor model OFF

Developer options
  --gc3          [number]; GC3 cutoff in training for grasses
  --training     to run only training step of algorithms; applicable to ES, ET or EP
  --prediction   to run only prediction step of algorithms using species parameters from previously executed training; applicable to ES, ET or EP
  --usr_cfg      [filename]; use custom configuration from this file
  --ini_mod      [filename]; use this file with parameters for algorithm initiation
  --test_set     [filename]; to evaluate prediction accuracy on the given test set
  --key_bin
  --debug
# -------------------



```


Awesome! Work fine! 

Now set into the bin



```
# Go to home directory
cd

# pen the bash profie
vi .bash_profile

```


Put the full path in the Path VARIABLE



```
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$HOME/program/GeneMark/gmes_linux_64

export PATH

```


press ESC + : + wq and then run



```
source .bash_profile

```


Test if it is working well


```
gmes_petap.pl
```


Got this result


```

# -------------------
Usage:  /home/kplee/program/GeneMark/gmes_linux_64/gmes_petap.pl  [options]  --sequence [filename]

GeneMark-ES Suite version 4.62_lic
Suite includes GeneMark.hmm, GeneMark-ES, GeneMark-ET and GeneMark-EP algorithms.

Input sequence/s should be in FASTA format.

Select one of the gene prediction algorithm

To run GeneMark-ES self-training algorithm
  --ES

To run GeneMark-ET with hints from transcriptome splice alignments
  --ET           [filename]; file with intron coordinates from RNA-Seq read splice alignment in GFF format
  --et_score     [number]; default 10; minimum score of intron in initiation of the ET algorithm

To run GeneMark-EP with hints from protein splice alignments
  --EP
  --dbep         [filename]; file with protein database in FASTA format
  --ep_score     [number,number]; default 4,0.25; minimum score of intron in initiation of the EP algorithm
or
  --EP           [filename]; file with intron coordinates from protein splice alignment in GFF format

To run GeneMark.hmm predictions using previously derived model
  --predict_with [filename]; file with species specific gene prediction parameters

To run ES, ET or EP with branch point model. This option is most useful for fungal genomes
  --fungus

To run hmm, ES, ET or EP in PLUS mode (prediction with hints)
  --evidence     [filename]; file with hints in GFF format

Masking option
  --soft_mask    [number] or [auto]; default auto; to indicate that lowercase letters stand for repeats;
                 masks only lowercase repeats longer than specified length
                 In 'auto' mode length is adjusted based on the size of the input genome

Run options
  --cores        [number]; default 1; to run program with multiple threads
  --pbs          to run on cluster with PBS support
  --v            verbose

Optional sequence pre-processing parameters
  --max_contig   [number]; default 5000000; will split input genomic sequence into contigs shorter then max_contig
  --min_contig   [number]; default 50000 (10000 fungi); will ignore contigs shorter then min_contig in training
  --max_gap      [number]; default 5000; will split sequence at gaps longer than max_gap
                 Letters 'n' and 'N' are interpreted as standing within gaps
  --max_mask     [number]; default 5000; will split sequence at repeats longer then max_mask
                 Letters 'x' and 'X' are interpreted as results of hard masking of repeats

Optinal algorithm parameters
  --max_intron            [number]; default 10000 (3000 fungi); maximum length of intron
  --max_intergenic        [number]; default 50000 (10000 fungi); maximum length of intergenic regions
  --min_contig_in_predict [number]; default 500; minimum allowed length of contig in prediction step
  --min_gene_in_predict   [number]; default 300 (120 fungi); minimum allowed gene length in prediction step
  --gc_donor              [value];  default 0.001; transition probability to GC donor in the range 0..1; 'auto' mode detects probability from training; 'off' switches GC donor model OFF

Developer options
  --gc3          [number]; GC3 cutoff in training for grasses
  --training     to run only training step of algorithms; applicable to ES, ET or EP
  --prediction   to run only prediction step of algorithms using species parameters from previously executed training; applicable to ES, ET or EP
  --usr_cfg      [filename]; use custom configuration from this file
  --ini_mod      [filename]; use this file with parameters for algorithm initiation
  --test_set     [filename]; to evaluate prediction accuracy on the given test set
  --key_bin
  --debug
# -------------------


```

Fantabulous! Working very well!



Notice, for the key management I used this [online ressource](https://eukcc.readthedocs.io/en/latest/install.html) as indication.




Try to run an example with real dataset


```
gmes_petap.pl --cores 2 --sequence sesamiMR4003.contigs.fasta.masked --ES --fungus &> log &

```


### GenID installation



I went [here](https://github.com/guigolab/geneid/archive/v1.4.5.tar.gz)


```
wget https://github.com/guigolab/geneid/archive/v1.4.5.tar.gz

gunzip -d v1.4.5.tar.gz

tar -xvf v1.4.5.tar

cd geneid-1.4.5/

make


cd bin


./geneid -h


```


Got this



```
NAME
        geneid - a program to annotate genomic sequences
SYNOPSIS
        geneid  [-bdaefitnxszru]
                [-TDAZU]
                [-p gene_prefix]
                [-G] [-3] [-X] [-M] [-m]
                [-WCF] [-o]
                [-j lower_bound_coord]
                [-k upper_bound_coord]
                [-N numer_nt_mapped]
                [-O <gff_exons_file>]
                [-R <gff_annotation-file>]
                [-S <gff_homology_file>]
                [-P <parameter_file>]
                [-E exonweight]
                [-V evidence_exonweight]
                [-Bv] [-h]
                <locus_seq_in_fasta_format>
RELEASE
        geneid v 1.4
OPTIONS
        -b: Output Start codons
        -d: Output Donor splice sites
        -a: Output Acceptor splice sites
        -e: Output Stop codons
        -f: Output Initial exons
        -i: Output Internal exons
        -t: Output Terminal exons
        -n: Output introns
        -s: Output Single genes
        -x: Output all predicted exons
        -z: Output Open Reading Frames

        -T: Output genomic sequence of exons in predicted genes
        -D: Output genomic sequence of CDS in predicted genes
        -A: Output amino acid sequence derived from predicted CDS

        -p: Prefix this value to the names of predicted genes, peptides and CDS

        -G: Use GFF format to print predictions
        -3: Use GFF3 format to print predictions
        -X: Use extended-format to print gene predictions
        -M: Use XML format to print gene predictions
        -m: Show DTD for XML-format output

        -j  <coord>: Begin prediction at this coordinate
        -k  <coord>: End prediction at this coordinate
        -N  <num_reads>: Millions of reads mapped to genome
        -W: Only Forward sense prediction (Watson)
        -C: Only Reverse sense prediction (Crick)
        -U: Allow U12 introns (Requires appropriate U12 parameters to be set in the parameter file)
        -r: Use recursive splicing
        -F: Force the prediction of one gene structure
        -o: Only running exon prediction (disable gene prediction)
        -O  <exons_filename>: Only running gene prediction (not exon prediction)
        -Z: Activate Open Reading Frames searching

        -R  <exons_filename>: Provide annotations to improve predictions
        -S  <HSP_filename>: Using information from protein sequence alignments to improve predictions

        -u: Turn on UTR prediction. Only valid with -S option: HSP/EST/short read ends are used to determine UTR ends
        -E: Add this value to the exon weight parameter (see parameter file)
        -V: Add this value to the score of evidence exons
        -P  <parameter_file>: Use other than default parameter file (human)

        -B: Display memory required to execute geneid given a sequence
        -v: Verbose. Display info messages
        -h: Show this help
AUTHORS
        geneid_v1.4 has been developed by Enrique Blanco, Tyler Alioto and Roderic Guigo.
        Parameter files have been created by Genis Parra and Tyler Alioto. Any bug or suggestion
        can be reported to geneid@crg.es




```


Fantabulous! Work well!Intruction for installation came from [here](https://github.com/guigolab/geneid)




### GlimmerHMM installation

I went [here](https://ccb.jhu.edu/software/glimmerhmm/)


Then


```
wget https://ccb.jhu.edu/software/glimmerhmm/dl/GlimmerHMM-3.0.4.tar.gz

tar tar -xzf GlimmerHMM-3.0.4.tar.gz

```

check in the bin


./glimmerhmm_linux_x86_64


got this


```
USAGE:  ./glimmerhmm_linux_x86_64 <genome1-file> <training-dir-for-genome1> [options]
Options:
-p file_name     If protein domain searches are available, read them from file file_name
-d dir_name      Training directory is specified by dir_name (introduced for compatibility with earlier versions)
-o file_name     Print output in file_name; if n>1 for top best predictions, output is in file_name.1, file_name.2, ... , file_name.n f
-n n             Print top n best predictions
-g               Print output in gff format
-v               Don't use svm splice site predictions
-f               Don't make partial gene predictions
-h               Display the options of the program

```

test `./glimmhmm.pl`

```
./glimmhmm.pl
```

get this

```
Usage: glimmhmm.pl <glimmerhmm_program> <fasta_file> <train_dir> <options>
```



Work fine!


### SNAP installation



```
wget https://github.com/KorfLab/SNAP/archive/master.zip

unzip SNAP-master

make

./snap [options] <HMM file> <FASTA file> [options] # run SNAP

```



#### Install EVM


```

wget https://github.com/EVidenceModeler/EVidenceModeler/archive/v1.1.1.tar.gz

tar -xzvf v1.1.1.tar.gz

```


[EVM resource link](https://evidencemodeler.github.io/#Obtaining_EVM)




#### Installation of PASA   



[TUTO](https://www.jianshu.com/p/d876a6d97625)

Just find awesome tutorials in genomics from Jianshu [1](https://www.jianshu.com/p/5842509e47f2) | [2](https://www.jianshu.com/p/a748d3a5421d) | [3](https://www.jianshu.com/p/ff781b0bb5e4) | [4](https://www.jianshu.com/p/7342451d82db) | [5](https://www.jianshu.com/p/9e8308b6a208) | [6](https://www.jianshu.com/p/ac8ebf0361cc) 

awesome tutorials [a](https://www.plob.org/article/7158.html)



######## TUTO pasa [1](https://www.plob.org/article/7158.html)
######## TUTO pasa [2](https://www.jianshu.com/p/d876a6d97625)
######## TUTO pasa [3](https://www.cnblogs.com/zhanmaomao/p/12456073.html)

I think I need to set MySQL first the 

[github](https://github.com/PASApipeline/PASApipeline/wiki/setting-up-pasa-mysql)



[genewise](http://www2.clst.riken.jp/phylo/genewise_installation.html)

[genewise1](https://iamphioxus.org/2015/04/26/install-genewise-on-the-centosrhel-system/)

[genewise2](http://avrilomics.blogspot.com/2012/11/installing-genewise-wise2.html)


[pheatmap](https://www.plob.org/article/16698.html)



[ka_ks](https://www.jianshu.com/p/6a179eef9cfe)



[jvci](https://www.jianshu.com/p/a748d3a5421d)

[augustus](https://iamphioxus.org/2017/05/08/installing-augustus-with-manual-bamtools-installation/)


awesome tuto [go analysis](https://www.cnblogs.com/zhanmaomao/p/11529589.html) | [go analysis](https://www.programmersought.com/article/12434648867/)

##### MySQL installation

Enable the MySQL 8.0 repository:


```
sudo yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm

```

Install MySQL 8.0 package with yum:

```

sudo yum install mysql-community-server

```


Starting MySQL

```
sudo systemctl start mysqld

```



During the installation process, a temporary password is generated for the MySQL root user. Locate it in the mysqld.log with this command

```
sudo grep 'temporary password' /var/log/mysqld.log

```


I got this


```
2021-01-02T23:07:27.740244Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: WH-YNg7iSU9y
```



Configuring MySQL
Run the security script:

```
sudo mysql_secure_installation

```

I got

```
[sudo] password for kplee:

```

I entered the root password

got this

```
Securing the MySQL server deployment.

Enter password for user root:
```
I entered the temporarely code

got this

```

The existing password for the user account root has expired. Please set a new password.

New password:

```

> Note: Enter a new 12-character password that contains at least one uppercase letter, one lowercase letter, one number and one special character. Re-enter it when prompted.


Got this

```
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.
Using existing password for root.

Estimated strength of the password: 100
Change the password for root ? ((Press y|Y for Yes, any other key for No) : *No*

 ... skipping.
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : *Yes*
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : *Yes*
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : *Yes*
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : *Yes*
Success.

All done!


```


Connection to MySQL

To log in to the MySQL server as the root user type:


```
mysql -u root -p

```



got this


```
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 8.0.22 MySQL Community Server - GPL

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>


```



I FOLLOWED THIS TUTO FOR THE [INSTALLATION](https://medium.com/@hbayraktar/how-to-install-mysql-on-centos-7-2c2dc0207fc1)



Install mysql-connector


```

wget https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm # Download latest mysql80-community-release-el7 rpm

sudo rpm -Uvh mysql80-community-release-el7-2.noarch.rpm # Install mysql80-community-release rpm

sudo yum install mysql-connector-c++-devel # Install mysql-connector


```



Now let us install openssl


Updating System Packages on CentOS

```
sudo yum update

```

check the version of OpenSSL installed



```
openssl version -a

```


got this


```

OpenSSL 1.0.2k-fips  26 Jan 2017
built on: reproducible build, date unspecified
platform: linux-x86_64
options:  bn(64,64) md2(int) rc4(16x,int) des(idx,cisc,16,int) idea(int) blowfish(idx)
compiler: gcc -I. -I.. -I../include  -fPIC -DOPENSSL_PIC -DZLIB -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DKRB5_MIT -m64 -DL_ENDIAN -Wall -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches   -m64 -mtune=generic -Wa,--noexecstack -DPURIFY -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_MONT5 -DOPENSSL_BN_ASM_GF2m -DRC4_ASM -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DAES_ASM -DVPAES_ASM -DBSAES_ASM -DWHIRLPOOL_ASM -DGHASH_ASM -DECP_NISTZ256_ASM
OPENSSLDIR: "/etc/pki/tls"
engines:  rdrand dynamic

```




Step 1: Install dependencies

```
sudo yum install perl-core zlib-devel -y

```

Step 2: Download OpenSSL


```
cd /usr/local/src/
sudo wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz
sudo tar -xf openssl-1.1.1c.tar.gz
cd openssl-1.1.1c

```

Step 3: Install OpenSSL


```
sudo ./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl shared zlib
```


got this


```
Operating system: x86_64-whatever-linux2
Configuring OpenSSL version 1.1.1c (0x1010103fL) for linux-x86_64
Using os-specific seed configuration
Creating configdata.pm
Creating Makefile

**********************************************************************
***                                                                ***
***   OpenSSL has been successfully configured                     ***
***                                                                ***
***   If you encounter a problem while building, please open an    ***
***   issue on GitHub <https://github.com/openssl/openssl/issues>  ***
***   and include the output from the following command:           ***
***                                                                ***
***       perl configdata.pm --dump                                ***
***                                                                ***
***   (If you are new to OpenSSL, you might want to consult the    ***
***   'Troubleshooting' section in the INSTALL file first)         ***
***                                                                ***
**********************************************************************

```


then


```
sudo make
sudo make test
sudo make install

```


Step 4: Configure OpenSSL Shared Libraries



```
cd /etc/ld.so.conf.d/
sudo vi openssl-1.1.1c.conf

#Ensure to save before you exit.

#Next, reload the dynamic link by issuing the command below:

sudo ldconfig -v

```




Step 5: Configure OpenSSL Binary


```
First, backup the default OpenSSL binary files.

sudo mv /bin/openssl /bin/openssl.backup
Next, create new environment files for OpenSSL:

sudo nano /etc/profile.d/openssl.sh
Enter the following:

#Set OPENSSL_PATH
OPENSSL_PATH="/usr/local/ssl/bin"
export OPENSSL_PATH
PATH=$PATH:$OPENSSL_PATH
export PATH
Ensure to save before you exit.

Next, make the openssl.sh file executable by issuing the command below:

sudo chmod +x /etc/profile.d/openssl.sh

Next, reload the OpenSSL environment and check the PATH bin directory using commands below:

source /etc/profile.d/openssl.sh
echo $PATH


We can now check and verify our installation of the latest stable version of OpenSSL using the command below:

 which openssl
 openssl version -a
 
```


And voila!   Inspirationnal tuto [here](https://cloudwafer.com/blog/installing-openssl-on-centos-7/)


# Install perl DBD::mysql module

```

sudo yum install "perl(DBD::mysql)"  # tuto [here](https://metacpan.org/pod/distribution/DBD-mysql/lib/DBD/mysql/INSTALL.pod)

```

Now check that it works:

```
perl -MDBD::mysql -e 'print "Hello there!";'

```


Install perl-DB_File

```
sudo yum install perl-DB_File   # [source](https://metacpan.org/pod/distribution/DBD-mysql/lib/DBD/mysql/INSTALL.pod)

```

setting up mysql for pasa use

I will do later


Intsall Tom Wu's GMAP cdna alignment utility.



```
$ wget http://research-pub.gene.com/gmap/src/gmap-gsnap-2020-12-17.tar.gz
$ tar zxvf gmap-gsnap-2020-12-17.tar.gz
$ cd gmap-2020-12-17
$ ./configure --prefix=$pwd
$ make -j 8
$ sudo make install

```

Install Jim Kent's BLAT aligner


```

$ sudo yum install libpng-devel
$ wget http://hgwdev.cse.ucsc.edu/~kent/src/blatSrc35.zip
$ unzip blatSrc35.zip
$ cd blatSrc
$ MACHTYPE=x86_64-redhat-linux-gnu          
$ export MACHTYPE
$ mkdir -p ~/bin/x86_64-redhat-linux-gnu   
$ make 




fail so I used this from this [post](https://www.biostars.org/p/291102/) 


$ echo $MACHTYPE
x86_64-pc-linux-gnu
$ MACHTYPE=x86_64
$ export MACHTYPE
$ echo $MACHTYPE 
x86_64
$ export PATH=~/bin/x86_64::$PATH
$ make

I refreshed the PATH variable with source ~/.bashrc and then I ran

blat

```

Install Bill Pearson's FASTA general sequence alignment utility



```

$ wget https://faculty.virginia.edu/wrpearson/fasta/fasta3/fasta-36.3.8h.tar.gz
$ tar zxvf fasta-36.3.8h.tar.gz
$ cd fasta-36.3.8h
$ cd src
$ make -f ../make/Makefile.linux_sse2 all
$ cd ..
$ ln -s $PWD/bin/fasta36 ~/bin/fasta




```



Install pasa




```

$ wget https://github.com/PASApipeline/PASApipeline/releases/download/pasa-v2.4.1/PASApipeline.v2.4.1.FULL.tar.gz
$ tar zxvf PASApipeline.v2.4.1.FULL.tar.gz
$ make

```

MySQL configuration

$ mysql -u root -p

# create a 'pasa_write' user destined for reading/writing/creating capabilities
mysql> create user 'pasa_write'@'localhost' identified by 'pasa_write_pwd';
> mysql> CREATE USER 'kplee'@'localhost' IDENTIFIED BY 'kpleeSESAME2_270190'; #I used this


# this below will allow user 'pasa_write' to create databases named with a _pasa suffix 
 # (and so not just any database name)
 
 
mysql> grant all privileges on `%_pasa`.* to 'pasa_write'@'localhost';
> mysql> GRANT ALL PRIVILEGES ON `%_pasa`.* TO 'kplee'@'localhost'; # I used this
> mysql> GRANT ALL PRIVILEGES ON *.* TO 'kplee'@'localhost'; # I change later


# create a 'pasa_access' user that we'll use for read-only database access.
 # this is useful for generating reports, pasa-web access, etc., where read-only is best.
 
> mysql> CREATE USER 'pasa'@'localhost' IDENTIFIED BY 'kpleeSESAME_270190';



[tuto](https://linuxize.com/post/how-to-create-mysql-user-accounts-and-grant-privileges/#:~:text=To%20create%20a%20new%20MySQL,user_password%20with%20the%20user%20password.)

# set config file

```
cd /home/kplee/program/PASApipeline.v2.4.1/pasa_conf

cp pasa.CONFIG.template conf.txt

vi conf.txt

```



Do that

```

#####################################
#### MANDATORY SETTINGS #############
#####################################

# This file is not used if SQLITEDB is set in the alignment assembly configuration file
## MySQL settings:

# server actively running MySQL
# MYSQLSERVER=server.com
MYSQLSERVER=localhost
# Pass socket connections through Perl DBI syntax e.g. MYSQLSERVER=mysql_socket=/tmp/mysql.sock

# read-write username and password
MYSQL_RW_USER=myid
MYSQL_RW_PASSWORD=mypassword

#### END OF MANDATORY SETTINGS ######


```















