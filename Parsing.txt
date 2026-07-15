# reference https://github.com/markandtwin/Pull-a-long for assistance
#This code maps long-reads with minimap2 then parses reads based on genome locations of mec-2 on the C. elegans X chromosome
#some of these files created will not be fully parsed and will be input into the diff function

module load gcc
export PATH=$HOME/anaconda3/bin:$PATH

conda create -n PL-seq_env -c defaults -c bioconda -c r -c conda-forge python=3.6 pysam samtools minimap2

conda activate PL-seq_env
module load samtools

paftools.js gff2bed /work/users/ccalovichbenne/GenomeFiles/Celegans.WBcel235.77_transcript.gtf > Celegans.WBcel235.77_transcript.gtf.bed

#Mapping with PL-seq paramaters
minimap2 -ax splice:hq -t 7 -B 3 -O 3,20 --junc-bed /work/users/ccalovichbenne/plseq/Celegans.WBcel235.77_transcript.gtf.bed /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa /work/users/ccalovichbenne/ISOSEQRawFiles/121422ANIsoSeq_SMU_ISOSEQ_CCB_n2_1_4_6-473-m64277e_221226_194658.hifi_reads.fastq > paramsmappedisoseq.sam

samtools sort mappedisoseq.sam > mappedisoseq.bam
samtools index output.bam

# Parse reads for promoter H (downstream of stomatin domain)
python exon_covering.py -i mappedisoseq.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterH.bed4 -o promoterH.bam -t 7

# Parse stomatin containing reads
python exon_covering.py -i mappedisoseq.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/stomatin.bed4 -o stomatinparsed.bam -t 7

# Parse promoter-specific (1st exon) reads from stomatin file just generated
python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterG.bed4 -o promoterG.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterF.bed4 -o promoterF.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterE.bed4 -o promoterE.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterD.bed4 -o promoterD.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterC.bed4 -o promoterC.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterB.bed4 -o promoterB.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterA.bed4 -o promoterA.bam -t 7

# Parse reads that splice to downstream exon 9A or exon 9B from promoter specific files (parse 5' -> 3')
python exon_covering.py -i promoterG.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/SpliceAshort5ss.bed4 -o pG_short9A.bam -t 7

python exon_covering.py -i promoterF.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/SpliceAshort5ss.bed4 -o pF_short9A.bam -t 7

python exon_covering.py -i promoterE.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/SpliceAshort5ss.bed4 -o pE_short9A.bam -t 7

python exon_covering.py -i promoterD.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/SpliceAshort5ss.bed4 -o pD_shortA.bam -t 7

python exon_covering.py -i promoterC.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/SpliceAshort5ss.bed4 -o pC_short9A.bam -t 7

python exon_covering.py -i promoterB.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/SpliceAshort5ss.bed4 -o pB_short9A.bam -t 7

python exon_covering.py -i promoterA.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/SpliceAshort5ss.bed4 -o pA_short9A.bam -t 7

python exon_covering.py -i promoterG.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Bexon.bed4 -o pG_9B.bam -t 7

python exon_covering.py -i promoterF.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Bexon.bed4 -o pF_9B.bam -t 7

python exon_covering.py -i promoterE.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Bexon.bed4 -o pE_9B.bam -t 7

python exon_covering.py -i promoterD.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Bexon.bed4 -o pD_9B.bam -t 7

python exon_covering.py -i promoterC.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Bexon.bed4 -o pC_9B.bam -t 7

python exon_covering.py -i promoterB.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Bexon.bed4 -o pB_9B.bam -t 7

python exon_covering.py -i promoterA.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Bexon.bed4 -o pA_9B.bam -t 7

# Parse alternative 5’splice site long (then subtract later from short) to find count of short since parsing is inclusive. Gives 9A from all transcripts not promoter specific.
python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/inc9Along5ss.bed4 -o 9Along5ss.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/inc9Ashort5ss.bed4 -o 9Ashort5ss.bam -t 7

# Parse promoters from exon 9A/B usage (parse 3' -> 5', double check no weird or funky mishaps in previous parsing)
python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/Splice9Ashort5ss.bed4 -o 9Asplicechoice.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/9Bexon.bed4 -o 9Bsplicechoice.bam -t 7

python exon_covering.py -i 9Asplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter1.bed4 -o 9A_pG.bam -t 7

python exon_covering.py -i 9Asplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter2.bed4 -o 9A_pF.bam -t 7

python exon_covering.py -i 9Asplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter3.bed4 -o 9A_pE.bam -t 7

python exon_covering.py -i 9Asplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter4.bed4 -o 9A_pD.bam -t 7

python exon_covering.py -i 9Asplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter5.bed4 -o 9A_pC.bam -t 7

python exon_covering.py -i 9Asplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter6.bed4 -o 9A_pB.bam -t 7

python exon_covering.py -i 9Asplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter7.bed4 -o 9A_pA.bam -t 7

python exon_covering.py -i 9Bsplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter1.bed4 -o 9B_pG.bam -t 7

python exon_covering.py -i 9Bsplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter2.bed4 -o 9B_pF.bam -t 7

python exon_covering.py -i 9Bsplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter3.bed4 -o 9B_pE.bam -t 7

python exon_covering.py -i 9Bsplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter4.bed4 -o 9B_pD.bam -t 7

python exon_covering.py -i 9Bsplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter5.bed4 -o 9B_pC.bam -t 7

python exon_covering.py -i 9Bsplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter6.bed4 -o 9B_pB.bam -t 7

python exon_covering.py -i 9Bsplicechoice.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoter7.bed4 -o 9B_pA.bam -t 7

# Parse for the cassette exon 12 from promoter E or H transcripts
python exon_covering.py -i pE_9A.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/shortcassette.bed4 -o pE_9Acassette.bam -t 7

python exon_covering.py -i pH.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/shortcassette.bed4 -o pHcassette.bam -t 7

# Parsing truncated and full length exon12 (cassette) from promoter E (stomatin parsed file but only pE has cassette)
python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/longcassette.bed4 -o longcassette.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/shortcassette.bed4 -o shortcassette.bam -t 7

python exon_covering.py -i stomatinparsed.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/lastexon.bed4 -o lastexon.bam -t 7

# parsing pH cassettes
python exon_covering.py -i pH.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/longcassette.bed4 -o pHlongcassette.bam -t 7

python exon_covering.py -i pH.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/shortcassette.bed4 -o pHshortcassette.bam -t 7

python exon_covering.py -i pH.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/lastexon.bed4 -o pHlastexon.bam -t 7

# parsing exon 12 cassette from pE_9A
python exon_covering.py -i onlyshort9A5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterE.bed4 -o pEs5ss.bam -t 7

python exon_covering.py -i long9A5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/promoterE.bed4 -o pEL5ss.bam -t 7

python exon_covering.py -i pEs5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/longcassette.bed4 -o pEs5sslongcassette.bam -t 7

python exon_covering.py -i p3s5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/shortcassette.bed4 -o pEs5ssshortcassette.bam -t 7

python exon_covering.py -i p3s5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/lastexon.bed4 -o pEs5sslastexon.bam -t 7

python exon_covering.py -i p3l5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/longcassette.bed4 -o pEL5sslongcassette.bam -t 7

python exon_covering.py -i p3l5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/shortcassette.bed4 -o pEL5ssshortcassette.bam -t 7

python exon_covering.py -i p3l5ss.bam -b /work/users/ccalovichbenne/plseq/mec2PL/bed4files/lastexon.bed4 -o pEL5sslastexon.bam -t 7
