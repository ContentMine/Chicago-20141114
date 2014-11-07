# AMI-SPECIES

AMI searches for species by a combination of style (italics) and regex. Thus *Panthera leo* is a species; There are very few false positives ("In vivo") except for some Journal names. These can be solved by blacklisting or lookup and we expect Recall > 99 % and Precision (with lookup) 100%. Species entity extraction is among the best illustrations of the power of mining. 

## Running ami-species 

In the VM, open up a terminal and execute 

```
$ ami-species
```

and you should see:

```
$ ami-species
Species: 
Extracts species with either genus or binomial names.
Universal options ('-f' is short for '--foo', etc.):
    -i  --input  inputSpec
                 mandatory: filename, directoryName, url, or (coming) identifier (e.g. PMID:12345678)
    -o  --output  outputSpec
                 mandatory: filename, directoryName
    -r  --recursive
                 recurse through directories
    -e  --extensions ext1 [ext2 ...]
                 mandatory if input directory: file extensions (htm, pdf, etc.)

```

### Example runnning on directory

To run this on a directory we need to specify the extension. Here a list of XML PLoSOne papers are being searched.

```
ami-species -i src/test/resources/org/xmlcml/xhtml2stm/plosone/2013-12-11/xml -e xml
```

The output is (unnecessarily) voluminous and results in a single file ``target/xml.xml/results.xml`` (yes, we'll tidy this later!). This contains XML, which can be displayed as:

```
<results xmlns="http://www.xml-cml.org/xhtml2stm">
	<source name="10.1371/journal.pone.0080763">
		<speciesList>
			<species count="1">Ateles geoffroyi</species>
			<species count="1">Macaca fascicularis</species>
			<species count="2">Cebus apella</species>
			<species count="1">Callicebus moloch</species>
			<species count="1">Hylobates lar</species>
			<species count="1">Cebus capucinus</species>
			<species count="1">Eulemur rufifrons</species>
			<species count="2">Symphonia globulifera</species>
		</speciesList>
	</source>
	<source name="10.1371/journal.pone.0080803">
		<speciesList>
			<species count="1">Mycobacterium tuberculosis</species>
		</speciesList>
	</source>
	<source name="10.1371/journal.pone.0080821">
		<speciesList />
	</source>
	<source name="10.1371/journal.pone.0081425">
		<speciesList />
	</source>
	<source name="10.1371/journal.pone.0081490">
		<speciesList>
			<species count="1">In vivo</species>
			<species count="1">E. coli</species>
			<species count="4">Saccharomyces cerevisiae</species>
		</speciesList>
	</source>
	<source name="10.1371/journal.pone.0081580">
		<speciesList>
			<species count="46">Origanum dayi</species>
			<species count="4">Artemisia judaica</species>
			<species count="1">Oryza sativa</species>
			<species count="1">Jasminum officinale</species>
			<species count="1">Agrostis species</species>
			<species count="92">Artemisia sieberi</species>
			<species count="1">Triticum aestivum</species>
			<species count="1">Artemisia vulgaris</species>
			<species count="1">Echinochloa crus</species>
			<species count="1">Origanum syriacum</species>
			<species count="1">Chlorella vulgaris</species>
			<species count="2">Helianthemum squamatum</species>
			<species count="1">Nicotiana attenuata</species>
			<species count="2">Artemisia annua</species>
			<species count="1">Cymbopogon citratus</species>
		</speciesList>
	</source>
	<source name="10.1371/journal.pone.0082038">
		<speciesList>
			<species count="1">S. cerevisiae</species>
		</speciesList>
	</source>
	<source name="10.1371/journal.pone.0082047">
		<speciesList>
			<species count="40">Streptococcus pneumoniae</species>
		</speciesList>
	</source>
	<source name="10.1371/journal.pone.0082213">
		<speciesList />
	</source>
	<source name="10.1371/journal.pone.0082388">
		<speciesList>
			<species count="1">In vivo</species>
		</speciesList>
	</source>
</results>
```

Yes, we have the famous species ``In vivo`` (which we need to blacklist). Note that we have expanded the names from (say)
``A. sieberi`` to ``Artemisia sieberi`` where the first mention is in full form; and we have also added counts.

## Sectioning

AMI contains a prototype sectioning tool (``-x`` for Xpath). When section names are normalised this can become a very powerful `de facto` standard.
