# This page is currently under development, but the idea is to add the steps taken to generate the various reference databases used by the Research Group for Genetics (RGG) at UiT as I progress.

# 💾🧬 CRABS_RefDBs 🧬💾 #

Welcome to *CRABS_RefDBs*! This repository serves to illustrate how I've created various reference databases for various metabarcoding markers used in eDNA research at *UiT - The Arctic University of Norway*. 
All databases are here built with the extremely versatile [CRABS](https://github.com/gjeunen/reference_database_creator) software (Jeunen et al. 2023), which interconnects different reference data repositories and harmonizes them with the taxonomic backbone of NCBI. 
**Note**: All CRABS credits should be directed [here](https://github.com/gjeunen/reference_database_creator), I'm just a fan of the toolkit they've developed.


Maybe you are interested in:

🐟 Aquatic organisms in marine or freshwater. 🍄 Fungi and their role in decomposition. 💩 Revealing animal diets through fecal samples.

🦟 Using invertebrate-derived DNA (iDNA) to monitor mammal presence. 🦠 Discovering protists in soil and water ecosystems. 🐾 Analyzing wildlife presence through fecal eDNA.

🦌 Studying diets and microbiomes of grazing animals. 🌱 Assessing plant biodiversity using eDNA. 🦗 Monitoring insect populations in grassland ecosystems.

🐎 Studying the impact of grazing on the vegetation. 🌍 Comparing soil biodiversity from around the globe. 🪱 Investigating soil-dwelling communities.

🦠 Analyzing microbial communities in specific environments. 🍄 Studying plant-soil interactions of root-associated fungi. 🌱 Interpreting plant-pollinator interactions from insect DNA deposits.

🌊 Broad tree-of-life approaches to understanding biodiversity.

Even if you're just trying to address the 🐘 elephant in the room 🐘, there's probably a right reference database for you! All metabarcoding projects have different scopes and may benefit from a customized reference database with a specific taxonomic or environmental focus, and focusing only on specific marker genes.

Many people rightly advocate not to blindly trust the data found in online reference data repositories (e.g. NCBI, BOLD). However, if the alternative is that each and every one of us will have to create references from organisms that we ourselves have identified from the respective environments that we work in, then the whole purpose behind the public repositories is lost. 
In this GitHub repo, focus is instead on collectively identifying and "blacklisting" those accession numbers that are clearly misidentified, and removing these entries as part of creating the reference database. As with fundamental legal principles, **innocent until proven guilty**, right? I suppose you could view this in legal terms as "all reference barcodes have a right to a fair trial".
I here provide a preliminary blacklist of all accession numbers that I do not trust the taxonomic identification of, but feel free to either add to this list locally, or to leave a comment on accession numbers that shouldn't be trusted. If you provide some contextual support for not trusting an accession number in your comment, I'd be happy to update the blacklist to include your suggested entries. 
Also note that each separate reference database created will work with the same

# Requirements/Prerequisites #
All of the databases can be built upon activating a conda environment based on the yml-file provided in this repo. Follow the below steps to create the environment.

```
conda env create -f crabs.yml
conda activate crabs
```

If you are working on SIGMA2/NRIS/EuroHPC infrastructure (e.g. Saga, Fram, Betzy, Olivia, LUMI), the direct use of conda installations is prohibited (or will be soon). An alternative solution is to install using [hpc-container-wrapper](https://github.com/CSCfi/hpc-container-wrapper), essentially wrapping your installation inside an Apptainer/Singularity container to reduce the number of files generated. 
But hey, if you have root access, you're the one commandeering the bioinformatic battleship anyway! 🚢🏴‍☠️⚓️

If you're discouraged to do a direct conda installation, follow the steps below instead (Olivia example with preinstalled hpc-container-wrapper accessed via the NRIS software stack "NRIS/CPU").
#### Note: replace <install_dir> with the path to where you want to make the installation.

```
module purge
source /opt/cray/pe/lmod/lmod/init/profile
export MODULEPATH=/cluster/software/modules/Core/
module load NRIS/CPU
module load hpc-container-wrapper
conda-containerize new --prefix <install_dir> crabs.yml
export PATH="<install_dir>/bin:$PATH"
```

You should now be able to call any executables installed in the environment as if the environment was activated. You can test whether this worked, e.g. by typing:

```
crabs -h
```

# Usage notes #

- At present, there are XX different reference databases supported here.
- Bash wrapper around the standard CRABS-commands
- These scripts essentially follow the crabs GitHub recommendations most of the way, but do not follow their recommendations completely.
- Downloading the bold database with matching dipteran families etc.
- Icky things in the code
-   Careful when adding to blacklist.txt, ensure it will work
-   Careful when using the taxonlist provided, it currently shrinks in size as your download progresses. The file depletes itself. (The file gets deconstructed as you succesfully download. This also updates the counts of taxa required for download and missing to be downloaded)
-   Careful with ...
-   Did not test what happens if you want to remove the last sequence of a fasta file (either bold or ncbi download) - code likely breaks
-   Bug fix to remove "ERROR | Error running makeblastdb BLAST Database creation error: Error: Duplicate seq_ids are found: GB|EU148067, aborting analysis..."
-   Ensure to update your email in the script (for NCBI)







# COI reference database for the "Leray" or "Leray-XT" marker

## Key features
**Target**: 313 bp of the mitochondrial Cytochrome c oxidase subunit I (COI)

**F-primer**: Either mlCOIintF 5′-GGWACWGGWTGAACWGTWTAYCCYCC-3′ (Leray et al., 2013) or mlCOIintF-XT 5′-GGWACWRGWTGRACWITITAYCCYCC-3′ (Wangensteen et al., 2018)

**R-primer**: TAIACYTCIGGRTGICCRAARAAYCA (Geller et al., 2013))

**Species-level resolution for the majority of metazoans based on current knowledge**

**Known issues with co-amplification of prokaryotes from environmental samples**

Can also serve as reference database for primers amplifying shorter fragments contained within the same 313 bp region ... **(find examples Mads!)**

##

This is the database for those of you who want **one (COI) database to rule them all** for all of your metabarcoding projects - especially those focusing on environmental DNA samples (in contrast to bulk samples where e.g. sorting of specimens would maybe rule out the need for certain taxa in your reference database).
On my setup, it takes ~12-14 hours from start of download to  ...
To create this monstrosity of a reference database, you obviously need patience. At the time of writing, it downloads >14 million sequences from [BOLD](https://boldsystems.org/) and >1.2 million sequences from [NCBIs nt database](https://www.ncbi.nlm.nih.gov/nuccore/?term=). 
Generating this database is unfortunately not as smooth-sailing as one could hope for, and this is largely due to heavy traffic on the BOLD servers.

Do note that creation of this reference database does not include any dereplication (as suggested by the CRABS software developers). I mainly use the [MetaBarFlow](https://github.com/evaegelyng/MetaBarFlow/tree/master) pipeline for analyzing metabarcoding data, and the downstream taxonomic identification script takes into account the amount of hits to each taxon for a given query sequence. 
This would clearly not be possible if the input file for creating the reference database has been dereplicated.

As a note to myself, I should probably look into including representative bacterial sequences in this reference database, as co-amplification of prokaryotes is likely to occur when amplifying this genetic region.

To build this database, make sure your environment is activated and run the following script.

```
bash Make_RefDB_LerayXT.sh
```

Once the database is built, you can perform blast searches against the database. An example blast search could look like below. **Note that 1)** The prefix of your database has to be specified (in this case "BLAST_TAX_COI", so not just the path to the directory in which the database is found). **2)** You can replace "OUTFILE.blasthits" and "INPUTFILE.fasta" with I/O names that match your files.
```
blastn -db "path/to/BLAST_TAX_COI" -max_target_seqs 500 -outfmt "6 std qlen qcovs staxid sscinames" -out OUTFILE.blasthits -qcov_hsp_perc 90 -perc_identity 80 -query INPUTFILE.fasta
```


![Figure_Example1](figures_readme/Fish_green_red_test.png)


# 🐟 12S reference database for the "MiFish" marker 🐟

## Key features
#### Targeting ~170 bp of the mitochondrially encoded 12S ribosomal RNA (12S rRNA) gene
#### F-primer: UiT version, original, Tele02? Others? Elasmo too?
#### R-primer: Original, Tele02? Others? Elasmo too?
##


To build this database, run the following script.

```
bash Make_RefDB_MiFish.sh
```


## 🧩 Contributors 🧩

* [Mads Reinholdt Jensen](https://github.com/MadsRJ): Main contributor

## 👏 Acknowledgements 👏

This repository was created under the framework of the EU-project [MARCO-BOLO](https://marcobolo-project.eu/).

## ✍️ References ✍️
Geller, J., Meyer, C., Parker, M., & Hawk, H. (2013). Redesign of PCR primers for mitochondrial cytochrome c oxidase subunit I for marine invertebrates and application in all-taxa biotic surveys. Molecular Ecology Resources 13(5), 851-861. https://doi.org/10.1111/1755-0998.12138.

Jeunen, G.-J., Dowle, E., Edgecombe, J., von Ammon, U., Gemmell, N. J., & Cross, H. (2023). crabs—A software program to generate curated reference databases for metabarcoding sequencing data. Molecular Ecology Resources 23(3), 725-738. https://doi.org/10.1111/1755-0998.13741.

Leray, M., Yang, J. Y., Meyer, C. P., Mills, S. C., Agudelo, N., Ranwez, V., Boehm, J. T., & Machida, R. J. (2013). A new versatile primer set targeting a short fragment of the mitochondrial COI region for metabarcoding metazoan diversity: application for characterizing coral reef fish gut contents. Frontiers in Zoology 10, 34. https://doi.org/10.1186/1742-9994-10-34.

Wangensteen, O. S., Palacín, C., Guardiola, M., & Turon, X. (2018). DNA metabarcoding of littoral hard-bottom communities: high diversity and database gaps revealed by two molecular markers. PeerJ 6, e4705. https://doi.org/10.7717/peerj.4705.

## ❓Questions❓

If you have questions or issues, email Mads Reinholdt Jensen (mads.jensen@uit.no) or leave a comment on this repository.
