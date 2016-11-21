# python-_-mutability-of-aminoacids-in-blosum62-matrix
#code to find the most and least mutable aminoacids in blosum 62 matrix
#import modules needed 
import pandas as pd
import sys
import os
import numpy as np
import re

print "Reading BLOSUM62"

blosum = sys.argv[1]

# newlist(variable list/array of blosum62 matrix) is created by appending the lines from blosum62.txt file(append only 20aminoacids)
# read the lines, split the lines with \n,deselect the lines start with # sign(text information in blosum62.txt)

i=1
newlist = []
with open(blosum) as infile:
#	print i
	i += 1
	line = infile.read()
	lines = line.split('\n')

#	print len(line)	
	for text in lines:
		if not text.startswith("#"):
			mat = re.split(' +', text)
#			print mat[1:]

    			newlist.append(mat[1:21])

#		print data
#	print newlist

#convert the newlist to pandas dataframe df.extract all diagonal values to look at highest and lowest blosum scores.

df = pd.DataFrame(newlist[1:21], columns=newlist[0], dtype='int32')
df.index = newlist[0]
df.diag = pd.Series(data=np.diag(df), index=df.index)

#print df.diag

# get top3 maximum values in the matrix
print "\na)The least mutable aminoacids and their scores are:"
print df.diag.sort_values(ascending=False).head(3)

#get top5 least values in the matrix
print "\nb)The most mutable aminoacids and their scores are:"
print df.diag.sort_values().head(5)


# get the highest values in the matrix (except diagonal values).create another datarframe df2 with diagonal values 0 and look for highest,lowest values
df2 = df

np.fill_diagonal(df2.values, 0)

print "\nc)The most frequent mutations involving 2 different amino acid types:"

print df2[df2==df2.max().max()].dropna(thresh=1, axis=1).dropna(thresh=1)

# get the least values in the matrix (except diagnoal values)
print "\nd)The least frequent mutations involving 2 different amino acids types:"

print df[df==df.min().min()].dropna(thresh=1, axis=1).dropna(thresh=1)

for index, elem in enumerate(df2):
	print(index, elem)
