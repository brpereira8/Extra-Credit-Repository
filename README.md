# Extra-Credit-Repository
``mkdir /home/ec2-user/labs/lab-myusername/slc``

Make directory for our protein

``cd /home/ec2-user/labs/lab-myusername/slc``

Go to the directory 

``ncbi-acc-download -F fasta -m protein XP_021375138.1``

Download the gene

``blastp -db ../allprotein.fas -query XP_021375138.1.fa -outfmt 0 -max_hsps 1 -out slc.blastp.typical.out``

Regions that are alike and gets the statistical significance

``less slc.blastp.typical.out``

Looks at blast output

``less -s slc.blastp.detail.out``

Detailed look

``grep -c
awk ‘{if ($6< 1e-35 )print $1 }’ slc.blastp.detail.out > slc.blastp.detail.filtered.out``

High-score matches that are significant to the 1e-35
``grep -o -E "^[A-Z][a-z]+\." slc.blastp.detail.filtered.out  | sort | uniq -c``

Paralogs on each species

``conda install -y -n base -c conda-forge aha
sudo yum install -y a2ps
pip install buddysuite``

Alignbuddy and buddysuite download

``nano ~/labs/lab-$MYGIT/slc.fas``

Use nano to type sequences with fasta format

``seqkit grep --pattern-file ~/labs/lab-$MYGIT/slc/slc.blastp.detail.filtered.out ~/labs/lab4-$MYGIT/slc/slc.homologs.fas``

Sequences from blast file with it

``muscle -in ~/labs/lab-$MYGIT/slc/slc.homologs.fas -out ~/labs/lab4-$MYGIT/slc/slc.homologs.al.fas``

Use muscle to create alignment

``alv -kli --majority ~/labs/lab-$MYGIT/slc/slc.homologs.al.fas | less -RS``

View alignment

``iqtree -s ~/labs/lab5-$MYGIT/slc/slc.homologs.al.fas -bb 1000 -nt 2``

Gives Likely trees

``Rscript --vanilla ~/labs/lab-$MYGIT/plotUnrooted.R  ~/labs/lab-$MYGIT/slc/slc.homologs.al.fas.treefile ~/labs/lab-$MYGIT/slc/slc.homologs.al.fas.treefile.pdf 0.4``

Unrooted tree

``gotree reroot midpoint -i ~/labs/lab-$MYGIT/slc/slc.homologs.al.fas.treefile -o ~/labs/lab-$MYGIT/slc/slc.homologs.al.mid.treefile``

Midpoint rooted tree

``nw_order -c n ~/labs/lab-$MYGIT/slc/slc.homologs.al.mid.treefile  | nw_display -``

Shows trees

``nw_order -c n ~/labs/lab-$MYGIT/slc/slc.homologs.al.mid.treefile | nw_topology - | nw_display -s  -w 1000 > ~/labs/lab-$MYGIT/slc/slc.homologs.al.midCl.treefile.svg -``

Cladogram

``$MYGIT/slc/slc.homologs.al.mid.treefile ~/labs/lab-$MYGIT/slc/slc.homologs.al.mid.treefile
java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s ~/labs/lab-$MYGIT/species.tre -g ~/labs/lab-$MYGIT/slc/slc.homologs.al.mid.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/labs/lab-$MYGIT/slc/``

The tool to create the reconciliation of gene and species trees

``slc.homologs.al.mid.treefile.reconciled.events.txt
grep NOTUNG-SPECIES-TREE ~/labs/lab-$MYGIT/gqr/gqr.homologs.al.mid.treefile.reconciled | sed -e "s/^\[&&NOTUNG-SPECIES-TREE//" -e "s/\]/;/" | nw_display -``

See lineages assigned to nodes

``python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/labs/lab-$MYGIT/gqr/gqr.homologs.al.mid.treefile.reconciled --include.species``

Download python

``thirdkind -Iie -D 40 -f ~/labs/lab6-$MYGIT/gqr/gqr.homologs.al.mid.treefile.reconciled.xml -o  ~/labs/lab6-$MYGIT/gqr/gqr.homologs.al.mid.treefile.reconciled.svg``

View reconciliation with thirdkind

``sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab8-$MYGIT/plotTreeAndDomains.r ~/labs/lab5-$MYGIT/gqr/gqr.homologs.al.mid.treefile ~/labs/lab8-$MYGIT/slc/slc.rps-blast.out ~/labs/lab8-$MYGIT/slc/slc.homologs.fas   ~/labs/lab8-$MYGIT/slc/slc.tree.rps.pdf``

Copy of raw sequences without stop codon

``wget -O ~/data/Pfam_LE.tar.gz ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/little_endian/Pfam_LE.tar.gz && tar xfvz ~/data/Pfam_LE.tar.gz  -C ~/data``

Download pfam database

``rpsblast -query ~/labs/lab8-$MYGIT/slc/slc.homologs.fas -db ~/data/Pfam -out ~/labs/lab8-$MYGIT/slc/slc.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001``

Run rps blast, get motifs 

``sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab8-$MYGIT/plotTreeAndDomains.r ~/labs/lab5-$MYGIT/slc/slc.homologs.al.mid.treefile ~/labs/lab8-$MYGIT/slc/slc.rps-blast.out ~/labs/lab8-$MYGIT/slc/slc.tree.rps.pdf``

Use R to get the domains next to the species

``mlr --inidx --ifs "\t" --opprint  cat ~/labs/lab8-$MYGIT/slc/slc.rps-blast.out | tail -n +2 | less -S``

Look at file created with the R script on the previous commands

``wget -O ~/data/Pfam_LE.tar.gz ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/little_endian/Pfam_LE.tar.gz && tar xfvz ~/data/Pfam_LE.tar.gz  -C ~/data``

Downloads pfam database

``rpsblast -query ~/labs/lab8-$MYGIT/slc/slc.homologs.fas -db ~/data/Pfam -out ~/labs/lab8-$MYGIT/slc/slc.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001``

Runs rps blast

``sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab8-$MYGIT/plotTreeAndDomains.r ~/labs/lab5-$MYGIT/slc/slc.homologs.al.mid.treefile ~/labs/lab8-$MYGIT/slc/slc.rps-blast.out ~/labs/lab8-$MYGIT/slc/slc.tree.rps.pdf``

Runs script that Dr. Rest wrote 

``mlr --inidx --ifs "\t" --opprint  cat ~/labs/lab8-$MYGIT/slc/slc.rps-blast.out | tail -n +2 | less -S
Looks at domains 
cut -f 1 ~/labs/lab8-$MYGIT/gqr/gqr.rps-blast.out | sort | uniq -c``

Shows the Distribution of domains across proteins

``sort  -k5rg ~/labs/lab8-$MYGIT/gqr/gqr.rps-blast.out | less -S``
Sorts the data



