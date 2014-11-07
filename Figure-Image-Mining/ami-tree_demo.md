Extracting phylogenetic tree data from images with ami-tree
============================

 * You will be provided (electronically) with some freely available articles from a journal called *International Journal of Systematic & Evolutionary Microbiology* (IJSEM). Although technical you should be able to carry out this exercise without any deep scientific knowledge.
 * Work on your own or in pairs.
 * The aim of this exercise is to extract computationally-reusable information from published scientific figures, in this case; phylogenies. The topology, branch lengths & tip labels will all be extracted from the image(s) so they can reused & re-analysed. 

 Background - Phylogenetic Trees
 --------

Phylogenetic tree figures are widely used in scientific papers to demonstrate the evolutionary relationships between the taxa studied. Further detail about these trees can be found on [Wikipedia](http://en.wikipedia.org/wiki/Phylogenetic_tree). 

![example tree from Wikipedia](http://upload.wikimedia.org/wikipedia/commons/thumb/3/3f/NewickExample.svg/500px-NewickExample.svg.png)

A typical tree may have a number of nodes, branches (edges) & tip (leaf) labels. Branches may or may not have specific lengths which if present can be useful information to capture & reuse.

For real examples of published phylogenetic tree figures (and the vast variety of ways in which they can be depcited) see the PLOS ONE phylogeny figure collection on Flickr: www.bit.ly/PLOStrees


## Getting Started

 
 Inside your ContentMine VM, open up a terminal (e.g. right click, select Terminal).
 Type or copy-in & execute this script to make sure everything is up to date:
 ```
 sh /home/workshop/ebi_workshop_20141006/setup/update_contentmine.sh
 ```
 
 Run *ami-tree* with no arguments to see what command-line arguments it accepts, you should see something like:
 
```
$ ami-tree
Tree: 
Extracts trees (e.g. phylogenetic).
Universal options ('-f' is short for '--foo', etc.):
    -i  --input  inputSpec
                 mandatory: filename, directoryName, url, or (coming) identifier (e.g. PMID:12345678)
    -o  --output  outputSpec
                 mandatory: filename, directoryName
    -r  --recursive
                 recurse through directories
    -x  --extensions ext1 [ext2 ...]
                 mandatory if input directory: file extensions (htm, pdf, etc.)

```

Navigate to *~/ebi_workshop_20141006/sessions/4_AMI/phylo_files/phylo-figure-images* and you'll find 44 bitmap figure images (as .png) of phylogenetic trees taken from freely available IJSEM papers. Each file is named after the DOI of the paper from which it came from. The last three digits of the file name indicate which figure it is in the paper, figure 1 = 000, figure 2 = 001 etc...

Try running ami-tree on the first image file:

![image of first tree](https://raw.githubusercontent.com/ContentMine/ebi_workshop_20141006/master/sessions/4_AMI/phylo_files/phylo-figure-images/ijs.0.014126-0-000.pbm.png)

```
ami-tree -i /home/workshop/ebi_workshop_20141006/sessions/4_AMI/phylo_files/phylo-figure-images/ijs.0.014126-0-000.pbm.png
```

In the output to terminal you should see the correct [Newick string](http://en.wikipedia.org/wiki/Newick_format) that corresponds to the figure image:

```
1420 [main] DEBUG org.xmlcml.diagrams.phylo.PhyloTreeAnalyzer  - NEWICK: (((585_569:0.58,(562_535:0.5,((((565_188:0.29,(464_223:0.12,460_257:0.11):0.02):0.03,((524_154:0.15,458_119:0.05):0.05,((611_50:0.19,548_15:0.09):0.06,510_84:0.09):0.09):0.06):0.01,((506_362:0.13,551_327:0.19):0.02,488_292:0.12):0.09):0.04,((525_431:0.24,(515_500:0.18,638_466:0.37):0.05):0.04,485_397:0.21):0.03):0.14):0.04):0.06,386_605:0.33):0.25,639_659:0.97):0.03;
```

Paste this Newick string into phylogenetic tree viewing software to visualize the result (we recommend the [Phyfi](http://cgi-www.daimi.au.dk/cgi-chili/phyfi/go) web-app), taking care to ensure you have all of it from the first parenthesis, to the final semi-colon:

```
(((585_569:0.58,(562_535:0.5,((((565_188:0.29,(464_223:0.12,460_257:0.11):0.02):0.03,((524_154:0.15,458_119:0.05):0.05,((611_50:0.19,548_15:0.09):0.06,510_84:0.09):0.09):0.06):0.01,((506_362:0.13,551_327:0.19):0.02,488_292:0.12):0.09):0.04,((525_431:0.24,(515_500:0.18,638_466:0.37):0.05):0.04,485_397:0.21):0.03):0.14):0.04):0.06,386_605:0.33):0.25,639_659:0.97):0.03;
```

You should see something like this:

![tree viz](https://dl.dropboxusercontent.com/u/757135/ami-tree-viz.png)

Is this a correct interpretation of the topology & branch lengths depicted in the original figure image? Remember individual clades can be rotated 180 degrees in visualizations and still be the same (identical) tree.

Ami-tree is still in development. Which images can it correctly interpret, which images can it not do? Where it fails can you determine from the source image why it might be failing to recover the correct topologies?

In some cases the quality of the published figure is just too poor for ami-tree to correctly interpret:

![line break example](https://dl.dropboxusercontent.com/u/757135/linebreak1.png)

## Extraction of tip labels

You may have noticed that ami-tree does not yet return extracted tip-labels from the image - this is still under development. But to show you what can be achieved try using an OCR-engine like tesseract, off-the-shelf, untrained.

Example output of tesseract-ocr against the first image is provided in */home/workshop/ebi_workshop_20141006/sessions/4_AMI/phylo_files/tesseract-output-of-phylofigures*

a common error is that 'l' is interpreted by tesseract as '/' but otherwise the output is fairly accurate.

Try tesseract on another figure image file.

```
tesseract <inputfile> <outputfile>
```

How correct on a per character basis are the Latin names? How correct are the GenBank accession numbers? Which are more reliable?


## Further details about PLUTo: Phylogenetic Literature Unlocking Tools

* The BBSRC-funded grant proposal is [here](http://www.bbsrc.ac.uk/pa/grants/AwardDetails.aspx?FundingReference=BB/K015702/1)

* Conference talk slides <iframe src="//www.slideshare.net/slideshow/embed_code/36236562" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/rossmounce/the-pluto-project-ievobio-2014" title="The PLUTo project @iEvoBio 2014" target="_blank">The PLUTo project @iEvoBio 2014</a> </strong> given by <strong><a href="http://www.slideshare.net/rossmounce" target="_blank">Ross Mounce</a></strong> </div>

* Blog post about the [PLOS ONE phylogeny figure image collection on Flickr](http://rossmounce.co.uk/2014/05/07/plos-one-phylogeny/) created by Ross Mounce as part of the PLUTo project

## Online tree visualisers

- [PHY·FI](http://cgi-www.cs.au.dk/cgi-chili/phyfi/go) is fast and easy to use. It offers full color control, tree manipulation, and several image formats.
- [T-REX](http://www.trex.uqam.ca/) - Tree and reticulogram REConstruction
- [ETE toolkit](http://etetoolkit.org/treeview/) phylogenetic tree view (newick format)
- [Phylodendron](http://iubio.bio.indiana.edu/treeapp/)  Phylogenetic tree drawing

## Further Reading

* Stoltzfus, A., O'Meara, B., Whitacre, J., Mounce, R., Gillespie, E., Kumar, S., Rosauer, D., and Vos, R. 2012. Sharing and re-use of phylogenetic trees (and associated data) to facilitate synthesis. BMC Research Notes 5:574 [DOI: 10.1186/1756-0500-5-574](http://dx.doi.org/10.1186/1756-0500-5-574)
* Drew, B. T., Gazis, R., Cabezas, P., Swithers, K. S., Deng, J., Rodriguez, R., Katz, L. A., Crandall, K. A., Hibbett, D. S., and Soltis, D. E. 2013. Lost branches on the tree of life. PLoS Biology 11:e1001636 [DOI: 10.1371/journal.pbio.1001636](http://dx.doi.org/10.1371/journal.pbio.1001636)
* Magee, A. F., May, M. R., and Moore, B. R. 2014. The dawn of open access to phylogenetic data. arXiv preprint [arXiv:1405.6623](http://arxiv.org/abs/1405.6623)
* Laubach, T., von Haeseler, A., and Lercher, M. 2012. TreeSnatcher plus: capturing phylogenetic trees from images. BMC Bioinformatics 13:110 [DOI: 10.1186/1471-2105-13-110](http://dx.doi.org/10.1186/1471-2105-13-110)
* Hughes, J. 2011. TreeRipper web application: towards a fully automated optical tree recognition software. BMC Bioinformatics 12:178 [DOI: 10.1186/1471-2105-12-178](http://dx.doi.org/10.1186/1471-2105-12-178)
