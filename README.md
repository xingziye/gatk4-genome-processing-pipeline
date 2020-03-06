# gatk4-genome-processing-pipeline
Workflows used for germline processing in whole genome sequence data.

1. Getting started with Cromwell. You can try to follow instructions on [Five minute Introduction to Cromwell](https://cromwell.readthedocs.io/en/stable/tutorials/FiveMinuteIntro/) and run your first “hello world” workflow locally. This tutorial is based on Cromwell release 49.

1. It is also a good time to get yourself familiar with WDL. Even though you are not required to write your own workflow, it is not bad thing to get an idea how this pipeline is being implemented. Please see [WDL Documentation](https://support.terra.bio/hc/en-us/sections/360007274612) for more detailed information.

1. Follow the steps from [Configuring a Google Project](https://cromwell.readthedocs.io/en/stable/tutorials/PipelinesApi101/#lets-get-started) to setup your credentials and any necessary resources.

1. For the backend of Cromwell, this `google.conf` template file is provided in this repository. Please replace `<google-project-id>` and `<google-bucket-name>` with your corresponding values.

1. Now we should be ready to run our workflow on Google Cloud.
```console
java -Dconfig.file=google.conf -jar cromwell-49.jar run WholeGenomeGermlineSingleSample.wdl -i WholeGenomeGermlineSingleSample.inputs.json
```
Change the value to your path accordingly and execute the command. This workflow could take hours to finish, and the outputs of your pipeline should be available in your Cloud Storage bucket when it is done.

### WholeGenomeGermlineSingleSample :
This WDL pipeline implements data pre-processing and initial variant calling (GVCF
generation) according to the GATK Best Practices (June 2016) for germline SNP and
Indel discovery in human whole-genome sequencing data.

#### Requirements/expectations
- Human whole-genome paired-end sequencing data in unmapped BAM (uBAM) format
- One or more read groups, one per uBAM file, all belonging to a single sample (SM)
- Input uBAM files must additionally comply with the following requirements:
- - filenames all have the same suffix (we use ".unmapped.bam")
- - files must pass validation by ValidateSamFile
- - reads are provided in query-sorted order
- - all reads must have an RG tag
- Reference genome must be Hg38 with ALT contigs

#### Outputs
- Cram, cram index, and cram md5
- GVCF and its gvcf index
- BQSR Report
- Several Summary Metrics

### Software version requirements :
- GATK 4.0.10.1
- Picard 2.20.0-SNAPSHOT
- Samtools 1.3.1
- Python 2.7
- Cromwell version support
  - Successfully tested on v47
  - Does not work on versions < v23 due to output syntax

### Important Notes :
- The provided JSON is meant to be a ready to use example JSON template of the workflow. It is the user’s responsibility to correctly set the reference and resource input variables using the [GATK Tool and Tutorial Documentations](https://gatk.broadinstitute.org/hc/en-us/categories/360002310591).
- Relevant reference and resources bundles can be accessed in [Resource Bundle](https://gatk.broadinstitute.org/hc/en-us/articles/360036212652).
- Runtime parameters are optimized for Broad's Google Cloud Platform implementation.
- For help running workflows on the Google Cloud Platform or locally please
view the following tutorial [(How to) Execute Workflows from the gatk-workflows Git Organization](https://gatk.broadinstitute.org/hc/en-us/articles/360035530952).
- The following material is provided by the GATK Team. Please post any questions or concerns to one of our forum sites : [GATK](https://gatk.broadinstitute.org/hc/en-us/community/topics) and  [Terra](https://broadinstitute.zendesk.com/hc/en-us/community/topics/360000500432-General-Discussion).
- Please visit the [User Guide](https://gatk.broadinstitute.org/hc/en-us) site for further documentation on our workflows and tools.

### LICENSING :
Copyright Broad Institute, 2019 | BSD-3
This script is released under the WDL open source code license (BSD-3) (full license text at https://github.com/openwdl/wdl/blob/master/LICENSE). Note however that the programs it calls may be subject to different licenses. Users are responsible for checking that they are authorized to run all programs before running this script.
- [GATK](https://software.broadinstitute.org/gatk/download/licensing.php)
- [BWA](http://bio-bwa.sourceforge.net/bwa.shtml#13)
- [Picard](https://broadinstitute.github.io/picard/)
- [Samtools](http://www.htslib.org/terms/)
