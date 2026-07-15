# Running Isoquant

conda create -c conda-forge -c bioconda -n isoquant python=3.8 isoquant
isoquant.py --test
module load gcc
conda activate isoquant


# Stomatin sorted then Promoter sorted by PL-seq
isoquant.py --reference /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa --genedb /work/users/ccalovichbenne/GenomeFiles/Celegans.WBcel235.77_transcript.gtf --bam /work/users/ccalovichbenne/plseq/mec2PL/2parse/*.bam --data_type pacbio_ccs --transcript_quantification all --gene_quantification all --count_exons --sqanti_output --threads 7

# Promoter H sorted
isoquant.py --reference /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa --genedb /work/users/ccalovichbenne/GenomeFiles/Celegans.WBcel235.77_transcript.gtf --bam /work/users/ccalovichbenne/plseq/mec2PL/2parse/promoterH.bam --data_type pacbio_ccs --transcript_quantification all --gene_quantification all --count_exons --sqanti_output --threads 7

# 9A vs 9B splicing event from sorting events after promoter sort
isoquant.py --reference /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa --genedb /work/users/ccalovichbenne/GenomeFiles/Celegans.WBcel235.77_transcript.gtf --bam /work/users/ccalovichbenne/plseq/mec2PL/2parse/9Avs9B/*.bam --data_type pacbio_ccs --transcript_quantification all --gene_quantification all --count_exons --sqanti_output --threads 7

# Long vs short exon9A 5’ss
isoquant.py --reference /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa --genedb /work/users/ccalovichbenne/GenomeFiles/Celegans.WBcel235.77_transcript.gtf --bam /work/users/ccalovichbenne/plseq/mec2PL/2parse/alt5ss/*.bam --data_type pacbio_ccs --transcript_quantification all --gene_quantification all --count_exons --sqanti_output --threads 7

# Measure cassette in/ex from promoter pE and pH 
isoquant.py --reference /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa --genedb /work/users/ccalovichbenne/GenomeFiles/Celegans.WBcel235.77_transcript.gtf --bam /work/users/ccalovichbenne/plseq/mec2PL/2parse/promotercassette/pH/*.bam --data_type pacbio_ccs --transcript_quantification all --gene_quantification all --count_exons --sqanti_output --threads 7

isoquant.py --reference /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa --genedb /work/users/ccalovichbenne/GenomeFiles/Celegans.WBcel235.77_transcript.gtf --bam /work/users/ccalovichbenne/plseq/mec2PL/2parse/promotercassette/pE_9A/*.bam --data_type pacbio_ccs --transcript_quantification all --gene_quantification all --count_exons --sqanti_output --threads 7
