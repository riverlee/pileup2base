pileup2base
===========

Parse samtools pileup file to get how many bases and what kind of bases are called.

## Update (08/04/2012)
Script **pileup2baseindel.pl** is a update version of old one **pileup2base.pl**(Keep in the archive without change). The update includes following:
  
  1. it will parse the insert and deletion too, 
  2. it can parse pileups from multiple sample,
  3.you don't need to define the outputfile,instead, 
   you will need to provide 'prefix' which will name result from each sample as prefix1.txt,  prefix2.txt,etc. Default, prefix is sample.
  4. Provide option to define the base quality score offset, default is 33 (Sanger standard).
  5. Read parameters from command line with options

Details
-------------------

Pileup format is described at [http://samtools.sourceforge.net/pileup.shtml](http://samtools.sourceforge.net/pileup.shtml). It describes information including reference allele, depth, read bases and base qualities. For read bases, we can also know the strand information for each base. However, the format is not easy for people to read. This program helps people to convert the original format into a human readable one and it can also be used as input to calcualte strand bias.


## Example with pileup2baseindel.pl (new version)
Original pileup (2 samples) input (see test.mpileup).
```
1	888659	T	14	c$CCCCcccCcCccc	GCECEFGEEBGEE@	11	.$...-3ATT...+2AG.+2AG.+4AGAGGG	<975;:<<<<<
1	1268847	T	0	*	*	11	.$..........	BGEGEEGFGEF
1	1421531	C	27	aAaaAAAAAaaaaAaAAAaAAaaAaa^]a	GEDGEBGEECGCFF-DGEBGGB?FEA:	C	17	.,,.,.,,,,,.,.,..	DGFEGEGBEEEGAGEGG
1	2938697	T	22	CgGGggCGgGgGGggggggggG	#G=:CG#5E@E#:#A##?@C#E	20	,,,,,..,.-4CACC.-4CACC....,.,,.^~.	==<<<<<<<<<<<::<;2<<

```
After apply ***perl pile2baseindel.pl -i test.mpileup***, Here is the result.


**sample1.txt**
```
chr	loc	ref	A	T	C	G	a	t	c	g	Insertion	Deletion
1	888659	T	0	0	6	0	0	0	8	0	NA	NA
1	888659	T	13	0	0	0	14	0	0	0	NA	NA
1	888659	T	0	0	2	7	0	0	0	13	NA	NA
```

**sample2.txt** (2:AG|1:AGAG means there are deletions with **AG** occur twice and **AGAG** once. NA means no insertin or deletion in this position)
```
chr	loc	ref	A	T	C	G	a	t	c	g	Insertion	Deletion
1	888659	T	0	9	0	2	0	0	0	0	2:AG|1:AGAG	1:ATT
1	888659	T	0	11	0	0	0	0	0	0	NA	NA
1	888659	T	0	0	0	0	0	0	0	0	NA	NA
1	888659	T	0	11	0	0	0	9	0	0	NA	2:CACC
```


Example with pileup2base.pl (old version)
---------------------------------
Original pileup format
```
1   888659	T	14	c$CCCCcccCcCccc	GCECEFGEEBGEE@
1	1268847	T	25	,G...,..GG.....G..GGG,.g^],	GE8BEF==B?EGCB=BA@DFG=EBA
1	1421531	C	27	aAaaAAAAAaaaaAaAAAaAAaaAaa^]a	GEDGEBGEECGCFF-DGEBGGB?FEA:
1	1888193	C	17	.,,.,.,,,,,.,.,..	DGFEGEGBEEEGAGEGG
1	2283117	C	24	.,.....................^].	?GE#GFGEFBEGGGFGGGGGGGDG
1	2938697	T	22	CgGGggCGgGgGGggggggggG	#G=:CG#5E@E#:#A##?@C#E
1	3742257	C	11	,....,.....	GGFGFEGEDDG
1	3743391	C	28	,$t,t,.t,tttt,t,t,t.t,,t,,t,t	G=GEGAGGGFGGEEGGGFGBGGGGCEEE
1	3753813	T	11	.$..........	BGEGEEGFGEF
1	3756074	C	12	............	GEGG==GFGDDE
```
After format conversion(upper ATCG means the read base comes from forward strand, while the lower means the read base comes from reverse strand )
```
chr  loc	ref	A	T	C	G	a	t	c	g
1	888659	T	0	0	6	0	0	0	8	0
1	1268847	T	0	13	0	7	0	4	0	1
1	1421531	C	13	0	0	0	14	0	0	0
1	1888193	C	0	0	7	0	0	0	10	0
1	2283117	C	0	0	23	0	0	0	1	0
1	2938697	T	0	0	2	7	0	0	0	13
1	3742257	C	0	0	9	0	0	0	2	0
1	3743391	C	0	0	2	0	0	14	12	0
1	3753813	T	0	11	0	0	0	0	0	0
1	3756074	C	0	0	12	0	0	0	0	0

```


