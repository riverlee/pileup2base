pileup2base
===========

Parse samtools pileup file to get how many bases and what kind of bases are called.

Details
-------------------

Pileup format is described at [http://samtools.sourceforge.net/pileup.shtml](http://samtools.sourceforge.net/pileup.shtml). It describes information including reference allele, depth, read bases and base qualities. For read bases, we can also know the strand information for each base. However, the format is not easy for people to read. This program helps people to convert the original format into a human readable one and it can also be used as input to calcualte strand bias.

Example
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


