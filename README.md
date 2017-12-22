# fast5_in_ref

This is a useful Nanopore (ONT) script that generates a list of file
paths for `fast5` files that are contained within a `fastq`, `BAM`, or
`SAM` file of interest.

The output can be piped to another unix command to copy the fast5
files to a new directory or to save a list of file paths. This is
great for situations when you only need to share some, but not all,
reads of an ONT dataset.

### Which Python do I need?

This script should work for both python 2 and python 3.

### Installation

To install the script with git, you can clone it with github. Then,
change to the directory where you installed it (`cd`) and make the
script executable (`chmod`)

    git clone https://github.com/mbhall88/fast5_in_ref.git
    cd fast5_in_ref && chmod +x fast5_in_ref

### Usage

It's pretty straight-forward to use:

    ./fast5_in_ref -i <fast5_dir> -r <in.fastq|in.bam|in.sam> -o <out.txt>

The script will walk down into subdirectories as well, so you can just give it your directory containing all your files.

What it does is read in `<in.fastq|in.bam|in.sam>` and extract the read id from each header. It then goes through all the fast5 files under `<fast_dir>` and checks whether their read id is in the set of read ids from `<in.fastq|in.bam|in.sam>`. If it is, the path to the file is written to it's own line in `<out.txt>`.

If no output (`-o`) is given, it will write the output to `stdout`.

So if you wanted to pipe these paths into another program, you could do something like

    mkdir subset_dir/
    ./fast5_in_ref -i </path/to/fast5s/> -r <in.fastq> | xargs cp -t subset_dir/


The above example would copy the `fast5` files that are found in your `fastq` to `subset_dir/`.

If there are any issues (with the program) let me know.

### Dependencies
You need to have [`h5py`](https://github.com/h5py/h5py). You'll also need [`pysam`](https://github.com/pysam-developers/pysam) if you're going to be using SAM or BAM files. 

    pip install h5py
    pip install pysam
