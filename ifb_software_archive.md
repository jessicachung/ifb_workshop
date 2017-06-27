# IFB workshop software archive

Create tarball containing most of the software required for the workshop.
Archive and upload to the object store. In the Ansible playbook, download
and extract the tarball in each VM instance.

## Setup

Launch an instance of GVL 4.2. SSH in with the `ubuntu` user.

-----

## Install software

### Anaconda for 2.7

```bash
mkdir /mnt/transient_nfs/software/ && cd /mnt/transient_nfs/software/
wget https://repo.continuum.io/archive/Anaconda2-4.4.0-Linux-x86_64.sh
bash Anaconda2-4.4.0-Linux-x86_64.sh
# Accept license and use location: /mnt/transient_nfs/software/anaconda2
# Add to path
source ~/.bashrc
```

### BrowseVCF

Dependencies are installed via the Ansible playbook.

https://github.com/BSGOxford/BrowseVCF

```bash
mkdir browsevcf && cd browsevcf
wget https://github.com/BSGOxford/BrowseVCF/releases/download/v2.8/BrowseVCF_gnu_2.8.tar.gz
tar xzf BrowseVCF_gnu_2.8.tar.gz
```

### GATK 3.7

Personal copy in my object store (will be removed later).

```bash
mkdir /mnt/transient_nfs/software/gatk && cd /mnt/transient_nfs/software/gatk
wget https://swift.rc.nectar.org.au:8888/v1/AUTH_377/jchung/GenomeAnalysisTK-3.7.tar.bz2
tar xjf GenomeAnalysisTK-3.7.tar.bz2
```

### Conda packages

```bash
conda install -c bioconda lumpy-sv=0.2.13
conda install -c bioconda delly=0.7.7
conda install -c bioconda snpeff=4.3.1p
conda install -c bioconda snpsift=4.3.1p
conda install -c bioconda picard=2.9.2
conda install -c bioconda fastqc=0.11.5
```

### CNVnator and dependencies
https://github.com/abyzovlab/CNVnator

Tried to install CNVnator by source, but couldn't get it to compile. Ended up
using conda packages

```bash
conda install -c remenska root=6.04
conda install -c pwwang cnvnator=0.3.2
```

### ERDS

```bash
mkdir /mnt/transient_nfs/software/erds && cd /mnt/transient_nfs/software/erds
wget http://www.utahresearch.org/wp-content/uploads/2014/01/erds1.1.tar
tar xf erds1.1.tar
```

### Annovar

Personal copy in my object store (will be removed later).

```bash
mkdir /mnt/transient_nfs/software/annovar && cd /mnt/transient_nfs/software/annovar
wget https://swift.rc.nectar.org.au:8888/v1/AUTH_377/jchung/annovar_2017-06-20.tar.gz
tar xzf annovar_2017-06-20.tar.gz
```

-----

## Annotation and workshop data

```bash
# Ensembl and dbNSFP?
# TODO after I get more details
```

-----

## Create archive

Clean up conda tarballs and create archive.

```bash
conda clean --tarballs --yes

tar czf ifb_workshop_test.tar.gz anaconda2 browsevcf erds gatk cnvnator
```

Create venv with swift.

```bash
virtualenv -p /usr/bin/python venv
source venv/bin/activate
pip install python-swiftclient
pip install python-keystoneclient
```

Enter credentials, then upload to object store.

```bash
swift upload jchung ifb_workshop_test.tar.gz
```
