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
$ cut -f 1,3,4 snp_position.txt > snp_position_extracted_columns.txt #this extracts field 1 (SNP_ID), field 2 (Chromosome), and field 3 (Position) and sends the stand out to snp_position_extracted_columns.txt
```




