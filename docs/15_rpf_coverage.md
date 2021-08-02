# RPF coverage plots (for 1x, 2x, 3x codons)

Before running this analysis, the reference files has to be pre-generated. Reference files include the coordinates across the CDS of all transcripts where occurence of the selected codons is found.

E.g. we are interested to find all 2x codons of Asparatic acid. Asparatic acid is encoded by 2 codons: `GAC` and `GAT`. 

It means, we are interested in combinations of all codons, which are: `GACGAC`, `GACGAT`, `GATGAC` and `GATGAT`.

We need to know the coordinates of such codons in all transcripts. This is defined in this file:

```
$BASE_DIR/static/hg19/codons/2x/asp_codon_positions.tsv
```

The file looks like this:

```
ENST00000379370.2       GATGAC  2568
ENST00000379288.3       GATGAC  156
ENST00000450390.2       GATGAC  348
ENST00000354700.5       GATGAC  936
ENST00000353662.3       GATGAC  810
ENST00000339381.5       GATGAC  744
ENST00000378891.5       GATGAC  192
ENST00000378888.5       GATGAC  192
ENST00000495558.1       GATGAC  480
ENST00000341028.7       GATGAC  114
ENST00000356200.3       GATGAC  12
ENST00000378638.2       GATGAC  12
```

where `ENST00000379370.2` is a transcript_id, `GATGAC` is a codon, and `2568` is a coordinate (0-based) this codon in CDS of the transcript. 


## 1. Get sequences from bam

```
bsub -R "rusage[mem=10G]" $BASE_DIR/software/diricore_subset/1_get_seq_from_bam.sh 17245 all_unique
```

## 2. 
