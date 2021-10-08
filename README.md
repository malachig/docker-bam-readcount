docker-bam-readcount
====================

Docker image based on Ubuntu Focal with `bam-readcount` `v1.0.0` built
and installed. Note that this does *not* build from `master`, but the
`v1.0.0` tag. Edit the `Dockerfile` to use `master` or a different branch
or tag.

This Dockerfile has a corresponding build in Dockerhub https://hub.docker.com/r/mgibio/bam-readcount

    docker pull mgibio/bam-readcount

The `bam-readcount` binary is installed as

    /usr/bin/bam-readcount


Resources
---------

https://github.com/genome/bam-readcount  
`bam-readcount` source and documentation

https://docs.docker.com/  
Docker documentation


Usage (via `ENTRYPOINT`)
------------------------

The Dockerfile sets

    ENTRYPOINT ["/usr/bin/bam-readcount"]

so the container can be run as an executable.

For example, to run `bam-readcount` on a FASTA reference `REFERENCE` and
BAM file `BAMFILE` in the current working directory, you can do

    # --rm 
    # Remove container when done
    # -v $(pwd):$(pwd)
    # Maps the current working directory to the same path inside the container
    # -w $(pwd) 
    # Sets the working directory to the current working directory inside
    # the container
    docker run --rm -v $(pwd):$(pwd) -w $(pwd) mgibio/bam-readcount -f REFERENCE BAMFILE


Usage (interactive)
-------------------

The `ENTRYPOINT` can be overridden for interactive use. 

For example, to run `bam-readcount` on test data included in the container,
do

    # --rm 
    # Remove container when done
    # -it
    # Docker interactive flags
    # --entrypoint /bin/bash
    # Set the Bash shell as the entrypoint
    docker run --rm -it --entrypoint /bin/bash mgibio/bam-readcount

The container will present an interactive prompt, and the working directory
will be `/opt/bam-readcount`. There is a directory `test-data` there, and 
`bam-readcount` can be run on this data with 

    # -w1 
    # The test BAM does not have the SM tag; -w1 will report warnings 
    # only once to avoid a warning for each read
    root@3a0a8df8b278:/opt/bam-readcount# bam-readcount -w1 -f test-data/ref.fa test-data/test.bam

To make your own data available for interactive use within the container,
use the Docker `-v` and `-w` flags to map your local directories and 
set a working directory, for example to use the current working local 
directory

    docker run --rm -it -v $(pwd):$(pwd) -w $(pwd) --entrypoint /bin/bash mgibio/bam-readcount



