# Phylogenetic tree construction based on *FOXP2* gene
In this project a phylogenetic tree was created from a multiple sequence alignment of peptide sequences for the forkhead box P2 transcription factor from 14 bird species.  All animals have orthologous sequences for this gene.  The goal of this project was to create a phylogenetic tree based upon the *FOXP2* gene using selected members of the Aves class. This was accomplished by obtaining peptide sequences for the forkheadbox protein, performing a multiple sequence alignment with them, and using the MSA to create the tree. 

To start, a directory was made on Ron for the project.
> mkdir Final_Project

A Git repository was initiated in /home/gen7112020/pm1090g71120/Final_Project/.git/
> git init

To accomplish the goal of the project, 14 protein sequence sets were acquired from Ensembl, one of which was the protein set for Struthio camelus australis (the American ostrich) to serve as the outgroup in the tree.  

**Data:**

•	African ostrich (Struthio camelus australis)

ftp://ftp.ensembl.org/pub/release-100/fasta/struthio_camelus_australis/pep/Struthio_camelus_australis.ASM69896v1.pep.all.fa.gz

•	Zebra finch (Taeniopygia guttata)

ftp://ftp.ensembl.org/pub/release-100/fasta/taeniopygia_guttata/pep/Taeniopygia_guttata.bTaeGut1_v1.p.pep.all.fa.gz

•	White-throated sparrow (Zonotrichia albicollis)

ftp://ftp.ensembl.org/pub/release100/fasta/zonotrichia_albicollis/pep/Zonotrichia_albicollis.Zonotrichia_albicollis-1.0.1.pep.all.fa.gz

•	Bengalese finch (Lonchura striata domestica)

ftp://ftp.ensembl.org/pub/release-100/fasta/lonchura_striata_domestica/pep/Lonchura_striata_domestica.LonStrDom1.pep.all.fa.gz

•	Blue tit (Cyanistes caeruleus)

ftp://ftp.ensembl.org/pub/release-100/fasta/cyanistes_caeruleus/pep/Cyanistes_caeruleus.cyaCae2.pep.all.fa.gz

•	Common canary (Serinus canaria)

ftp://ftp.ensembl.org/pub/release-100/fasta/serinus_canaria/pep/Serinus_canaria.SCA1.pep.all.fa.gz

•	Dark-eyed junco (Junco hyemalis)

ftp://ftp.ensembl.org/pub/release-100/fasta/junco_hyemalis/pep/Junco_hyemalis.ASM382977v1.pep.all.fa.gz

•	Budgerigar (Melopsittacus undulatus)

ftp://ftp.ensembl.org/pub/release-100/fasta/melopsittacus_undulatus/pep/Melopsittacus_undulatus.Melopsittacus_undulatus_6.3.pep.all.fa.gz

•	Yellow-billed parrot (Amazona collaria)

ftp://ftp.ensembl.org/pub/release-100/fasta/amazona_collaria/pep/Amazona_collaria.ASM394721v1.pep.all.fa.gz

•	Japanese quail (Coturnix japonica)

ftp://ftp.ensembl.org/pub/release-100/fasta/coturnix_japonica/pep/Coturnix_japonica.Coturnix_japonica_2.0.pep.all.fa.gz

•	Burrowing owl (Athene cunicularia)

ftp://ftp.ensembl.org/pub/release-100/fasta/athene_cunicularia/pep/Athene_cunicularia.athCun1.pep.all.fa.gz

•	Chicken (Gallus gallus)

ftp://ftp.ensembl.org/pub/release-100/fasta/gallus_gallus/pep/Gallus_gallus.GRCg6a.pep.all.fa.gz

•	Golden eagle (Aquila chrysaetos chrysaetos)

ftp://ftp.ensembl.org/pub/release-100/fasta/aquila_chrysaetos_chrysaetos/pep/Aquila_chrysaetos_chrysaetos.bAquChr1.2.pep.all.fa.gz

•	Mallard (Anas platyrhynchos)

ftp://ftp.ensembl.org/pub/release-100/fasta/anas_platyrhynchos/pep/Anas_platyrhynchos.ASM874695v1.pep.all.fa.gz

---
The data was downloaded within the Final_Project directory from Ensembl using the “wget” command, repeated separately for each organism.  
> wget ftp://ftp.ensembl.org/pub/release-100/fasta/struthio_camelus_australis/pep/Struthio_camelus_australis.ASM69896v1.pep.all.fa.gz

The sequence sets downloaded from Ensembl were then each individually unzipped using the “gzip -d” command. 
> gzip -d Struthio_camelus_australis.ASM69896v1.pep.all.fa.gz


Each of the unzipped sequence sets were unwrapped using the awk command because the sequences were not formatted to be all on a single line.  Additionally, the output files were each given a more simplified name.
> awk '{if(NR==1) {print $0} else {if($0 ~ /^>/) {print "\n"$0} else {printf $0}}}' Struthio_camelus_australis.ASM69896v1.pep.all.fa > ostrich_unwrap.fa  


Once all of the protein sequence sets from Ensembl were unzipped and unwrapped, the peptide sequence(s) of each birds FoxP2 protein, and their headers, were pulled from their unwrapped file and moved to a new file designated for the sequences of that bird.  This was accomplished by searching each file using the grep command and the gene identification sequence for the FOXP2 gene in that specific bird (within the quotations below), beginning with the American ostrich.  
> grep -A 1 "ENSSCUG00000004384" ostrich_unwrap.fa > american_ostrich_foxp2.fa


Once the FoxP2 peptide sequences and their headers were pulled out of each of the unwrapped protein set files, and put into their own subsequent new files, the awk command was used again on each new file in order to format the headers to be shorter and more informative for the phylogenetic tree.  
> awk '/^>/{print ">American_ostrich_" ++i; next}{print}' american_ostrich_foxp2.fa > header_american_ostrich_foxp2.fa

---
**MSA and Phylogenetic tree creation**

All of the files containing the sequences with formatted headers were concatenated into a single fasta file using the “cat” command. 
> cat header_burrowing_owl_foxp2.fa header_budgeriger_foxp2.fa header_chicken_foxp2.fa header_darkeyed_junco_foxp2.fa header_golden_eagle_foxp2.fa header_japanese_quail_foxp2.fa header_mallard_foxp2.fa header_common_canary_foxp2.fa header_blue_tit_foxp2.fa header_whitethroated_sparrow_foxp2.fa header_yellowbilled_parrot_foxp2.fa header_bengalese_finch_foxp2.fa header_zebra_finch_foxp2.fa header_american_ostrich_foxp2.fa > foxp2_peptideseqs.fa 

The new single file containing all of the FoxP2 protein peptide sequences for all of the birds (the output file from the last code) was then used as the input file to create a multiple sequence alignment (MSA), using the mafft command.  The type of alignment in the code was set to "auto" so that the program would choose the optimal alignment strategy, which was local pairwise alignment.  
> mafft --auto foxp2_peptideseqs.fa > mafft_foxp2_output.fa 

Iqtree was used to create the phylogenetic tree with the MSA file as the input file. 
> iqtree -s mafft_foxp2_output.fa -m LG -bb 1000 -pre FoxP2 

The phylogenetic tree, and all of the other iqtree output files, can be found in this repository containing the prefix "FoxP2".  
