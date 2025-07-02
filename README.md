# Methanogen-Comparison-Phylogenetic-Tree
Code on how to make a neighbor-joining tree with GToTree using bit to automatically download genomes from GTDB.

This pipeline uses conda installs for all of the bioinformatic programs. For information on how to set up conda on your local computer see here: https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-python

## Step 1. Grab the NSHQ4/Methanobacterium/Lost City MAGS/Methanocalculus natronophilus genomes off of the google drive (They are uploaded to this repo for easy access under "methanogen_MAGs.zip")
```
NSHQ14B.bins.3.fasta
NSHQ14B.bins.4.fasta
bin.009_Methanobacterium.fasta_assembly.fa
bin.019_Methanobacterium.fasta_assembly.fa
Methanosarcinaceae_JAAXQB01.fasta_assembly.fa
Mnat_SPAdes.Assembly.fa
```
## Step 2. Grab additional genomes in bulk off of GTDB using bit
### bit documentation: https://github.com/AstrobioMike/bit
```
# Access genomes from gtdb 
## conda/mamba install and environment activation
mamba create -y -n bit -c astrobiomike -c conda-forge -c bioconda -c defaults bit

## Activate conda environment
conda activate bit

## searching GTDB for a specific taxon and getting the corresponding NCBI accessions
#1: Genus Methanobacterium
bit-get-accessions-from-GTDB -t "Methanobacterium”
#2: Genus Methanocalculus
bit-get-accessions-from-GTDB -t "Methanocalculus"
#3: Genus Methanothrix
bit-get-accessions-from-GTDB -t "Methanothrix”
#4: Genus Methanosarcina
bit-get-accessions-from-GTDB -t "Methanosarcina”
#4: Genus Methanosalsum
bit-get-accessions-from-GTDB -t “Methanosalsum”

## Download those genomes from gtdb 
#1: Genus Methanobacterium (56 genomes)
bit-dl-ncbi-assemblies -w GTDB-Methanobacterium-genus-accs.txt -f fasta -j 4
#2: Genus Methanocalculus (17 genomes)
bit-dl-ncbi-assemblies -w GTDB-Methanocalculus-genus-accs.txt -f fasta -j 4
#3: Genus Methanothrix (81 genomes)
bit-dl-ncbi-assemblies -w GTDB-Methanothrix-genus-accs.txt -f fasta -j 4
#4: Genus Methanosarcina (177 genomes)
bit-dl-ncbi-assemblies -w GTDB-Methanosarcina-genus-accs.txt -f fasta -j 4
#4: Genus Methanosalsum (5 genomes)
bit-dl-ncbi-assemblies -w GTDB-Methanosalsum-genus-accs.txt -f fasta -j 4
```

## Step 3. Alignment using GToTree

### reference database download here: https://zenodo.org/records/5571251
### GToTree documentation https://github.com/AstrobioMike/GToTree
### Dr. Emily Skoog's github with an expanded tutorial: https://github.com/emilieskoog/Desulfurobacteriaceae_phylogenomic_tree?tab=readme-ov-file
### Interactive Tree of Life (where you will upload your .tre file): https://itol.embl.de/
#### Note: This is a phylogenomic tree tutorial (aligning whole genomes based on marker genes), NOT aligning single proteins of interest.
#### Another quick note before starting, this program is 'safe' to run locally (on your computer). It is not that large of a program and aligning and building a phylogenomic tree for 10 genomes only takes a few minutes. It will run reliably locally for 50 genomes. I ran mine on the supercomputing cluster, which is why I have more than 50 genomes included.
```
# 1. Create GToTree Environment

conda create -y -n gtotree -c conda-forge -c bioconda -c astrobiomike gtotree

# 2. Activate GToTree Environment

conda activate gtotree

# 3. Create text file with all fasta files listed
## You can do this by placing all genome files in one folder. Enter into that folder ('cd' into it from terminal).
## Then execute:

ls *.fa > fasta_files.txt

## This will take all files with the extension .fa and put their names into a text file called fasta_files.txt.
## If your files have a different extension, make sure to reflect that with the *.fa (can change to *.fasta, etc.)

# 4. Run GToTree

## Again, make sure you have activated your environment. Then execute:

GToTree -a ncbi_accs_methanosarcinaeceae_methanobaceriacaea.txt \
	   -f fasta_files.txt \
	   -H Archaea \
        -t -o By_Family \
	   -D \
	   -j 4

## An example: 
GToTree -f fasta_files.txt -H Gammaproteobacteria \
        -t -o Marinobacter_selected

## (Read the GToTree user manual for details on flags, etc, and cater to your data.)

# 5. Visualize tree: PART 1

## GToTree will output a file with the extension .tre. In this case, because I declared that my output would be named Desulfurobacteria
## (as per my -o Desulfurobacteria flag), the outputted file of interest is called Desulfurobacteria.tre. You can take this bad boy directly to
## ITOL (Interactive Tree of Life) and it's upload page and drag and drop that file to view your tree.
```

