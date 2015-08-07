_cutadapt_ removes adapter sequences from high-throughput sequencing data. This is usually necessary when the read length of the sequencing machine is longer than the molecule that is sequenced, for example when sequencing microRNAs.

# Installation #
If _pip_ is installed, then cutadapt can be installed using this command:
```
pip install cutadapt
```

# Documentation #

Cutadapt's documentation is available at [readthedocs](https://cutadapt.readthedocs.org).


# News #

**Juli 29, 2015:** Cutadapt 1.8.3 is a maintenance release that fixes [problems with 'pip install cutadapt' not working](https://github.com/marcelm/cutadapt/issues/138) and sometimes [hanging gzip processes with Python 2.6](https://github.com/marcelm/cutadapt/issues/137). To fix [issue 138](https://github.com/marcelm/cutadapt/issues/138), untrimmed reads are now shown in the info file.

**March 14, 2015:** Cutadapt 1.8 is available and brings with it major new features.

  * Support single-pass paired-end trimming with the new `-A`/`-G`/`-B`/`-U` parameters. These work just like their -a/-g/-b/-u counterparts, but they specify sequences that are removed from the **second read** in a pair.

> Also, if you start using one of those options, the read modification options such as `-q` (quality trimming) are applied to **both** reads. For backwards compatibility, read modifications are applied to the first read only if neither of `-A`/`-G`/`-B`/`-U` is used. See [the documentation](http://cutadapt.readthedocs.org/en/stable/guide.html#paired-end) for details.

> This feature has not been extensively tested, so please give feedback if something does not work!
  * The report output has been re-worked in order to accomodate the new paired-end trimming mode. This also changes the way the report looks like in single-end mode. It is hopefully now more accessible.
  * Chris Mitchell contributed a patch adding two new options: `--trim-n` removes any `N` bases from the read ends, and the `--max-n` option can be used to filter out reads with too many `N`s.
  * Support notation for repeated bases in the adapter sequence: Write `A{10}` instead of `AAAAAAAAAA`. Useful for poly-A trimming: Use `-a A{100}` to get the longest possible tail.
  * Quality trimming at the 5' end of reads is now supported. Use `-q 15,10` to trim the 5' end with a cutoff of 15 and the 3' end with a cutoff of 10.
  * Fix incorrectly reported statistics (> 100% trimmed bases) when `--times` set to a value greater than one.
  * Support .xz-compressed files (if running in Python 3.3 or later).
  * Started to use the [GitHub issue tracker](https://github.com/marcelm/cutadapt/issues) instead of Google Code. All old issues have been moved.

**August 2, 2011:** An [application note about cutadapt](http://journal.embnet.org/index.php/embnetjournal/article/view/200) has been published in [EMBnet.journal](http://journal.embnet.org/). Please be aware that cutadapt has gained some more features since that text was written. If you use cutadapt, I would be glad if you cite it.

Please see the [changelog](changelog.md) for older release announcements.

# Features #

  * Trims reads from current high-throughput sequencing machines (esp. Illumina, 454 and SOLiD).
  * Gapped alignment with mismatches and indels, that is, errors in the adapter are tolerated
  * Finds adapters both in the 5' and 3' ends of reads
  * Accepts FASTQ, FASTA or .csfasta and .qual files (for AB SOLiD data)
  * Any input or output file can be gzip-compressed (this is automatically detected)
  * Outputs FASTA or FASTQ
  * Trims color space reads correctly
  * Optionally removes primer base in color space data
  * Can produce MAQ- or BWA-compatible output

# Citation #

Here is a BibTeX entry for the cutadapt paper.
```
@ARTICLE{Martin2011Cutadapt,
  author = {Marcel Martin},
  title = {Cutadapt removes adapter sequences from high-throughput sequencing
	reads},
  journal = {EMBnet.journal},
  year = 2011,
  month = may,
  volume = 17,
  pages = {10--12},
  number = 1,
  doi = {http://dx.doi.org/10.14806/ej.17.1.200},
  url = {http://journal.embnet.org/index.php/embnetjournal/article/view/200}
}
```