### BOT563: Phylogenetic Analysis of Molecular Data
### Joseph Walston Repository

Data will come from GenBank from the paper Liu and Smith from 2021.

For homework due 2-20-2024, I am still unsure what alignment method to use for my data. I believe that ClustalW would be best, but my laptop's operating system is too new to work with it. I have downloaded 25 nucleotides from GenBank all from the paper Liu and Smith. I do not plan on using T-Coffee because of Dr. Claudia's suggestion (since it would likely be the slowest or most complex). M-Coffee intrigued me but that seems too complex for right now. I plan on going to office hours next week when Dr. Claudia is back because I am confused on how I would know how to write a reproducible script based on an alignment method from the Alignathon paper.

<<<<<<< HEAD

Code used to make distance and parsimony trees using my data:
Below are the codes for both parsimony and distance methods. Neither are the most common used today (lieklihood methods). In these methods, you cannot infer timing as a measure of branch length, without further analysis and data. Parsimony is particularly complex and takes a lot more time than other methods.

install.packages("adegenet", dep=TRUE)
install.packages("phangorn", dep=TRUE)

install.packages("igraph", type="binary")

library(ape)
library(adegenet)
library(phangorn)


dna <- fasta2DNAbin(file="/Users/jwalston/Desktop/myproject/HeliamphoraNucleotides2.19.24")

D <- dist.dna(dna, model="TN93")
tre <- nj(D)
tre <- ladderize(tre)
plot(tre, cex=.6)
title("A simple NJ tree")

#second activity
dna <- fasta2DNAbin(file="/Users/jwalston/Desktop/myproject/HeliamphoraNucleotides2.19.24")
dna2 <- as.phyDat(dna)

tre.ini <- nj(dist.dna(dna,model="raw"))
parsimony(tre.ini, dna2)

tre.pars <- optim.parsimony(tre.ini, dna2)

plot(tre.pars, cex=0.6)
=======
(clustalw_env) Josephs-MacBook-Pro-2:data jwalston$ clustalw2 -ALIGN -INFILE=primatesAA.fasta -OUTFILE=primatesAA-aligned.fasta -OUTPUT=FASTA
>>>>>>> d96ba2c758810ec99983f5b7b96332fafad01807


(clustalw_env) Josephs-MacBook-Pro-2:Desktop jwalston$ clustalw2 -ALIGN -INFILE=data/HeliamphoraNucleotides2.19.24 -OUTFILE=HeliamphoraNucleotidesAligned.fasta -OUTPUT=FASTA

./raxml-ng --check --msa Heliamphoranucelotides2.19.24.fa --model GTR+G
./raxml-ng --check --msa Heliamphoranucelotides2.19.24.fa/bad.fa.raxml.reduced.phy --model GTR+G

Below is the line of code used to check that my aligned nucleotides are all good to go
../raxml-ng_v1.2.1_macos_x86_64/raxml-ng --check --msa HeliamphoraNucleotidesAligned.fasta --model GTR+G

This is the line of code used to make the raxml tree
(clustalw_env) Josephs-MacBook-Pro-2:myProject jwalston$ ../raxml-ng_v1.2.1_macos_x86_64/raxml-ng --msa HeliamphoraNucleotidesAligned.fasta --model GTR+G --prefix T3 --threads 2 --seed 2

Here is the line of code used to make the IQTree:
iqtree-1.6.12-MacOSX/bin/iqtree -s HeliamphoraNucleotidesAligned.fasta

Here is the R-code that was used to plot the Raxml and IQTREE
phy <- read.tree(file="/Users/jwalston/Desktop/MyProject/T3.raxml.bestTree")
plot(phy, cex=0.6)

phy = root(phy, outgroup="MN428598.1")
plot(phy, cex=0.6)

#iqtree
phy2 <- read.tree(file="/Users/jwalston/Desktop/MyProject/HeliamphoraNucleotidesAligned.fasta.treefile")
plot(phy2, cex=0.6)

phy3 = root(phy2, outgroup="MN428598.1")
plot(phy3, cex=0.6)


Mr. Bayes code (it worked):

First, take your aligned data (HeliamphoraNucleotidesAligned.fasta) and convert it a nexus file. I used http://phylogeny.lirmm.fr/phylo_cgi/data_converter.cgi. It will spit out text that you put in a text file and label it with .nex at the end. This is the file that you will use in MrBayes.

which mb
cd Desktop
cd MyProject

