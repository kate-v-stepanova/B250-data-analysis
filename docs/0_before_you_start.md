# Before you start

Before running the bioinformatic scripts, the following has to be configured:

1. BASE_DIR

This has to be set as an environmental variable. When running a script, the script will try to load this variable. 

Open the `.bash_profile`: `vim ~/.bash_profile`

And add this line: `export BASE_DIR="/omics/groups/OE0532/internal/from_snapshot/"`

2. Configure .bash_profile (optionally)

For more convenient work, `~/.bash_profile` can be configured as following: 

```
# colors
export LC_CTYPE="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"
export LC_CTYPE="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"
export PS1='\u@\H:\w$ ';
export PS1="\[${cyan}\](\$(basename \$CONDA_DEFAULT_ENV 2> /dev/null)) "$PS1;

# conda
module load anaconda2/2019.07

# for RStudio
if ps -e | grep -q rserver; then
  module load gcc/7.2.0
  module load libpng/1.6.37
  module load hdf5/1.8.18
  #/icgc/dkfzlsdf/analysis/OE0532/tmp/RStudio_data
else
    source activate p3
fi

# BASE_DIR
export BASE_DIR="/omics/groups/OE0532/internal/from_snapshot/"
```

3. Virtual env for python3

Mainly the scripts will be running under python 3.

Load anaconda: `module load anaconda2/2019.07`

Create conda environment: `conda create -n p3 python=3.7` 

Activate env: `conda activate p3` or `source activate p3`

4. Virtual env for python2

Python2 is mainly used by diricore and couple of more tools.

Load anaconda:  `module load anaconda2/2019.07`

Create conda environment: `conda create -n diricore python=2.7` 

Activate env: `conda activate diricore` or `source activate diricore`


