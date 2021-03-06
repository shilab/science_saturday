Getting Your Feet Wet In Bioinformatics
==================

For this hands on demonstration we will use some Zebrafish data that the Shi lab group has been working with.

We will be using the command line to perform much of the analysis. If you are using a Mac, go to the finder and search for "Terminal". This is your command line, and you'll use it to type in all the commands below.
If you are using Windows go to [this link](http://gooseberrycreative.com/cmder/), and follow the download directions. This will give you a command line similar to the one on a Mac or Linux computer.

We will also need Python and R. If you are on a Mac, Python should already be installed.
If you are on Windows, you can go [here](http://continuum.io/downloads#all), and download an installation of Python. 
To download RStudio, which provides a nice interface for R, go [here](http://www.rstudio.com/), and follow the instructions. 

##1) Make a folder for the data

To move into your downloads folder type:  
```
cd Downloads
```  
Now we will make a new folder called data inside your Downloads folder. This is where we will put the files we will use.  
```
mkdir data
```

##2) Download the data

We will use curl to download the data from GitHub  
```
curl https://raw.githubusercontent.com/shilab/science_saturday/master/data/CNV_matrix > data/CNV_matrix
curl https://raw.githubusercontent.com/shilab/science_saturday/master/data/CNV_position > data/CNV_position
curl https://raw.githubusercontent.com/shilab/science_saturday/master/data/gene_position> data/gene_position
curl https://raw.githubusercontent.com/shilab/science_saturday/master/data/liver_expression > data/liver_expression
```

##3) Manipulating the data

We now have four files: a genotype file, a gene expression file, a gene position file and a CNV position file. Unfortunately for some samples we only have genotype information and not phenotype information, and for others we only have phenotype information.
We only want samples that have both genotype and phenotype data, and we also want to have the columns in the same order. We have written a Python script to do just that. If you go to [this page](https://github.com/shilab/sample_overlap) you can download the file. On the lower right hand side of the screen is a button that says "Download ZIP", click on it and wait for the download to finish. Next you need to unzip the folder, so simply double click on the downloaded file. 

Now we can use this python code to overlap the two files.

In your command line type  
```
python ~/Downloads/sample_overlap-master/overlap.py ~/Downloads/data/CNV_matrix ~/Downloads/data/liver_expression
```

It should run for a few seconds. When it is done you should see two new file in the data folder.

##4) Running the analysis  
We'll use Matrix eQTL to do the analysis. Matrix eQTL is an R package that can be used for eQTL studies. If you want you can read more about it [here](http://www.bios.unc.edu/research/genomic_software/Matrix_eQTL/).

Open up RStudio, and type  
```
install.packages('MatrixEQTL')
```

Next we are going to install a function that will make the eQTL analysis easier to run. Go to [this page](https://github.com/shilab/meQTL_functions) and download the folder using the "Download Zip" button. When it has downloaded, unzip the folder. 

To install it type:  
```
install.packages('~/Downloads/meQTL_functions/meQTLfunc_0.0.2.tar.gz', repos = NULL, type="source")
```

Next we need to load the packages we have installed. Simply type: 
```
library(MatrixEQTL)
library(meQTLfunc)
```

Now we can actually run the analysis.
Type  
```
me<-mxeqtl('~/Downloads/data/CNV_matrix.out','~/Downloads/data/CNV_position','~/Downloads/data/liver_expression.out', '~/Downloads/data/gene_position','~/Downloads/data/cisResults', 0.05)
```

You should see some text being displayed on the screen as the analysis progresses.

##5) Analyzing the results

Move into the data folder by typing
```
cd ~/Downloads/data
```

We can look at the results by typing
```
head cisResults
```
head lets us look at the first lines of a file. Our results has six columns, and each row represents a potential eQTL. 

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
