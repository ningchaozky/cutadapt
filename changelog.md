# Changelog #

**November 26, 2014:** If you get the message `OSError: [Errno 2] No such file or directory: 'cutadapt/_align.pyx'`, then please update to cutadapt 1.7.1, which fixes the error.

**November 25, 2014:** Cutadapt 1.7 is now available.

  * IUPAC characters are now supported. For example, use `-a YACGT` for an adapter that matches both `CACGT` and `TACGT` with zero errors. Disable with `-N`. By default, IUPAC characters in the read are not interpreted in order to avoid matches in reads that consist of many (low-quality) `N` bases. Use `--match-read-wildcards` to enable them also in the read.
  * Support for demultiplexing was added. This means that reads can be written to different files depending on which adapter was found. See [the section in the documentation](http://cutadapt.readthedocs.org/en/latest/guide.html#demultiplexing) for how to use it. This is currently only supported for single-end reads.
  * Add support for anchored 3' adapters. Append `$` to the adapter sequence to force the adapter to appear in the end of the read (as a suffix). Closes [issue #81](https://code.google.com/p/cutadapt/issues/detail?id=#81).
  * Option `--cut` (`-u`) can now be specified twice, once for each end of the read. Thanks to Rasmus Borup Hansen for the patch!
  * Options `--minimum-length`/`--maximum-length` (`-m`/`-M`) can be used standalone. That is, cutadapt can be used to filter reads by length without trimming adapters.
  * Fix bug: Adapters read from a FASTA file can now be anchored.

**October 6, 2014**: Cutadapt v1.6 is ready for download. There are no new features, but some bugs have been fixed and the documentation has been updated quite a bit.

  * Fix [issue 77](https://code.google.com/p/cutadapt/issues/detail?id=77): Ensure `--format=...` can be used even with paired-end input.
  * Fix [issue 79](https://code.google.com/p/cutadapt/issues/detail?id=79): Sometimes output files would be incomplete because they were not closed correctly.
  * Extensive work on the documentation. It's now available at https://cutadapt.readthedocs.org/ .
  * The alignment algorithm is a tiny bit faster.
  * For 3' adapters, statistics about the bases preceding the trimmed adapter are collected and printed. If one of the bases is overrepresented, a warning is shown since this points to an incomplete adapter sequence. This happens, for example, when a TruSeq adapter is used but the A overhang is not taken into account when running cutadapt.
  * Duo to code cleanup, there is a change in behavior: If you use `--discard-trimmed` or `--discard-untrimmed` in combination with `--too-short-output` or `--too-long-output`, then cutadapt now writes also the discarded reads to the output files given by the `--too-short` or `--too-long` options. If anyone complains, I will consider reverting this.
  * Galaxy support files are now in a separate repository at https://bitbucket.org/lance_parsons/cutadapt_galaxy_wrapper . Thanks to Lance Parsons for his work on this.

**August 5, 2014**: Cutadapt v1.5 has been released with the following improvements:

  * Adapter sequences can now be read from a FASTA file. For example, write `-a file:adapters.fasta` to read 3' adapters from `adapters.fasta`. This works also for `-b` and `-g`. This fixes the long-standing [issue #33](https://code.google.com/p/cutadapt/issues/detail?id=#33). Note that cutadapt isn't really optimized for trimming dozens or even hundreds of adapters!
  * There is now an option `--mask-adapter`, which can be used to not remove adapters, but to instead mask them with `N` characters. Thanks to Vittorio Zamboni for contributing this feature!
  * U characters in the adapter sequence are automatically converted to T.
  * Add the option `-u`/`--cut`, which can be used to unconditionally remove a number of bases from the beginning or end of each read.
  * When the new option `--quiet` is used, no report is printed after all reads have been processed.
  * When processing paired-end reads, cutadapt now checks whether the reads are properly paired.
  * To handle paired-end reads, an option --untrimmed-paired-output was added.

Minor changes:
  * Do not run Cython at installation time unless the --cython option is provided.
  * Make `--zero-cap` the default for colorspace reads.

**March 15, 2014:** Cutadapt version 1.4.1 fixes a small problem in the installation procedure. If you were not able to install cutadapt successfully, try this version.

**March 13, 2014:** Cutadapt version 1.4 has been released. This release reduces the overhead of reading and writing files. On my test data set, a typical run of cutadapt (with a single adapter) takes 40% less time. These are the detailed changes:
  * Reading and writing of FASTQ files is faster (thanks to Cython).
  * Reading and writing of gzipped files is faster (up to 2x) on systems where the `gzip` program is available.
  * The quality trimming function is four times faster (also due to Cython).
  * Fix the statistics output for 3' colorspace adapters: The reported lengths were one too short. Thanks to Frank Wessely for reporting this.
  * Support the `--no-indels` option. This disallows insertions and deletions while aligning the adapter. Currently, the option is only available for anchored 5' adapters. This fixes [issue 69](https://code.google.com/p/cutadapt/issues/detail?id=69).
  * As a sideeffect of implementing the --no-indels option: For colorspace, the length of a read (for `--minimum- and --maximum-length`) is now computed after primer base removal (when `--trim-primer` is specified).
  * Added one column to the info file that contains the name of the found adapter.
  * Add an explanation about colorspace ambiguity to the README


**November 8, 2013**: After a long hiatus, cutadapt v1.3 has been released. These are the changes:

  * There is now preliminary support for paired-end adapter trimming using the new `--paired-output` option (contributed by James Casbon). See the [section in the README](https://github.com/marcelm/cutadapt/blob/master/README.md#paired-end-adapter-trimming) for how to use it.
  * Add the `--too-long-output` option.
  * Add the `--no-trim option`, contributed by Dave Lawrence.
  * Port handwritten C alignment module to Cython.
  * Slightly speed up alignment of 5' adapters.
  * Improve statistics output.
  * Support bzip2-compressed files.
  * Fix the `--rest-file` option ([issue 56](https://code.google.com/p/cutadapt/issues/detail?id=56)), and various other small fixes.



**November 30, 2012**: cutadapt v1.2 has been released.

  * At least 25% faster processing of .csfasta/.qual files due to faster parser.
  * Between 10% and 30% faster writing of gzip-compressed output files.
  * Support 5' adapters in color space, even when no primer trimming is requested.
  * Use the `--info-file' option to write further information about the found adapters in each read to a separate file.
  * Named adapters are now possible. Use `-a My_Adapter=ACCGTA` to assign the name "My\_adapter" to an adapter.
  * Improve alignment algorithm for better poly-A trimming when there are sequencing errors. Previously, not the longest possible poly-A tail would be trimmed.
  * James Casbon contributed the `--discard-untrimmed` option.

**June 18, 2012:** cutadapt v1.1 has been released. This version is faster than previous versions and now supports 5' colorspace adapters. Note: If you use cutadapt without installation, you now need to type `bin/cutadapt` (instad of `./cutadapt`).

The changes in this release are:

  * Speed up the alignment by a factor of at least 3 by using Ukkonen's algorithm. The total runtime decreases by about 30% in the tested cases.
  * Speedup of approx. 25% when reading from .gz files and using Python 2.7.
  * When using cutadapt without installing it, you now need to run `bin/cutadapt` due to a new directory layout.
  * Allow to "anchor" 5' adapters (`-g`), forcing them to be a prefix of the read. To use this, add the special character `^` to the beginning of the adapter sequence.
  * Add the "-N" option, which allows 'N' characters within adapters to match literally.
  * Allow to only trim qualities when no adapter is given on the command-line.
  * Add a patch by James Casbon: include read names (ids) in rest file
  * Use nosetest for testing. To run, install nose and simply run `nosetests`.
  * Allow to give a colorspace adapter in basespace (gets automatically converted).
  * Allow to search for 5' adapters (those specified with `-g`) in colorspace.
  * allow to deal with colorspace FASTQ files from the SRA that contain a fake additional quality in the beginning (use `--format sra-fastq`)


**November 4, 2011:** cutadapt v1.0 has been released. This special version number does not mark fundamental changes, it merely indicates that cutadapt is now a mature tool. Thanks also to all external contributors for their work on improving the tool!

The changes in this release are:
  * ASCII-encoded quality values were assumed to be encoded as ascii(quality+33). With the new parameter --quality-base, FASTQ files with qualities encoded as ascii(quality+64) as used in some versions of the Illumina pipeline can be read (fixes [issue 7](https://code.google.com/p/cutadapt/issues/detail?id=7).)
  * Allow to specify that adapters were ligated to the 5' end of reads. This change is based on a patch contributed by James Casbon.
  * Add Galaxy support, contributed by Lance Parsons.
  * Patch by James Casbon: Allow N wildcards in read or adapter or both. Wildcard matching of 'N's in the adapter is always done. If 'N's within reads should also match without counting as error, this needs to be explicitly requested via --match-read-wildcards.

**July 20, 2011:** cutadapt v0.9.5 has been released. Please note that this version produces slightly different results compared to older versions! The changes are:

  * Fix [issue 20](https://code.google.com/p/cutadapt/issues/detail?id=20): Make the report go to standard output when -o/--output is specified.
  * Recognize .fq as an extension for FASTQ files
  * Add more unit tests (run them with `cd tests; ./tests.sh`)
  * The alignment algorithm has changed. It will now find some adapters that previously were missed.

With the old alignment algorithm, finding an adapter would work as follows:
  1. Find an alignment between adapter and read -- longer alignments are better.
  1. If the number of errors in the alignment (divided by length) is above the maximum error rate, report the adapter as not being found.
Sometimes, the long alignment that was found had too many errors, but a shorter alignment would not. The adapter was then incorrectly seen as "not found". The new alignment algorithm checks the error rate while aligning and only reports alignments that do not have too many errors.


**May 20, 2011:** cutadapt v0.9.4 has been released. These are the changes:
  * Now compatible with Python 3.
  * Add the --zero-cap option, which changes negative quality values to zero.
This is a workaround to avoid segmentation faults in BWA. The option is now
enabled by default when --bwa/--maq is used.
  * Lots of unit tests added. Run them with cd tests && ./tests.sh .
  * Fix [issue 16](https://code.google.com/p/cutadapt/issues/detail?id=16): --discard-trimmed did not work.
  * Allow to override auto-detection of input file format with the new -f/--format parameter. This mostly fixes [issue 12](https://code.google.com/p/cutadapt/issues/detail?id=12).
  * Don't break when the input file is empty.

**March 16, 2011:** cutadapt v0.9.3 has been released. (**Note:** v0.9.2 had an incorrect manifest file. If you already have it and can run it, don't worry.)
  * Case is now ignored when comparing the adapter with the read sequence.
  * .FASTA/.QUAL files (not necessarily in color space) can now be read (some 454 software uses this format)
  * Allow to input FASTA/FASTQ on standard input, fixing [issue 10](https://code.google.com/p/cutadapt/issues/detail?id=10).
  * The package has been refactored a lot. It now installs a single 'cutadapt' Python package instead of multiple Python modules. This avoids cluttering the global namespace and should lead to less problems with other Python modules. Thanks to Steve Lianoglou for pointing this out to me!

**January 10, 2011:** cutadapt v0.9 is available. New features are:
  * Use --too-short-output and --untrimmed-output to redirect too short or untrimmed reads to a separate file, based on patch by Paul Ryvkin (thanks!).
  * With --maximum-length, reads longer than a specified length can be discarded.
  * Added the --length-tag option, which helps to fix read lengths in FASTA/Q comment lines  (e.g., 'length=123' becomes 'length=58' after trimming) (requested by Paul Ryvkin)
  * add -q/--quality-cutoff option for trimming low-quality ends (uses the same algorithm as BWA)

**December 9, 2010:** cutadapt v0.8 has been released. Important changes are:
  * The default behavior now is to assume that an adapter has been ligated to the 3' end. This should be the correct  behavior for at least the SOLiD small RNA protocol (SREK) and also for the Illumina protocol. See the README for details.
  * Different scoring function improves trimming: Some reads that should have been trimmed weren't.
  * 20% faster

**December 3, 2010:** cutadapt v0.7 has been released. Most visible change in this release is the support for discarding reads that are shorter than a specified length after trimming (--minimum-length or -m parameter). Error reporting was improved when encountering malformed files.

**November 18, 2010:** cutadapt v0.6 supports gzipped input and output.

**November 17, 2010:** cutadapt v0.5.1 has been released. This release adds the option "--discard" to discard reads matching an adapter. (This deprecates 0.5, which contained a syntax error in the setup script.)

**November 17, 2010:** cutadapt v0.4 has been released. This release fixes a bug that occurred when dealing with multiple adapters.

**September 27, 2010:** cutadapt v0.3 has been released. This version fixes a bug that led to huge memory usage when reading CSFASTA/QUAL files.