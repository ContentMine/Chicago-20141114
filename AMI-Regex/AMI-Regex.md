# AMI-Regex exercise #

## Overview ##

Regular expressions (regex, regexp) are a very widely used and common way of searching text. A regex describes a set of 
possible textual matches and  AMI-regex apllies one or more to the documents/ that are to be searched. 

Regexes can be very simple, but also become very complex and have a reputation for difficulty. AMI-regex has been 
designed to cut down down the complexity and chance of error. This is partly because symbols are used which take 
time to learn. There are now lots of online tutorials and we hope the community will contribute links. 
However in this exercise we want non-experts to learn a few basic rules only.

## INSTALLATION - BRAND NEW UPDATES!!!

(from inside your VM) Download & install the very latest package:

* uninstall the broken stuff (the password is password):
```
sudo apt-get remove xhtml2stm
```
* download the new stuff that works:
```
wget https://bitbucket.org/petermr/ami-core/downloads/ami-core-2.deb
```
* install with 
```
sudo dpkg -i ami-core-2.deb
```

and clone some data files to test with:

```
sudo apt-get install mercurial
```

then...

```
hg clone https://bitbucket.org/petermr/ami-core/
```

then...

```
cd ami-core
```

AMI-regex reads the regexes from a file; we have some prepared some already so you can practice. In your ami-core distrib you will find fields

```
regex/
   which contains
       genbank.xml
       general.xml
       metadata.xml
       phylotree.xml
       pdb.xml
```       

there's also a copy of the "lion" paper in
```
       ./docs/regex/1471-2148-14-70.xml
```
       
## Running the plugin ###

To run the regex plugin type:
```
      ami-regex 
```

from the commandline which should bring up a help output. To run a complete job you need to specify the input file (-i), output
directory (-o) and the regex file to use (-g). Run the typical command:

```
ami-regex -i docs/regex/1471-2148-14-70.xml -g regex/general.xml 
```

The output should end up in 
```
target/1471-2148-14-70.xml/results.xml
```
To view this open it in any modern browser which may also highlight and indent XML structure.

By varying -i, and -g you should be able to run different regexes over different files.

## Regex syntax ##

This is deliberately kept simple.

To search for the word "Panthera" the regex is simply:
```
Panthera
```
(Spaces may matter). However if we want it to be a complete word we write:
```
\WPanthera\W
```

To search for both "lion" or lions" we express this as 
"lion" with an optional "s". The syntax for this is
```
lions?
```
The query after the character indicates optional.

## format of AMI-regex files ##

AMI-regex is created to allow for many options, but without the unreadability of normal regexes. It also allows 
for labelling the captured characters ("capture group"). Here's a simple example ("general.xml")

```
<compoundRegex title="regex">
	<regex weight="1.0" url="http://contentmine.org/genbank" fields="genbank g1 g2" case="required"
	   title="genbank">\W(([A-Z]{2}\d{6})|([A-Z]\d{5}))\W</regex>
	<regex weight="1.0" fields="lion" case="required"
	   title="lion">\W(Panthera)\W</regex>
	<regex weight="1.0" fields="abbrev value" case="required"
	   title="abbrev">\W([^:]*):([^;]*);</regex>
</compoundRegex>
```
This describes THREE regexes which are all applied. The first selects Genbank identifiers (we'll explain later), 
the second is Panthera (the "\W") means that it has to be a Whole Word. The latter is more ambitious and selects 
abbreviations and their meaning in this paper.

The following are our own enhancements:

* The ''weight'' is how much weight we give to the result
* fields are how we want the results to be labelled. Thus the result of a genbank match would be labelled 
   something like ''genbank=AB123456''
* case indicates whether we want exact case-matching
* title is a label to select the regex
* url gives a link to external documentation

