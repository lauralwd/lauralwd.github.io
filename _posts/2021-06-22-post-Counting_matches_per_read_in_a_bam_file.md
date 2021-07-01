---
title: "Counting matches per read in a bam file"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - blog
  - technology
  - bioinformatics
  - genomics
---

## Usecase
A while back, we ran into this issue where we would like to select long PacBio RSII reads from a bam file based on the number of matching nucleotides to a reference.
I couldn't find a tool or script that does this, so here I wrote my own.
You can find the script on [Github](https://gist.github.com/lauralwd/28e7ac3c37140fbf2aa7a6bf360519de)
Or copy paste the text below.
The script takes two arguments: a bamfile as input and a tab-delimited text file as output.

```
./matches-in-bam.sh <input.bam> <output.tab>
```

## What it does in the background
The only requirement is samtools, which should be in your path.
The script will read the bamfile, itterate over it, summing up all matches.
It will then save the name of the read, and the number of matches to the reference in a table.

It can happen that any read maps to multiple positions. 
The script checks if this is the case, and then does several things:
First, it makes a list with all multimapping reads, if you'd like to remove these from a bam file for example.
Second, it creates the same table as the initial one, except without any of the multimappers.
Finally, it creates again a similar table, but sums up all hits per multimapping read. 
In that case all multimappers will only appear once in the table, with a total amount of matching nucleotides from all places it aligned.
This final step is implemented in quite an ugly and inefficient way, so if you don't need this feel free to kill the script when in progress. 
It is quite verbose when it's working, so you will know once you are there.


### The script
```
#!/bin/bash
# This script takes a bam file, then creates a table with the total amount of matches per read.
# The script takes two arguments. First, a bamfile, second an output table.
# If a named output is lacking, it will create a table with the bam base file name appended by _matches-per-read.tab

# Laura Dijkhuizen Utrecht University june 2021

# check input
if   [[ $# -eq 0 ]]
then echo 'usage: matches-in-bam.sh <input.bam> <output.tab>'
     exit 1
elif [[ $# -eq 1 ]]
then echo 'bam file is given, but no output is specified. Will continue with default output file'
elif [[ $# -gt 2 ]]
then echo 'Too many arguments are given to this script, it requires only two'
     echo 'usage: matches-in-bam.sh <input.bam> <output.tab>'
     exit 1
fi

# define in and output as human readable names
bam="${1}"
bambase="${bam%.*}"
output="${2:-$bambase}"_matches-per-read.tab


# convert bam to sam (without header) and put it in a named pipe (and remove pipe if it still exists from an old run)
if [ -e /tmp/filtered_sam ]
then rm /tmp/filtered_sam
fi
mkfifo /tmp/filtered_sam
samtools view "$bam" > /tmp/filtered_sam &

# in a loop, read the samfile line for line
# 1 get query name
# 2 get all numbers for all matches from cigarstring (removing M's)
# 3 replace separators by '+', remove the last '+' and pass through bash calculator
# 4 articulate output line
# 5 remove any variables made within the loop,just making sure these don't survive an iteration somehow.

echo 'reading bamfile, this may take a while'
if	[ ! -f $output ]
then	echo -e "readname\ttotalmatches" > "$output"
	while read -r line
	do	readname=$(echo "$line" | cut -f 1)
		matches=( $(echo "$line" | cut -f 6	\
				| grep -oP '(\d+)M'	\
				| tr -d 'M' ) )
		sum=$(echo "${matches[@]/%/+}" | sed 's/\+$//' | bc -l)
		echo -e "$readname\t$sum"
		unset readname matches sum
	done < /tmp/filtered_sam >> "$output"
fi

# remove named pipe
rm /tmp/filtered_sam

# inform user
echo "Output file was saved as $output"

# check for mult-mapped reads
# if so, take the output (without header) and create separate files with
#  - multimappers
#  - unique mappers
#  - one entry for every read, accumulating the hits of any multimappers

echo 'Checking for multimapped reads...'
multimappers=( $(tail -n +2 "$output" |  cut -f 1 | sort | uniq -d  ) )
if	[[ "${#multimappers[@]}" -gt 0 ]]
then	echo "${#multimappers[@]} multimappers were found, will output:"

	echo " - a list of multimappers in ${output%.*}_multimappers.tab"
	echo 'multimapping_count readname' > "${output%.*}"_multimappers.tab
	cut -f 1 "$output" | tail -n +2 | sort | uniq -d --count >> "${output%.*}"_multimappers.tab

	echo " - table without multimappers in ${output%.*}_unique-mappers.tab"
	echo -e "readname\ttotalmatches" > "${output%.*}"_unique-mappers.tab
	tail -n +2 "$output" | grep -v -f <(printf "%s\n" "${multimappers[@]}") >> "${output%.*}"_unique-mappers.tab

	echo " - a table with unique readnames and accumulated multimappers in ${output%.*}_summed-mappers.tab"

	#this is sloppy, but it works if you have patience.
	for m in "${multimappers[@]}"
	do  cp "${output%.*}"_unique-mappers.tab "${output%.*}"_summed-mappers.tab
	    sum=$(tail -n +2 "$output" | grep "$m" | cut -f 2 | tr '\n' '+' | sed 's/\+$/\n/' | bc -l)
	    echo -e "$m\t$sum" >> "${output%.*}"_summed-mappers.tab
	done

else	echo 'no multimappers found, exiting now'
	exit 0
fi
```
