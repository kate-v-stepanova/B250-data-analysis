
SRA toolkit

$BASE_DIR/software/sratoolkit.2.9.6-1-centos_linux64/bin

compiled CentOS (May 7, 2019): https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.9.6-1/sratoolkit.2.9.6-1-centos_linux64.tar.gz

Installation guide: https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software


Convert SRA to fastq: `$BASE_DIR/software/sra/sra_to_fastq.sh mito_riboseq`

`mito_riboseq` is a a project_id

the `.sra` files should be located in: `$BASE_DIR/mito_riboseq/analysis/input/sra/*.sra` 

and the output will be written to: `$BASE_DIR/mito_riboseq/analysis/output/demultiplexed`
