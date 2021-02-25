#Unix Assignment

##Data Inspection

###Attributes of "fang_et_al_genotypes"


$ ls -lh fang_et_al_genotypes.txt

1. I used this code to figure out the file size
2. The -lh command simplifies the file size and outputted that file was ~11M in size


$ wc fang_et_al_genotypes.txt

1. This provided me with the number of lines words and characters in the file
2. This file contained 2783 lines, 2744038 words, and 11051939 characters

 
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt

1. This provded me with the number of columns in the file that are separated by white space
2. The command reported that there are 986 columns in the file 

 
$ wc transposed_genotypes.txt

1. After transposing fang_et_al_genotypes.txt I used wc on the new file created 
2. the purpose of this was to ensure the command worked by seeing if the column number of fang_et_al_genotypes.txt (which was 986) is now the number of lines in transposed_genotypes.txt
3. The transpose.awk command worked since the lines and columns numbers were flipped in transposed_genotypes.txt in comparison to fang_et_al_genotypes.txt

###Attributes of snp_position.txt

 
$ ls -lh snp_position.txt

1. this command outputs the file size 
2. This file was significantly smaller than fang_et_al_genotypes.txt 
3. The file had a size of 81K

 
$ wc snp_positions.txt

1. This command prints teh number of lines, words, and characters in the file 
2. this file has 984 lines, 13198 words, and 82763 characters


$ awk -F "\t" '{print NF; exit}' snp_positions.txt

1. this prints out the number of columns present in the file 
2. this file contains 15 columns 

 
$ head -n 2 snp_position.txt | column -t

1. first outputted the first two lines of the file and piped them to a command that would sort the lines into a readable output
2. I saw the contents of the header and noticed thre important columsn that were SNP_ID, Chromosome, and Position

##Data Processing

###Maize Data


$ awk -v RS="\t" '/^ZMMIL/{print NR;}' master_SCPG.txt

1. First want to mention that the pattern ZMMIL was also swapped with ZMMLE and ZMMMR
2. This code tells me what the column numbers are in the txt file that contain the pattern I put after the ^ 
3. I then take the ranges of when each group appeared and used it in my next code 


$ cut -f 1-3,1213-2785 master_SCPG.txt > master_SCPG_maize.txt

1. this code was used to form a txt file that contains the first three columns with SNP_ID, Chromosome #, and Position. the remaining columns are represented by the genotype data from the groups signfied in the prior code. 
2. Since the maize group consists of ZMMIL, ZMMLR, and ZMMMR, I created a txt file consisting of only these groups to then build other files off of. 


$ awk '$2 == 1' master_SCPG_maize.txt | sort -n -k3 | sed 's/multiple/?/g' > chromosome1_maize.txt

1. this code first searches field 2 for any lines that contain the value 1
2. Once all lines are outputted to the next command, they are then sorted in numerical order by field 3
3. lastly, anything that matches the string "multiple" will be replaced with a "?" and it is not important to specify the field since "mutiple" can only occur in $3 
4. this code will also be used for all other chromosomes in increasing numerical order. I will simply swap the value that $2 is equivalent to with the respecting chromosome number and change that in the file name it is outputted to. 


$ awk '$2 == 1' master_SCPG_maize.txt | sort -n -r -k3 | sed 's/multiple/-/g' > chromosome1_maize_decreasing.txt

1. this code is xactly the same as the one above except for 3 changes
2. in the sort program I added -r after -n to indicate that it is numerical but in the reverse order
3. in the sed program I changed what character would swpa the place of "multiple" and now a "-" appears for when "multiple" was indicated in the $3
4. I also changed the name of the output file by adding decreasing at the end to indicate this file has reverse numerical order 


$ grep "unknown" master_SCPG_maize.txt | sort -k1 > unknown_maizeSNP.txt

1. this command searches for the string "unknown" in the indicated file and when it finds all "unknown" strings it outputs those lines
2. I then sorted them by the first field for fun and outputted everything into a file name unknown_maizeSNP.txt 


$ awk '$2~/^multiple/' master_SCPG_maize.txt > multiple_maizeSNP.txt

1. this code searches for the string "multiple" within the $2 of the master_SCPG_maize.txt file
2. any matches it comes up with will pull the entire line and output it to the file multiple_maizeSNP.txt 


###Teosinte Data


$ awk -v RS="\t" '/^ZMPBA/{print NR;}' master_SCPG.txt

1. using the same code as from the maize data, we will simply replace the pattern following ^ with the groups that represent teosinte which are ZMPBA, ZMPIL, and ZMPJA


$ cut -f 1-3, 77-1010, 1166-1206 master_SCPG.txt > master_SCPG_teosinte.txt

1. using the same code as from the maize data, we will replace the field values with the input we got back from the previous code. This will cut out the fields indicated after the -f command and then compile them together into a file named master_SCPG_teosinte.txt


$ awk '$2 == 1' master_SCGP_teosinte.txt | sort -n -k3 | sed 's/multiple/?/g' > chromosome1_teosinte.txt

1. using the same code as from the maize data, we will replace the file we run our awk command on with master_SCGP_teosinte.txt and also change our output file name to chromosome1_teosinte.txt
2. once again, this code was inputted multiple times to accomdate for their being 10 chromosomes and this was accomplished by changing the number that has to match in $2 to the chromosome number and changing the outpu file name. 


$ awk '$2 == 1' master_SCGP_teosinte.txt | sort -n -r -k3 | sed 's/multiple/-/g' > chromosome1_teosinte_decreasing.txt

1. using the same code as from the maize data, we simply replace the files to match the teosinte files. 


$ grep "unknown" master_SCPG_teosinte.txt | sort -k1 > unknown_teosinteSNP.txt

1. using the same code as from the maize data, we simply replace the files to match the teosinte files. 


$ awk '$2~/^multiple/' master_SCPG_teosinte.txt > multiple_teosinteSNP.txt

1. using the same code as from the maize data, we simply replace the files to match the teosinte files. 
   
