# Soybean rhizosphere microbiome-soil type and cultivar impracts
  First run data
  

## Sequence decompress

```
cd /lustre/projects/staton/projects/soybean_strigolactones/16S_raw_fastq/soybean_strigolactone_16S_03_02_2017
for file in *; do echo $file; cd $file; cp * ../../fastq_gunzip/; cd ..; done
cd /lustre/projects/staton/projects/soybean_strigolactones/16S_raw_fastq/fastq_gunzip
gunzip *

```

## Demultiplex 


###Library prep protocol refer to### 

>Lundberg, D.S., Yourstone, S., Mieczkowski, P., Jones, C.D. and Dangl, J.L., 2013. Practical innovations for high-throughput amplicon sequencing. Nature methods, 10(10), pp.999-1002.

###Barcode1###
>F-Bc1_Fs2,4,6 seqeunce is *TGA....TCACTCCTACGGG.GGC.GCAG*

###Barcode2###
>F-Bc2_Fs1,3,5 sequence is *ACT....TCACTCCTACGGG.GGC.GCAG*

###demultiplex script and jobs###

**1) Read1 demultiplex scripts**


1) Read1 barcoded with ACT
```
# "$1" is read1.fastq, "$2" is sampleID that barcoded with TGA, "$3" is sampleID that barcoded with ACT
grep -B1 -A2 "ACT....TCACTCCTACGGG.GGC.GCAG" "$1" | grep -v "^--$" > "$3"_R1.fastq
grep "^@M04398" "$3"_R1.fastq > "$3"_header1.txt
awk '{print$1}' "$3"_header1.txt > "$3"_header2.txt
```
2) Read1 barcoded with TGA

```
# "$1" is read1.fastq, "$2" is sampleID that barcoded with TGA, "$3" is sampleID that barcoded with ACT
grep -B1 -A2 "TGA....TCACTCCTACGGG.GGC.GCAG" "$1" | grep -v "^--$" > "$2"_R1.fastq
grep "^@M04398" "$2"_R1.fastq > "$2"_header1.txt
awk '{print$1}' "$2"_header1.txt > "$2"_header2.txt

```
Read1 demultiplex job file

```
#$ -N demultiplex
#$ -cwd
#$ -q medium*

sh  demultiplex_update.sh  SAMPLE1_S1_L001_R1_001.fastq    Gene1_01      AgCV1_01
sh  demultiplex_update.sh  SAMPLE2_S2_L001_R1_001.fastq    Gene10_01     AgCV1_02
sh  demultiplex_update.sh  SAMPLE3_S3_L001_R1_001.fastq    Gene14_01     AgCV1_03
sh  demultiplex_update.sh  SAMPLE4_S4_L001_R1_001.fastq    CT_01         AgCV1_04
sh  demultiplex_update.sh  SAMPLE5_S5_L001_R1_001.fastq    HypIII_01     AgCV1_05
sh  demultiplex_update.sh  SAMPLE6_S6_L001_R1_001.fastq    Ag_B_01       AgCV1_06
sh  demultiplex_update.sh  SAMPLE7_S7_L001_R1_001.fastq    For_B_01      AgCV1_07
sh  demultiplex_update.sh  SAMPLE8_S8_L001_R1_001.fastq    AgCV1_09      AgCV1_08
sh  demultiplex_update.sh  SAMPLE9_S9_L001_R1_001.fastq    Gene1_02      AgCV2_01
sh  demultiplex_update.sh  SAMPLE10_S10_L001_R1_001.fastq  Gene10_02     AgCV2_02

          ...                          ...                    ...           ...
          ...                          ...                    ...           ...
          ...                          ...                    ...           ...
          
sh  demultiplex_update.sh  SAMPLE95_S95_L001_R1_001.fastq  For_B_12     ForCV6_07
sh  demultiplex_update.sh  SAMPLE96_S96_L001_R1_001.fastq  Blank        ForCV6_08
    
```
Here are some tricky stuff I got because dos and unix character different
Every file named by the fifth column of job file, got output got extra character, e.g., AgCV1_01.fastq becae AgCV1_01?.fastq
But those file names could not be recognized by bash command, mv does not work. So, here came with the solution:

```
for file in *; do mv "$file" "$(echo $file | sed s'/\?r//g')"; done
```

To avoid this error:
1) check job file using head | od -c to check if there are any \r characters
2) transfer file from dos to unix form by 
```
dos2unix filename

```

**2) Read2 demultiplex**

Using unpublished lab code - *extract_seq_list_from_fasta_file.py*
job file

```
#$ -N Read2_extract
#$ -cwd
#$ -q medium*

module load python/3.5.1
module load biopython/1.65


python  extract_seq_list_from_fasta_file.py     AgCV1_1_header2.txt     SAMPLE1_S1_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV1_2_header2.txt     SAMPLE2_S2_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV1_3_header2.txt     SAMPLE3_S3_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV1_4_header2.txt     SAMPLE4_S4_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV1_5_header2.txt     SAMPLE5_S5_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV1_6_header2.txt     SAMPLE6_S6_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV1_7_header2.txt     SAMPLE7_S7_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV1_8_header2.txt     SAMPLE8_S8_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     AgCV2_1_header2.txt     SAMPLE9_S9_L001_R2_001.fastq
                  ...                                  ...                      ...
                  ...                                  ...                      ...
                  ...                                  ...                      ...

python  extract_seq_list_from_fasta_file.py     ForCV6_3_header2.txt    SAMPLE91_S91_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     ForCV6_4_header2.txt    SAMPLE92_S92_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     ForCV6_5_header2.txt    SAMPLE93_S93_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     ForCV6_6_header2.txt    SAMPLE94_S94_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     ForCV6_7_header2.txt    SAMPLE95_S95_L001_R2_001.fastq
python  extract_seq_list_from_fasta_file.py     ForCV6_8_header2.txt    SAMPLE96_S96_L001_R2_001.fastq               

```
output are named like this *SAMPLE1_S1_L001_R2_001.fastq.filtered*

So, file name were then renamed from corresponding filter file to Read2 file
e.g., *mv SAMPLE1_S1_L001_R2_001.fastq.filtered   AgCV1_01_R2.fastq*

At the end, all of the demultiplexed read1 and corresponding read2 file were checked to make sure they have same number of lines after extraction

** Read file arrangement **
All of the Reads from Cultivar project are  

*Now read files are ready for subsequent analysis using mothur*

## Mothur sequence processing ##

Sequence preprocess refer to [MiSeq Sop] (https://www.mothur.org/wiki/MiSeq_SOP)

### Modification include ###

- custom 16S reference-- start and end location determination

Refer to [Git hub] (https://github.com/mothur/mothur/issues/235) 
In my case, the start location is 6428, and end location is 23444

- make file

After make.contigs:
 Start   End     NBases  Ambigs  Polymer NumSeqs
Minimum:        1       267     267     0       3       1
2.5%-tile:      1       403     403     0       4       80657
25%-tile:       1       409     409     0       4       806565
Median:         1       429     429     1       5       1613130
75%-tile:       1       429     429     4       6       2419694
97.5%-tile:     1       430     430     16      8       3145602
Maximum:        1       569     569     268     300     3226258
Mean:   1       422.28  422.28  2.95994 5.10718
number of Seqs:      **3226258**

- screen.seqs ------







