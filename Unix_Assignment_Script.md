#BCB546 UNIX Assignment

##Data Inspection
The transposed data of fang_et_al_genotypes.txt was used for the assignment to allow for merging. Thus, transposed_genotypes.txt was inspected together with snp_positions.txt

###Attributes of `transposed_genotypes.txt`

```
here is my snippet of code used for data inspection
1. less transposed_genotypes.txt
2. head transposed_genotypes.txt
3. tail transposed_genotypes.txt
4. vim transposed_genotypes.txt
5.:set nowrap
6. awk -F "\t" '{print NF; exit}' transposed_genotypes.txt
7. du -h transposed_genotypes.txt
8. wc transposed_genotypes.txt
9. file transposed_genotypes.txt
10. grep -c "^$" transposed_genotypes.txt
```

By inspecting this file I learned that:

1. transposed_genotypes.txt has 2783 columns
2. the file size is 3.7M
3. the file has 986 lines, 2744038 words and 11051939 byes
4. file has ASCII characters with very long lines
5. there are no blank lines in the file
6. This file has column named Group which is similar to SNP_ID in snp_position.txt

###Attributes of `snp_position.txt`

```
here is my snippet of code used for data inspection
1. less snp_position.txt
2. head snp_position.txt
3. tail snp_position.txt
4. vim snp_position.txt
5.:set nowrap
6. awk -F "\t" '{print NF; exit}' snp_position.txt
7. du -h snp_position.txt
8. wc snp_position.txt
9. file snp_position.txt
10. grep -c "^$" snp_position.txt
```

By inspecting this file I learned that:

1. snp_position.txt has 15 columns
2. the file size is 49K
3. the file has 984 lines, 13198 words and 82763 byes
4. file has ASCII characters
5. there are no blank lines in the file
6. the first column of snp_position.txt is SNP_ID

##Data Processing

###Maize Data

```
here is my snippet of code used for data processing
awk '{for(i=1; i<=NF; i++) print $i}' <(head -n 1 snp_position.txt) | sort > cols1.txt
awk '{for(i=1; i<=NF; i++) print $i}' <(head -n 1 transposed_genotypes.txt) | sort > cols2.txt
comm -12 cols1.txt cols2.txt
grep -F -f <(head -n 1 snp_position.txt | tr '\t' '\n' | sort) <(head -n 1 transposed_genotypes.txt | tr '\t' '\n' | sort)
tail -n +3 transposed_genotypes.txt > tranposed_1header.txt
cat transposed_1header.txt
tail -n +6 transposed_1header.txt | awk -F "\t" '{print NF; exit}'
head -n 1 snp_position.txt
sed 's/Group/SNP_ID/g' transposed_1header.txt | head -n -1 > transposed_SNP_ID.txt
cat transposed_SNP_ID.txt
join -1 1 -2 1 -a 1 snp_position.txt transposed_SNP_ID.txt > merged.txt
wc merged.txt
cat merged.txt
sed 's/Group/SNP_ID/g' transposed_1header.txt > transposed_SNP_ID.txt
join -1 1 -2 1 -t $'\t' snp_position.txt transposed_SNP_ID.txt > join_lengths.txt
awk '{for(i=1;i<=NF;i++) if ($i == "ZMMIL") print "Found at column " i}' join_lengths.txt
awk '{for(i=1;i<=NF;i++) if ($i == "ZMMMR") print "Found at column " i}' join_lengths.txt
awk '{for(i=1;i<=NF;i++) if ($i == "ZMMLR") print "Found at column " i}' join_lengths.txt
cut -d $'\t' -f1,3,4,2508-2797,1225-2480,2481-2507 join_lengths.txt > maize.txt
sort -k3,3n maize.txt > maize_sort.txt
(head -n 1 maize.txt && tail -n +2 maize.txt | sort -k3,3n) > Maize_sorted.txt
wc Maize_sorted.txt
grep -v "^#" Maize_sorted.txt | cut -f2 | sort | uniq -c
for i in {1..10}; do
    { head -n 1 Maize_sorted.txt && awk -v chr="$i" '$2 == chr' Maize_sorted.txt; } > "Maize_chr${i}.txt"
done
{ head -n 1 Maize_sorted.txt && awk '$2 == "multiple"' Maize_sorted.txt; } > Maize_chrm.txt
{ head -n 1 Maize_sorted.txt && awk '$2 == "unknown"' Maize_sorted.txt; } > Maize_chru.txt
awk -F "\t" '{print NF; exit}' Maize_chr1.txt Maize_chr2.txt Maize_chr3.txt Maize_chr4.txt Maize_chr5.txt Maize_chr6.txt Maize_chr7.txt Maize_chr8.txt Maize_chr9.txt Maize_chr10.txt Maize_chrm.txt Maize_chru.txt
cat Maize_chr1.txt
cat Maize_chr7.txt
cat Maize_chru.txt
mkdir Maize_ascend
mv Maize_chr1.txt Maize_chr2.txt Maize_chr3.txt Maize_chr4.txt Maize_chr5.txt Maize_chr6.txt Maize_chr7.txt Maize_chr8.txt Maize_chr9.txt Maize_chr10.txt Maize_chrm.txt Maize_chru.txt Maize_ascend
(head -n 1 Maize_hyphen.txt && tail -n +2 Maize_hyphen.txt | sort -k3,3nr) > Maize_descend.txt
for i in {1..10}; do
    { head -n 1 Maize_descend.txt && awk -v chr="$i" '$2 == chr' Maize_descend.txt; } > "Maize_chrd${i}.txt"
awk -F "\t" '{print NF; exit}' Maize_chrd1.txt Maize_chrd2.txt Maize_chrd3.txt Maize_chrd4.txt Maize_chrd5.txt Maize_chrd6.txt Maize_chrd7.txt Maize_chrd8.txt Maize_chrd9.txt Maize_chrd10.txt
cat Maize_chrd1.txt
cat Maize_chrd5.txt
cat Maize_chrd10.txt
mkdir Maize_descednd
mv Maize_chrd1.txt Maize_chrd2.txt Maize_chrd3.txt Maize_chrd4.txt Maize_chrd5.txt Maize_chrd6.txt Maize_chrd7.txt Maize_chrd8.txt Maize_chrd9.txt Maize_chrd10.txt Maize_descend
```