and then i pasted this at the bottom of the nexus document:
begin mrbayes;
 set autoclose=yes;
 prset brlenspr=unconstrained:exp(10.0);
 prset shapepr=exp(1.0);
 prset tratiopr=beta(1.0,1.0);
 prset statefreqpr=dirichlet(1.0,1.0,1.0,1.0);
 lset nst=2 rates=gamma ngammacat=4;
 mcmcp ngen=10000 samplefreq=10 printfreq=100 nruns=1 nchains=3 savebrlens=yes;
 outgroup MN428598.1;
 mcmc;
 sumt;
end;

note how I changed the outgroup (line 87) to the outgroup of my data
code continued:
mb HeliamphoraNucleotidesAligned.nex


Redoing all the steps (align .... ) for 26S Gene for final project
Code used to align:

jwalston$ conda activate /Users/jwalston/miniconda3/envs/clustalw_env
(clustalw_env) Josephs-MacBook-Pro-2:myProject jwalston$ clustalw2 -ALIGN -INFILE=Heliamphora26SNucleotides -OUTFILE=Heliamphora26SNucleotidesAligned.fasta -OUTPUT=FASTA

Next step is to use RaxML. First I had to deactivate conda by doing "conda deactivate"
Then, I found my folder with RaxML downloaded and moved my aligned fasta file into that folder.
Then, I ran this code: (base) Josephs-MacBook-Pro-2:raxml-ng_v1.2.1_macos_x86_64 jwalston$ ./raxml-ng --check --msa Heliamphora26SNucleotidesAligned.fasta --model GTR+G
I reactivated Conda and then ran this code to make the RaxML tree: (clustalw_env) Josephs-MacBook-Pro-2:raxml-ng_v1.2.1_macos_x86_64 jwalston$  ../raxml-ng_v1.2.1_macos_x86_64/raxml-ng --msa Heliamphora26SNucleotidesAligned.fasta --model GTR+G --prefix T4 --threads 2 --seed 2
Next, I ran IQTree. I amde sure to put my aligned sequences file into the IQTree folder and its subfolder called bin. Here is the code for making IQTree: (base) Josephs-MacBook-Pro-2:bin jwalston$ ./iqtree -s Heliamphora26SNucleotidesAligned.fasta


Now that I have my RaxML and IQTrees made, I go to R to create the tree figures.
###This was the code used to make the RaxML tree:

#raxml26s

phy <- read.tree(file="/Users/jwalston/Desktop/MyProject/T4.raxml.bestTree")
plot(phy, cex=0.6)
phy = root(phy, outgroup="MN428574.1")
plot(phy, cex=0.6)

newtips2 <- c("Darlingtonia californica", "Heliamphora huberi", "Heliamphora chimantensis", "Heliamphora ciliata", "Heliamphora pulchella", "Heliamphora minor var. pilosa", "Heliamphora tatei", "Heliamphora ceracea", "Heliamphora hispida", "Heliamphora neblinae", "Heliamphora parva", "Heliamphora nutans", "Heliamphora elongata", "Heliamphora arenicola", "Heliamphora ionasi", "Heliamphora sarracenioides", "Heliamphora exappendiculata", "Heliamphora sp. Akopan Tepui", "Heliamphora uncinata", "Heliamphora glabra", "Heliamphora sp. Angasima Tepui", "Heliamphora purpurascens", "Heliamphora collina", "Heliamphora folliculata", "Heliamphora heterodoxa")
phy$tip.label <- newtips2
plot(phy)

###Here is the code used to make the IQtree.

#iqtree26s

phy2 <- read.tree(file="/Users/jwalston/Desktop/MyProject/Heliamphora26SNucleotidesAligned.fasta.treefile")
plot(phy2, cex=0.6)

phy3 = root(phy2, outgroup="MN428574.1")
plot(phy3, cex=0.6)
newtips<- c("Heliamphora sp. Akopan Tepui", "Heliamphora exappendiculata", "Heliamphora glabra", "Heliamphora folliculata", "Heliamphora collina", "Heliamphora sarracenioides", "Heliamphora heterodoxa", "Heliamphora arenicola", "Heliamphora ionasi", "Heliamphora elongata", "Heliamphora nutans", "Heliamphora neblinae", "Heliamphora parva", "Heliamphora hispida", "Heliamphora ceracea", "Heliamphora tatei", "Heliamphora chimantensis", "Heliamphora ciliata", "Heliamphora huberi", "Heliamphora pulchella", "Heliamphora minor var. pilosa", "Darlingtonia californica", "Heliamphora sp. Angasima Tepui", "Heliamphora purpurascens", "Heliamphora uncinata")
phy3$tip.label <- newtips
plot(phy3)

The line of code saying "root" roots the tree with its outgroup. The "newtips" portion changes the tip labels to their actual scientific names.
