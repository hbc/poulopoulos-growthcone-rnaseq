## Overview of Paolo's growthcone RNA-seq project

## Getting the data and setting everything up
1. Download files
```bash
wget --mirror https://software.rc.fas.harvard.edu/ngsdata/150831_D00365_0590_BHC7JMBCXX
wget --mirror https://software.rc.fas.harvard.edu/ngsdata/151001_D00365_0613_AH72FJBCXX
```

2. Combine all fastq files for each barcode
```bash
barcodes="BC5 BC6 BC7 BC8 BC9 BC10 BC11 BC12 BC13 BC14 BC15 BC16"
for barcode in $barcodes
do
find software.rc.fas.harvard.edu -name $barcode*R1.fastq.gz -exec cat {} \; > data/${barcode}_R1.fastq.gz
find software.rc.fas.harvard.edu -name $barcode*R2.fastq.gz -exec cat {} \; > data/${barcode}_R2.fastq.gz
done
```

3. There was a second run and a third run of the same samples, we added those in. These samples are not
biological replicates, they are reruns of the same samples so they are technical replicates.
4. There are barcodes 1-4 that are part of another experiment, so ignore those files.
5. We created a file that describes what each sample is called growthcone.csv.
6. Using the bcbio-nextgen template setup, set up the analysis:
```bash    
bcbio_nextgen.py -w template illumina-rnaseq.yaml growthcone.csv data
```
7. Run bcbio-nextgen.
8. Run bcbio.rnaseq to generate a skeleton report: 
```bash
bcbio-rnaseq summarise -f ~compartment project_summary.yaml
```
