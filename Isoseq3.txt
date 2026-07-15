#running Isoseq3 pipeline

module load gcc
export PATH=$HOME/anaconda3/bin:$PATH
conda create -n isoseq_env -c defaults -c bioconda -c r -c conda-forge isoseq jasmine lima pbaa pbbam pbccs pbcommand pbcopper pbcore pbcoretools pbfusion 
conda install -n isoseq_env -c defaults -c bioconda -c r -c conda-forge pbipa 
conda install -n isoseq_env -c defaults -c bioconda -c r -c conda-forge pbmarkdup pbmm2 pbpigeon pbskera pbsv pbtk recalladapters trgt
conda activate isoseq3_env

#once conda env is setup can reference isoseq.how for the below
lima /work/users/ccalovichbenne/ISOSEQRawFiles/121422ANIsoSeq_SMU_ISOSEQ_CCB_n2_1_4_6-473-m64277e_221226_194658.hifi_reads.bam /work/users/ccalovichbenne/isoseq3.2/NEB_primers.fasta limaoutput.fl.bam --isoseq

isoseq refine /work/users/ccalovichbenne/isoseq3.2/1.lima/limaoutput.fl.NEB_5p--NEB_Clontech_3p.bam /work/users/ccalovichbenne/isoseq3.2/raw/NEB_primers.fasta refine.flnc.bam
## Did not use –require-polya

isoseq cluster /work/users/ccalovichbenne/isoseq3.2/2.refine/refine.flnc.bam clusteroutput.bam --verbose --use-qvs

pbmm2 align --preset ISOSEQ --sort /work/users/ccalovichbenne/isoseq3.2/3.cluster/clusteroutput.bam /work/users/ccalovichbenne/GenomeFiles/Caenorhabditis_elegans.WBcel235.dna.chromosomes.fa mapped.bam
