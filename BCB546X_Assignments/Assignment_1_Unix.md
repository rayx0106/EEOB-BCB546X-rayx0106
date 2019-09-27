#BCB546X Fall 2019

##UNIX_Assignment

##David D. Ray

##Data Inspection

###Attributes of `fang_et_al_genotypes.txt`

####File Size
```
$ du -h fang_et_al_genotypes.txt 	#inspection of file size
```
#####Standard Output 

	* 6.1M
	
####Word Count	
```
$ wc fang_et_al_genotypes.txt	#details the number of lines, words, and bytes
```
#####Standard Output
	* 2783 lines
	* 2744038 words
	* 11051939 bytes

	
####Inspecting the Number of Fields
```
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt	#awk one-line returns how many fields (columns)
```

#####Standard Output
	* 986 
with so many fields we will need to perform a `cut` command to extract specific columns

####Header Inspection
```
$ cut -f 1-5 fang_et_al_genotypes.txt | head -n1 #cut -f 1-5 specifies which fields to keep and the standard output is piped to the head command which only prints the first line
```
#####Standard Output
```
SNP_ID	cdv_marker_id	Chromosome	Position	alt_pos		
```



###Attributes of `snp_position.txt`
####File Size
```
$ du -h snp_position.txt 	#inspection of file size
```
#####Standard Output 

	* 38K
	
####Word Count	
```
$ wc snp_position.txt	#details the number of lines, words, and bytes
```
#####Standard Output
	* 984 lines
	* 13198 words
	* 82763 bytes

####Inspecting the Number of Fields
```
$ awk -F "\t" '{print NF; exit}' snp_position.txt	#awk one-line returns how many fields (columns)
```

#####Standard Output
	* 15

####Header Inspection
```
$ $ cut -f 1-5 snp_position.txt | head -n1	#specifies which fields to keep and the standard output is piped to the head command which only prints the first line
```
#####Standard Output
```
SNP_ID	cdv_marker_id	Chromosome	Position	alt_pos
```

		


##Data Format Editing for Downstream Analysis


###Extracting Fields 1-3 from snp_position.txt 
```
$ cut -f 1,3,4 snp_position.txt > snp_position_extracted_columns.txt #this extracts field 1 (SNP_ID), field 2 (Chromosome), and field 3 (Position) and sends the stand output to snp_position_extracted_columns.txt
```

###Extract group SNP genotype data
``` 
$ grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
```
```
grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt
```
	
This extracted group SNP genotypes from the specified field


###Transpose extracted group SNP genotype file
```
$ awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt
```
```
awk -f transpose.awk teosinte_genotypes.txt > transposed_maize_genotypes.txt
```
We transpose the field to record in order to match snp_position.txt
						
						
###Extract the matching record from the two transposed files (maize and teosinte) and extract the corresponding match from snp_position.txt 
```
$ sed -i '1,3d' transposed_maize_genotypes.txt transposed_teosinte_genotypes.txt
```
Deletion of records 1 - 3 on both tranposed files

```
$ sed '1d' snp_position.txt > snp_position_extracted_header.txt
```
Deletion of record 1 

###Sort snp_position and the maize/teosinite genotypes

```
$ sort -k1,1V snp_position_extracted_header.txt > snp_position_extracted_header_sorted.txt

$ sort -k1,1V transposed_maize_genotypes.txt > transposed_maize_genotypes_sorted.txt

$ sort -k1,1V transposed_teosinte_genotypes.txt > transposed_teosinte_genotypes_sorted.txt
```
Start,end (-k1,1) numerically within a sting using -V


###Join

```
$ join -1 1 -2 1 -t $'\t' snp_position_extracted_header_sorted.txt transposed_teosinte_genotypes_sorted.txt > transposed_teosinte_genotypes_snp_position_joined.bed
```

```
$ join -1 1 -2 1 -t $'\t' snp_position_extracted_header_sorted.txt transposed_maize_genotypes_sorted.txt > transposed_maize_genotypes_snp_position_joined.bed
```
Syntax: file 1 field 1 is to be joined with file 2 field 1 in a tab delimited format
Output: maize and snp_positions plus teosinte and snp_position
	
###Sort and Uniq	
We want to use UNIQ -c to identify the number of duplicates of chromosomes, but first we need to sort the chromosome field						
 
syntax extract everything but the header pipe to cut and extract just field 3 (-f3) pipe to uniq (-c) for a standard output that tells us the number of unique


