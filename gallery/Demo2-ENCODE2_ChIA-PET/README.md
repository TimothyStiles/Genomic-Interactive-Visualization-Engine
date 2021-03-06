# [ChIA-PET long-range chromatin interactions](https://chiapet.givengine.org/)

This genome browser demo presents 15 datasets of ChIA-PET long-range chromatin interactions along with human genome assembly GRCh37 (hg19) for comparative studies. ChIA-PET data were generated by [**ENCODE** Phase 2](https://www.encodeproject.org/matrix/?type=Experiment&assay_title=ChIA-PET&assembly=hg19&replicates.library.biosample.donor.organism.scientific_name=Homo+sapiens&award.rfa=ENCODE2&files.file_type=bed+bed12). The ChIA-PET chromatin interaction data includes 8 experiments in 5 cell lines (K562, MCF-7, HeLa-S3, HCT116 and NB4) with 3 kinds of target genes (POLR2A, CTCF and ESR1).

![fig](./GIVE_demo2_chiapet.PNG)

## Data preparation for GIVE
We used the ENCODE ChIA-PET long-range chromatin interactions bed (bed12) format data, which can be downloaded from [**ENCODE** Experiment Matrix](https://www.encodeproject.org/matrix/?type=Experiment&assay_title=ChIA-PET&assembly=hg19&replicates.library.biosample.donor.organism.scientific_name=Homo+sapiens&award.rfa=ENCODE2&files.file_type=bed+bed12). Alternatively, you can use [`batch_download.txt`](./batch_download.txt) with following shell command to download all the data.

```bash
## run the following command in linux shell 
xargs -n 1 curl -O -L < batch_download.txt
```
The following table shows all the 15 bed format data sets we can get from [**ENCODE** Phase 2](https://www.encodeproject.org/matrix/?type=Experiment&assay_title=ChIA-PET&assembly=hg19&replicates.library.biosample.donor.organism.scientific_name=Homo+sapiens&award.rfa=ENCODE2&files.file_type=bed+bed12).


|DataFile|Experiment Accession|Target gene|Cell line|Description|
| --- |--- |--- |--- |--- |
|ENCFF001THT.bed.gz|ENCSR000BZX|POLR2A|HCT116|POLR2A ChIA-PET on human HCT-116|
|ENCFF001THU.bed.gz|ENCSR000BZW|POLR2A|HeLa-S3|POLR2A ChIA-PET on human HeLa-S3|
|ENCFF001THV.bed.gz|ENCSR000CAC|CTCF|K562|CTCF ChIA-PET on human K562|
|ENCFF001THW.bed.gz|ENCSR000BZY|POLR2A|K562|POLR2A ChIA-PET on human K562|
|ENCFF001THX.bed.gz|ENCSR000CAD|CTCF|MCF-7|CTCF ChIA-PET on human MCF-7|
|ENCFF001THY.bed.gz|ENCSR000CAD|CTCF|MCF-7|CTCF ChIA-PET on human MCF-7|
|ENCFF001THZ.bed.gz|ENCSR000BZZ|ESR1|MCF-7|ESR1 ChIA-PET on human MCF-7|
|ENCFF001TIA.bed.gz|ENCSR000BZZ|ESR1|MCF-7|ESR1 ChIA-PET on human MCF-7|
|ENCFF001TIB.bed.gz|ENCSR000BZZ|ESR1|MCF-7|ESR1 ChIA-PET on human MCF-7|
|ENCFF001TIC.bed.gz|ENCSR000BZY|POLR2A|K562|POLR2A ChIA-PET on human K562|
|ENCFF001TID.bed.gz|ENCSR000CAA|POLR2A|MCF-7|POLR2A ChIA-PET on human MCF-7|
|ENCFF001TIE.bed.gz|ENCSR000CAA|POLR2A|MCF-7|POLR2A ChIA-PET on human MCF-7|
|ENCFF001TIF.bed.gz|ENCSR000CAA|POLR2A|MCF-7|POLR2A ChIA-PET on human MCF-7|
|ENCFF001TIG.bed.gz|ENCSR000CAB|POLR2A|NB4|POLR2A ChIA-PET on human NB4|
|ENCFF001TIJ.bed.gz|ENCSR000CAA|POLR2A|MCF-7|POLR2A ChIA-PET on human MCF-7|

The following table shows a sample of the bed12 format used in **ENCODE** datasets, and the full description can be found [here](https://genome.ucsc.edu/FAQ/FAQformat.html#format1). 

|chrom|chromStart|chromEnd|name|score|strand|thickStart|thickEnd|itermRgb|blockCount|blockSizes|blockStarts|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|chr1|3507144|3538145|chr1:3507144..3509308-chr1:3534421..3538145,3|300|.|3507144|3538145|255,0,0|2|2164,3724|0,27277|
|chr1|3507584|3520603|chr1:3507584..3509631-chr1:3518303..3520603,3|300|.|3507584|3520603|255,0,0|2|2047,2300|0,10719|
|chr1|761369|763199|chr1:761369..763199-chr8:182999..184804,2|200|.|761369|763199|255,0,0|1|1830|0|
|chr8|182999|184804|chr1:761369..763199-chr8:182999..184804,2|200|.|182999|184804|255,0,0|1|1805|0|

The first two rows in the sample show two intra-chromosome interactions, and the last two rows duplicates show one inter-chromosome interactions. So you may have found that we only need the `name` column to get all the interaction information. So we wrote a simple script [chiapet2give.sh](./chiapet2give.sh) to convert the datasets to the GIVE supported interaction bed format (the format definition can be found in [GIVE Manual](https://github.com/Zhong-Lab-UCSD/Genomic-Interactive-Visualization-Engine/blob/master/manuals/3.1-GIVE-Toolbox-usages.md#6-add_track_interactionsh)). Run following command results in converted GIVE interaction bed files, which are named with a prefix `give_x_`, such as `give_x_ENCFF001THU.bed.gz.bed`.

```bash
## run in linux shell
ls ENCFF*.bed.gz | xargs -n 1 -P 4 -I {} bash ../../chiapet2give.sh {} ./
```

The following table shows the GIVE interaction bed format. These datasets can be loaded to GIVE MySQL server. 

|ID|chrom|Start|End|linkID|value|dirFlag|
| --- | --- | --- | --- | --- | --- | --- |
|1|chr20|47889560|47895795|1|16.026310742063|-1|
|2|chr20|47896527|47898203|1|16.026310742063|-1|
|3|chr17|79827812|79838989|2|15.548584214411|-1|
|4|chr17|79848037|79871266|2|15.548584214411|-1|
|5|chr17|27046828|27048611|3|15.5357777367182|-1|
|6|chr17|27048612|27049990|3|15.5357777367182|-1|

## Build track in MariaDB
You need a server to build a genome browser with GIVE. Please read the [prerequisites and configuration of GIVE server](https://github.com/Zhong-Lab-UCSD/Genomic-Interactive-Visualization-Engine/blob/master/tutorials/2.2-custom-installation.md#prerequisites). In that tutorial page, you will also learn how to [prepare MariaDB database](https://github.com/Zhong-Lab-UCSD/Genomic-Interactive-Visualization-Engine/blob/master/tutorials/2.2-custom-installation.md#installing-give-server) and the GIVE Toolbox has information on how to [build a reference genome for GIVE](https://github.com/Zhong-Lab-UCSD/Genomic-Interactive-Visualization-Engine/blob/master/tutorials/3-GIVE-Toolbox.md#step-2-initialization-and-create-reference-genome). When you have prepared MariaDB and built a `hg19` database, you can use [`GIVE_chiapetTrack.sql`](./GIVE_chiapetTrack.sql) file and following command template to load all the datasets to MariaDB and build 15 tracks.

Alternatively, you can set up the [GIVE Docker container](https://github.com/Zhong-Lab-UCSD/Genomic-Interactive-Visualization-Engine/blob/master/tutorials/2.1-GIVE-Docker.md), which is mostly preconfigured.

```bash
## run these commands in linux shell
# change `<your user name>` to your user name of MariaDB
mysql -u `<your user name>` -p <./GIVE_chiapetTrack.sql
```
## Build genome browser
When you have built tracks in MariaDB, it's very easy to build a genome browser. The following code is what we used to build our [demo](https://chiapet.givengine.org/). You can just copy and paste it in [jsfiddle](https://jsfiddle.net/) and then you can get the genome browser supported by our GIVE server. If you have built the tracks on your own GIVE server, you only need to replace the url in the code with your own server's url. 

```html
<!-- change the url to your own server path -->
<script src="https://www.givengine.org/libWC/webcomponents-lite.min.js"></script> 
<!-- change the url to your own server path-->
<link rel="import" href="https://www.givengine.org/lib/chart-controller/chart-controller.html">
<!-- Embed the browser in your web page -->
<chart-controller title-text="ChIA-PET long-range chromatin interactions" 
    ref="hg19" num-of-subs="2" 
    coordinate='["chr17:7520037-7643128", "chr17:7441031-7588154"]'
    group-id-list='["genes", "ENCODE2_ChIA-PET", "customTracks"]'>
</chart-controller>
```

You can read this [tutorial](https://github.com/Zhong-Lab-UCSD/Genomic-Interactive-Visualization-Engine/blob/master/tutorials/1.2-html-tweak.md) to learn how to simply tweak the genome browser. Our [demo](https://chiapet.givengine.org/) is based on the tweaked [`give_chiapet.html`](./give_chiapet.html) HTML file.
