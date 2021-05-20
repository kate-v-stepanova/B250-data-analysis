# Diricore on subset of genes

Let's consider the following list of Mitochondrial genes: 

```
MT-ATP8
MT-ATP6
MT-CO1
MT-CO2
MT-CO3
MT-CYB
MT-ND1
MT-ND2
MT-ND3
MT-ND4L
MT-ND4
MT-ND5
MT-ND6
```


1. Create a reference file: `cat /icgc/dkfzlsdf/analysis/OE0532/static/hg19/MT-genes.txt | cut -d '|' -f 1 > /icgc/dkfzlsdf/analysis/OE0532/static/hg19/MT-transcripts.txt`
2. Create output dir: `mkdir -p /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/bam_ids/all/`
3. Get seq IDs from transcriptome bams: `for f in $(ls /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/alignments/toTranscriptome/231*_toTranscriptome.bam); do fn=$(basename $f); fn=${fn%_toTranscriptome.bam}; echo "samtools view $f | fgrep -w -f /icgc/dkfzlsdf/analysis/OE0532/static/hg19/MT-transcripts.txt | cut -f 1 > /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/bam_ids/all/${fn}.txt"; done`
4. Create sam dir: `mkdir -p /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/all_MT-genes/sam/`
5. Create sam headers: `for f in $(ls /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/bam_ids/all/*txt); do fn=$(basename $f); fn=${fn%.txt}; echo "samtools view -H /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/alignments/toGenome/${fn}_toGenome.bam > /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/all_MT-genes/sam/${fn}.sam"; done`
6. Extract reads from genome bams: `for f in $(ls /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/bam_ids/all/*txt); do fn=$(basename $f); fn=${fn%.txt}; echo "samtools view /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/alignments/toGenome/${fn}_toGenome.bam | fgrep -w -f $f >>   /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/all_MT-genes/sam/${fn}.sam"; done`
7. Convert sam to bam: `for f in $(ls /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/all_MT-genes/sam/*.sam); do fn=$(basename $f); fn=${fn%.sam}; echo "samtools view -b $f > /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/all_MT-genes/alignments/toGenome/${fn}_toGenome.bam"; done`

(optionally - can remove sam files: `rm -rf /icgc/dkfzlsdf/analysis/OE0532/18436/analysis/output/diricore_subset/all_MT-genes/sam/`)

8. Run rpf density analysis: 
```
bsub -q long -R "rusage[mem=20G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore_subset/rpf_density_analysis.sh 18436 hg19 5 all_MT-genes
```

9. Subsequence analysis:
```
bsub -q long -R "rusage[mem=20G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore_subset/subsequence_analysis.sh 18436 hg19 5 all_MT-genes
```

10. RPF transcript distribution:

`bsub -q long  -R "rusage[mem=20G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore_subset/plot_rpf_transcript_distribution.sh 18436 hg19 5 all_MT-genes`