Here is my brief description of what this code does
62.	Extracts the first line from snp_position.txt, splits by tab, prints each field, sorts them, and stores in cols1.txt.
63.	Same as above, but for the first line of transposed_genotypes.txt, stores in cols2.txt.
64.	Finds and displays common lines between cols1.txt and cols2.txt.
65.	Finds matching fields between the first line of snp_position.txt and transposed_genotypes.txt, converting tabs to newlines.
66.	Skips the first two lines of transposed_genotypes.txt and saves the rest to tranposed_1header.txt.
67.	Displays the contents of tranposed_1header.txt.
68.	Skips the first five lines of transposed_1header.txt, then prints the number of fields in the 6th line.
69.	Displays the first line of snp_position.txt.
70.	Replaces "Group" with "SNP_ID" in transposed_1header.txt, removes the last line, and saves the result to transposed_SNP_ID.txt.
71.	Displays the contents of transposed_SNP_ID.txt.
72.	Joins snp_position.txt and transposed_SNP_ID.txt based on the first column, with unmatched lines from snp_position.txt included, and stores in merged.txt.
73.	Counts the number of lines, words, and characters in merged.txt
74.	Displays the contents of merged.txt.
75.	Replaces "Group" with "SNP_ID" in transposed_1header.txt and stores it in transposed_SNP_ID.txt.
76.	Joins snp_position.txt and transposed_SNP_ID.txt based on the first column, using a tab delimiter, and stores in join_lengths.txt.
77.	Searches for "ZMMIL" in join_lengths.txt and prints the column.
78.	Searches for "ZMMMR" in join_lengths.txt and prints the column number.
79.	Searches for "ZMMLR" in join_lengths.txt and prints the column number.
80.	Extracts specified columns from join_lengths.txt and stores them in maize.txt.
81.	Sorts maize.txt by the 3rd column in numeric order and saves the result to maize_sort.txt.
82.	Sorts maize.txt by the 3rd column, keeping the first line intact, and saves to Maize_sorted.txt.
83.	Counts lines, words, and characters in Maize_sorted.txt.
84.	Filters out lines starting with "#", extracts the second column, sorts, and counts unique occurrences.
85-87.	Loops through chromosomes 1-10, extracts matching rows, and saves them to separate files (Maize_chr1.txt to Maize_chr10.txt).
88.	Extracts rows with "multiple" in the second column and saves to Maize_chrm.txt.
89.	Extracts rows with "unknown" in the second column and saves to Maize_chru.txt.
90.	Prints the total number of columns of all file.
91.	Displays the contents of Maize_chr1.txt.
92.	Displays the contents of Maize_chr7.txt.
93.	Displays the contents of Maize_chru.txt.
94.	Creates a directory called Maize_ascend.
95.	Moves the Maize chromosome files into the Maize_ascend directory.
96.	Sorts Maize_hyphen.txt in descending order based on the 3rd column and saves to Maize_descend.txt.
96-98.	Loops through descending chromosomes 1-10, extracts matching rows from Maize_descend.txt, and saves to separate files.
99.	Prints the total number of columns of all files
100.	Displays the contents of Maize_chrd1.txt.
101.	Displays the contents of Maize_chrd5.txt.
102.	Displays the contents of Maize_chrd10.txt.
103.	Creates a directory called Maize_descednd.
104.	Moves the descending chromosome files into the Maize_descend directory. 


###Teosinte Data

