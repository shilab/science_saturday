Getting Your Feet Wet In Bioinformatics
==================

We'll use some Zebrafish data that we've been working with.

1) Download the data

We'll download the data from a remote server. You'll need to use the command line/terminal to do this.

Type
```
scp root@45.55.133.218:/root/data/ ~/Downloads
```

It should ask you for a password, which will be written on the board.

2) Manipulating the data

Some of the data in one file isn't in the other file, so we need to remove it, and get the columns in the same order. Go to [this page](https://github.com/shilab/sample_ovelap) and download the file.

We can use this python code to overlap the two files.

Type
```
python ~/Downloads/sample_overlap-master/overlap.py ~/Downloads/data/CNV_matrix ~/Downloads/data/liver_expression
```

It should run for a few seconds. When it is done you should see two new file in the data folder.

3) Running the analysis
We'll use Matrix eQTL to do the analysis.

Open up RStudio, and type
```
install.packages('MatrixEQTL')
```

Go to [this page](https://github.com/shilab/meQTL_functions) and download the folder.

Use the RStudio installer to install the meQTL functions packages.

Type
```
library(MatrixEQTL)
library(meQTLfunc)
```

Now we can actually run the analysis.
Type
```
me<-mxeqtl('~/Downloads/data/CNV_matrix.out','~/Downloads/data/CVN_position','~/Downloads/data/liver_expression.out', '~/Downloads/data/gene_position','~/Downloads/data/cisResults','0.05')
```
4) Analyzing the results

Move into the data folder by typing
```
cd ~/Downloads/data
```

We can look at the results by typing
```
head cisResults
```

Now lets take a deep look at the data
```
cut -f 1 cisResults
```

```
cut -f 1 cisResults | sort | uniq -c
```

```
cut -f 1 cisResults | sort | uniq -c | sed 's/^ *//g' | sort -n
```

```
cut -f 1 cisResults | sort | uniq -c | sed 's/^ *//g' | sort -n > unique_cnvs
```

```
cut -f 2 cisResults | sort | uniq -c | sed 's/^ *//g' | sort -n > unique_genes
```

Now we can use R to make plots
