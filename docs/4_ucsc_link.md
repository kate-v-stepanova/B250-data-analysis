# UCSC link

## 1. Create UCSC tracks

```
/icgc/dkfzlsdf/analysis/OE0532/software/diricore/generate_ucsc_tracks.sh 21012 mm10 all_unique
```

This will output the following:

```
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S10_CoC_NG_LG.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S1_E0771_HG.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S2_E0771_LG.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S3_E0771_LC.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S4_CoC_GFP_HG.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S5_CoC_GFP_LG.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S6_MEF_HG.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S7_MEF_LG.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S8_MEF_LC.sh
bsub -q long -R "rusage[mem=30G]" /icgc/dkfzlsdf/analysis/OE0532/tmp/21012/ucsc/S9_CoC_NG_HG.sh
```

Copy & paste these commands to submit the jobs. 

## 2. Create annotation file

```
/icgc/dkfzlsdf/analysis/OE0532/software/diricore/ucsc_track_annotation.sh 21012 mm10 all_unique
```

This will output the following:

```
Created file: /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/ucsc_track_annotation.txt
```

## 3. Upload tracks to the FTP

```
python /icgc/dkfzlsdf/analysis/OE0532/software/scripts/ucsc_tracks_to_ftp.py 21012 all_unique
```

This will output the following:

```
2021-02-25 13:56:35
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S4_CoC_GFP_HG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S2_E0771_LG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S6_MEF_HG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S9_CoC_NG_HG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S6_MEF_HG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S4_CoC_GFP_HG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S9_CoC_NG_HG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S5_CoC_GFP_LG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S3_E0771_LC_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S1_E0771_HG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S3_E0771_LC_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S7_MEF_LG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S8_MEF_LC_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S10_CoC_NG_LG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S1_E0771_HG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S7_MEF_LG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S2_E0771_LG_plus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S10_CoC_NG_LG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S5_CoC_GFP_LG_minus.bw
Uploading /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/S8_MEF_LC_minus.bw
```

> **_NOTE:_** DKFZ FTP-server stores files for a certain period of time, and then deletes them. 

## 4. Create UCSC session

This has to be done manually via UCSC web-site. 

1. Open the link: http://genome.ucsc.edu/cgi-bin/hgCustom
2. Delete existing tracks if there are any

[](/pics/ucsc_1.png)

3. Add custom tracks

[](/pics/ucsc_2.png)

4. Add tracks & select the genome version

Copy & paste the annotation file: `less /icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/gen_tracks/all_unique/ucsc_track_annotation.txt

```
browser
track name=S10_CoC_NG_LG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S10_CoC_NG_LG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S10_CoC_NG_LG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S10_CoC_NG_LG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S1_E0771_HG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S1_E0771_HG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S1_E0771_HG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S1_E0771_HG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S2_E0771_LG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S2_E0771_LG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S2_E0771_LG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S2_E0771_LG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S3_E0771_LC_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S3_E0771_LC_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S3_E0771_LC_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S3_E0771_LC_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S4_CoC_GFP_HG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S4_CoC_GFP_HG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S4_CoC_GFP_HG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S4_CoC_GFP_HG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S5_CoC_GFP_LG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S5_CoC_GFP_LG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S5_CoC_GFP_LG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S5_CoC_GFP_LG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S6_MEF_HG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S6_MEF_HG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S6_MEF_HG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S6_MEF_HG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S7_MEF_LG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S7_MEF_LG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S7_MEF_LG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S7_MEF_LG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S8_MEF_LC_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S8_MEF_LC_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S8_MEF_LC_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S8_MEF_LC_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S9_CoC_NG_HG_minus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S9_CoC_NG_HG_minus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
track name=S9_CoC_NG_HG_plus db=mm10 type=bigWig bigDataUrl=ftp://ftp.dkfz-heidelberg.de/outgoing/B250/ucsc/21012/all_unique/S9_CoC_NG_HG_plus.bw visibility=full alwaysZero=on yLineOnOff=on yLineMark=0 maxHeightPixels=40
```

5. Open tracks in the UCSC browser
[](/pics/ucsc_4.png)

And view the results
[](/pics/ucsc_5.png)

6. Save session

Open the UCSC sessions: `http://genome.ucsc.edu/cgi-bin/hgSession`

> **_NOTE_** You need to be registered, to be able to save sessions

Save the UCSC link: 

[](/pics/ucsc_6.png)

Now the ucsc link can be shared: `http://genome.ucsc.edu/s/stephz/21012_all_unique`

> **_NOTE:_** DKFZ FTP-server stores files for a certain period of time, and then deletes them. 
