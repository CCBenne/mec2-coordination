# Bash DIFF function for sam (readable) files, allows filtering of the inclusive reads from parsing 

module load gcc
module load samtools

# Convert bam files to sam
samtools view -h file.bam > file.sam

# For mass bam to sam format keeping file names the same
## for file in *.bam; do samtools view -h -o "${file%.bam}.sam" "$file"; done

### LAYOUT OF FUNCTION ###
diff -biw out.sam out2.sam | sed -n 's/^> //p' > different.sam
# Out.sam = whatever you do not want in other .sam file
# Out2.sam = includes longer transcripts you want subtracted out
sed -i 1d different.sam
sed -i 1d different.sam
head -12 out2.sam > header.sam
cat header.sam different.sam > differentwheader.sam
samtools view -S -b differentwheader.sam > differentwheader.bam
samtools sort -o differentwheader_sorted.bam differentwheader.bam
samtools index differentwheader_sorted.bam
######

# Filtering long exon 12 out of short exon 12 file. FINAL file is truncated exon 12
diff -biw p0longcassette.sam p0shortcassette.sam | sed -n 's/^> //p' > different.sam
sed -i 1d different.sam
sed -i 1d different.sam
head -12 p0longcassette.sam > header.sam
cat header.sam different.sam > differentwheader.sam
samtools view -S -b differentwheader.sam > differentwheader.bam
samtools sort -o differentwheadersorted.bam differentwheader.bam
samtools index differentwheadersorted.bam

# Filtering cassette out of last exon file. FINAL file is excluded exon 12 from promoter H
diff -biw p0shortcassette.sam p0lastexon.sam | sed -n 's/^> //p' > different.sam
sed -i 1d different.sam
sed -i 1d different.sam
head -12 p0shortcassette.sam > header.sam
cat header.sam different.sam > differentwheader.sam
samtools view -S -b differentwheader.sam > differentwheader.bam
samtools sort -o differentwheadersorted.bam differentwheader.bam
samtools index differentwheadersorted.bam

# Looking at longer 3' UTR from "mid" exon 12 
diff -biw longcassette.sam CassetteLongUTR.sam | sed -n 's/^> //p' > different.sam
sed -i 1d different.sam
sed -i 1d different.sam
head -12 longcassette.sam > header.sam
cat header.sam different.sam > differentwheader.sam
samtools view -S -b differentwheader.sam > differentwheader.bam
samtools sort -o differentwheadersorted.bam differentwheader.bam
samtools index differentwheadersorted.bam

# Looking at shorter 3' UTR from "mid" exon 12 
diff -biw CassetteLongUTR.sam CassetteShortUTR.sam | sed -n 's/^> //p' > different.sam
sed -i 1d different.sam
sed -i 1d different.sam
head -12 CassetteShortUTR.sam > header.sam
cat header.sam different.sam > differentwheader.sam
samtools view -S -b differentwheader.sam > differentwheader.bam
samtools sort -o differentwheadersorted.bam differentwheader.bam
samtools index differentwheadersorted.bam

# Filtering for only short UTR in last exon
diff -biw LastExonLongUTR.sam LastExonShortUTR.sam | sed -n 's/^> //p' > different.sam
sed -i 1d different.sam
sed -i 1d different.sam
head -12 LastExonShortUTR.sam > header.sam
cat header.sam different.sam > differentwheader.sam
samtools view -S -b differentwheader.sam > differentwheader.bam
samtools sort -o differentwheadersorted.bam differentwheader.bam
samtools index differentwheadersorted.bam
