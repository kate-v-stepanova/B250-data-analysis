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

where `ENST00000379370.2` is a transcript_id, `GATGAC` is a codon, and `2568` is a coordinate (0-based) of this codon in CDS of the transcript. 

## 0. Generate reference files - already done

To generate the reference files, run the following command:

```
bsub -R "rusage[mem=100G]" python $BASE_DIR/software/ext_diricore/triplets/0_get_codon_positions.py 2 hg19
```

This will generate reference files for 2x codons in all amino acids.

The output will be written in this directory: `$BASE_DIR/static/hg19//omics/groups/OE0532/internal/from_snapshot/static/hg19/codons/2x`

For 1x and 3x codons, adjust the first parameter, `1` or `3` instead of `2`. For a different genome, adjust the second parameter, e.g. `mm10` instead of `hg19`

## 1. Get sequences from bam

```
bsub -R "rusage[mem=10G]" $BASE_DIR/software/diricore_subset/1_get_seq_from_bam.sh 17245 all_unique
```

## 2. 


# RPF coverage plots for combination of amino acids

Sometimes we are interested in a combination of codons from different amino acids.

For example, we want to find the combination of 2 codons of Asparatic acid and 1 codon of Proline: `asp_asp_pro`.

It means, that we are interested in the following 16 codons: 

```
GAT-GAC-CCA
GAT-GAC-CCC
GAT-GAC-CCG
GAT-GAC-CCT
GAC-GAT-CCA
GAC-GAT-CCC
GAC-GAT-CCG
GAC-GAT-CCT
GAC-GAC-CCA
GAC-GAC-CCC
GAC-GAC-CCG
GAC-GAC-CCT
GAT-GAT-CCA
GAT-GAT-CCC
GAT-GAT-CCG
GAT-GAT-CCT
```

## 0. Generate reference file

```
python $BASE_DIR/software/ext_diricore/triplets/0_get_codon_positions_aa_combination.py asp_asp_pro hg19
```

This will create the following file: `$BASE_DIR/static/hg19/codons/3x/aa_combinations/asp_asp_pro.tsv`