```
here is my snippet of code used for data processing
wc join_lengths.txt
awk '{for(i=1;i<=NF;i++) if ($i == "ZMPBA") print "Found at column " i}' join_lengths.txt
awk '{for(i=1;i<=NF;i++) if ($i == "ZMPIL") print "Found at column " i}' join_lengths.txt
awk '{for(i=1;i<=NF;i++) if ($i == "ZMPJA") print "Found at column " i}' join_lengths.txt
cut -d $'\t' -f1,3,4,89-988,1178-1218,989-1022 join_lengths.txt > Teosinte.txt
sort -k3,3n Teosinte.txt > Teosinte_sort.txt
(head -n 1 Teosinte.txt && tail -n +2 Teosinte.txt | sort -k3,3n) > Teosinte_sorted.txt
wc Teosinte_sorted.txt
grep -v "^#" Teosinte_sorted.txt | cut -f2 | sort | uniq -c
for i in {1..10}; do
    { head -n 1 Teosinte_sorted.txt && awk -v chr="$i" '$2 == chr' Teosinte_sorted.txt; } > "Teosinte_chr${i}.txt"
{ head -n 1 Teosinte_sorted.txt && awk '$2 == "multiple"' Teosinte_sorted.txt; } > Teosinte_chrm.txt
{ head -n 1 Teosinte_sorted.txt && awk '$2 == "unknown"' Teosinte_sorted.txt; } > Teosinte_chru.txt
awk -F "\t" '{print NF; exit}' Teosinte_chr1.txt Teosinte_chr2.txt Teosinte_chr3.txt Teosinte_chr4.txt Teosinte_chr5.txt Teosinte_chr6.txt Teosinte_chr7.txt Teosinte_chr8.txt Teosinte_chr9.txt Teosinte_chr10.txt Teosinte_chrm.txt Teosinte_chru.txt
cat Teosinte_chr1.txt
cat Teosinte_chr6.txt
mkdir Teosinte_ascend
mv Teosinte_chr1.txt Teosinte_chr2.txt Teosinte_chr3.txt Teosinte_chr4.txt Teosinte_chr5.txt Teosinte_chr6.txt Teosinte_chr7.txt Teosinte_chr8.txt Teosinte_chr9.txt Teosinte_chr10.txt Teosinte_chrm.txt Teosinte_chru.txt Teosinte_ascend
sed 's/?/-/g' Teosinte_sorted.txt > Teosinte_hyphen.txt
cat Teosinte_hyphen.txt
wc Teosinte_hyphen.txt
awk -F "\t" '{print NF; exit}' Teosinte_hyphen.txt
(head -n 1 Teosinte_hyphen.txt && tail -n +2 Teosinte_hyphen.txt | sort -k3,3nr) > Teosinte_descend.txt
for i in {1..10}; do
    { head -n 1 Teosinte_hyphen.txt && awk -v chr="$i" '$2 == chr' Teosinte_hyphen.txt; } > "Teosinte_chrd${i}.txt"
awk -F "\t" '{print NF; exit}' Teosinte_chrd1.txt Teosinte_chrd2.txt Teosinte_chrd3.txt Teosinte_chrd4.txt Teosinte_chrd5.txt Teosinte_chrd6.txt Teosinte_chrd7.txt Teosinte_chrd8.txt Teosinte_chrd9.txt Teosinte_chrd10.txt
mkdir Teosinte_descend
mv Teosinte_chrd1.txt Teosinte_chrd2.txt Teosinte_chrd3.txt Teosinte_chrd4.txt Teosinte_chrd5.txt Teosinte_chrd6.txt Teosinte_chrd7.txt Teosinte_chrd8.txt Teosinte_chrd9.txt Teosinte_chrd10.txt Teosinte_descend
```

Here is my brief description of what this code does
Teosinte Data was processed using the same set of codes from points 77 to 104 used for Maize Data processing. 
All files named Maize_~.txt were named Teosinte_~.txt following the same format.
Since most of the processing was already done during the Maize data processing, I began Teosinte Data processing using the join_length.txt file which is a file formed after merging intermediate files from snp_position.txt and transposed_genotypes.txt.
The description of codes used for Maize data processing also applies to Teosinte. The difference is just the change in file names from Maize_~.txt to Teosinte_~.txt.


###Pushing to GitHub

```
git init
git remote add origin git@github.com:kofetch/UNIX_Assignment.git
git add Maize_ascend Maize_descend Teosinte_ascend Teosinte_descend
git commit -m "Added Assignment folders"
git status
git remote -v
git push origin master
```

Here is my brief description of what this code does
194.	Initializes a new Git repository in the current directory.
195.	Adds a remote repository named "origin" to the local Git repository.
196.	Stages the specified folders for commit.
197.	Commits the staged changes with a message "Added Assignment folders."
198.	Displays the current status of the Git repository (changes, staged files, etc.).
199.	Lists the URLs of the remote repositories associated with the local repository.
200.	Pushes the local changes to the "master" branch on the remote repository "origin."
