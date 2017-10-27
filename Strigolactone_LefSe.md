#                               2016 strigolactone LefSe analysis documentation
-----------------

## Data manipulation within mothur -
The reason that I manipulate shared file inside of mothur instead of within R is that make.lefse command need .shared file as input.

### Family level shared file -- taxlevel=2

1. Rarefy\_based normalization - Subset ``.shared`` using rarefaction method to minimum read depth along all samples 

``sub_sample``

```

```


-------------------------

### Genus level shared file -- taxlevel=1

Forder path

```
/nics/d/home/fliu21/16S_strigolactone_proj/LefSe/LefSe_st_genus_update
```

1. Rarefy\_based normalization - Subset ``.shared`` using rarefaction method to minimum read depth (26050) along all samples using ``sub_sample``

```
sub.sample(shared=strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.shared_original,size=26050,groups=CT_1-CT_2-CT_3-CT_4-CT_5-CT_6-CT_7-Gene10_1-Gene10_2-Gene10_3-Gene10_4-Gene10_5-Gene10_6-Gene10_7-Gene1_1-Gene1_2-Gene1_3-Gene14_1-Gene14_2-Gene14_3-Gene14_4-Gene14_5-Gene14_6-Gene14_7-Gene1_4-Gene1_5-Gene1_6-Gene1_7-HYPIII_B_1-HYPIII_B_2-HYPIII_B_3-HYPIII_B_4-HYPIII_B_5-HYPIII_B_6-HYPIII_B_7-HYPIII_B_8)
```
2. Remove genus with total abundance less than 50. Using ``remove.rare`` function in mothur


```
remove.rare(shared=strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.shared_original,nseqs=50)
```
3. Creat lefse object using ``make.lefse``

```
make.lefse(shared=strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.1.pick.shared_original,constaxonomy=strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.cons.taxonomy_original,design=design.txt,scale=totalgroup)
```
4. Now, change to beacon from mothur. Vim generated *.lefse* file, there are lots of double quotes inside. Using vim replacement to replace all doucle quote with empty
```
$%s/"//g
```
5. Add path to the environment ``export PATH=/lustre/medusa/fliu21/anaconda2/bin:$PATH``

6. Run ``lefse-format_lefse.py`` to change format for running real lefse analysis

```
lefse-format_input.py strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.1.pick.1.lefse strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.1.pick.1.lefse.in -c 1 -s -1 -u 2
```
7. Doing lefse analysis using ``run_lefse.py``
  * ``-y`` parameter is used to set whether the test is performed in one against one or in one again all setting. default is one against all y = 0
  
```
run_lefse.py strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.1.pick.1.lefse.in strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.1.pick.1.lefse.res -l 2 -b 100 -t strigolactone_genus_level_lefse -y 0
```

Here are the output:

```
Number of significantly discriminative features: 720 ( 757 ) before internal wilcoxon
Number of discriminative features with abs LDA score > 2.0 : 0
```

 *  y = 1

```
run_lefse.py strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.1.pick.1.lefse.in strigolactone.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.tx.1.1.subsample.1.pick.1.lefse.res -l 2 -b 100 -t strigolactone_genus_level_lefse -y 1
```
Here are the output:

```

```













