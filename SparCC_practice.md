#   SparCC 

#### SparCC [source depository](https://bitbucket.org/yonatanf/sparcc)is a python module for computing correlations
in compositional data (16S, metagenomics, etc').

# Installation to Beacon server
* Download the source code from [download site](https://bitbucket.org/yonatanf/sparcc/get/05f4d3f31d77.zip) to my directory
```
cd /lustre/medusa/fliu21/SparCC
wget https://bitbucket.org/yonatanf/sparcc/get/05f4d3f31d77.zip
```
Then **unzip** the directory and **change file name** to SparCC\_bitbucket

```
unzip 05f4d3f31d77.zip
mv yonatanf-sparcc-05f4d3f31d77/ SparCC_bitbucket
rm 05f4d3f31d77.zip
```
* I have installed Panda package in my ``anaconda2/bin`` directory, so I will set up the PATH to this 
```
export PATH=/lustre/medusa/fliu21/anaconda2/bin:$PATH
```
* As python has already installed on beacon server, I just load the environment by
```
module load python/3.6.1
```
* Now type in ``python SparCC.py -h`` inside of ``SparCC_bitbucket`` directory, you will see below manual

```
Example: python SparCC.py example/fake_data.txt -i 20 --cor_file=example/basis_corr/cor_mat_sparcc.out

Options:
  -h, --help            show this help message and exit
  -c COR_FILE, --cor_file=COR_FILE
                        File to which correlation matrix will be written.
  -v COV_FILE, --cov_file=COV_FILE
                        File to which covariance matrix will be written.
  -a ALGO, --algo=ALGO  Name of algorithm used to compute correlations (SparCC
                        (default) | pearson | spearman | kendall)
  -i ITER, --iter=ITER  Number of inference iterations to average over (20
                        default).
  -x XITER, --xiter=XITER
                        Number of exclusion iterations to remove strongly
                        correlated pairs (10 default).
  -t TH, --thershold=TH
                        Correlation strength exclusion threshold (0.1
                        default).

```
Look into the example file, the input out table is formated with **rows being OTUs** from 1 to 50; **columns are sample** names from 1 to 200 samples