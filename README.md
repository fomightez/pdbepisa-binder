# pdbepisa-binder
Analysis of PDBePISA-related data via active Jupyter sessions provided via MyBinder.org. Adapt the demonstrations to analyze your favorite structures.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/fomightez/pdbepisa-binder/main?urlpath=%2Fnotebooks%2Findex.ipynb)


*tl;dr:*  
Click any `launch binder` badge on this page to use the demonstrations inside your browser.

------


***pdbepisa-binder:  Jupyter notebook environment for analysis of PDBePISA-related data.***

A launchable, working Jupyter-based environment that has a collection of demonstrations of analysis of PDBePISA-related data served via MyBinder.org.

You can also easily adapt the demonstrations to analyze your favorite structures.

Meant to be self-contained and ready-to-go. No installations or copying of notebooks is necessary if `launch binder` is clicked. Everything will just work. Of course, static versions of the notebooks can also be used. I recommend rendering the static versions by placing the URLs into the [nbviewer](https://nbviewer.jupyter.org/). The views provided by [nbviewer](https://nbviewer.jupyter.org/) look best and Github's rendering often times out (your mileage may vary).

[PDBePISA](https://www.ebi.ac.uk/pdbe/pisa/) itself is accessible online [here](https://www.ebi.ac.uk/pdbe/pisa/).



Usage
-----

This repository is set up to allow analysis of [PDBePISA](https://www.ebi.ac.uk/pdbe/pisa/)-related data after pressing the `launch binder` button above or below. The target use case is where you want to be thorough in your analyses with the add of compuatation or analyze multiple structures. You shouldn't need to install anything.

In the notebooks that can be launched, I have added some examples illustrating how to get data and process ir easily with Python and convert to other forms. Alternatively, the notebooks with most of resources can be viewed statically and [nbviewer](https://nbviewer.jupyter.org/) is recommended for that as discussed above.

## Attributions

Users of [PDBePISA](https://www.ebi.ac.uk/pdbe/pisa/)-sourced data should probably cite the following, as directed [here](https://www.ebi.ac.uk/pdbe/pisa/picite.html):

- [Inference of macromolecular assemblies from crystalline state. Krissinel E, Henrick K. J Mol Biol. 2007 Sep 21;372(3):774-97. doi: 10.1016/j.jmb.2007.05.022. Epub 2007 May 13. PMID: 17681537](https://pubmed.ncbi.nlm.nih.gov/17681537)

***Clarifying Software Attribution: I, Wayne, am not involved with PDBePISA in any way. I simply set up this repository to make analysis of the data easier without installation headaches. See the links above and below for the materials by those who established and maintain PDBePISA, and the related site jsPISA.***


I, Wayne, did share Jupyter/Python-based utilities for use with the data available from PDBePISA; these are available [here](https://github.com/fomightez/structurework/tree/master/pdbepisa-utilities) and utilized in the notebooks in this repository to process data and allow easily converting the results to other forms.



## Related items by others

There is also jsPISA, which is [supposedly](https://pubmed.ncbi.nlm.nih.gov/25908787/) an improved user interface; however, I don't see a way to use access as an API, several PDB entries I put in gave the error that they did not exist, and when I tried with [4fgf that shows a very informative interface table and PDBePISA](http://www.ebi.ac.uk/pdbe/pisa/cgi-bin/piserver?qi=4fgf), I was not able to see equivalent at jsPISA. jsPISA is maintained by CCP4 [here](http://www.ccp4.ac.uk/pisa).

[Louis](https://www.biostars.org/u/9020/) has [a script](https://gist.github.com/lmmx/91515d38a1fc0644268f#file-getxml-py) that gets HTML (or is it XML?) for a lot of PDB indentifiers. It allows for interrupted/resumed download.


## Related items by me

- My [pdbepisa-utilities sub-repo](https://github.com/fomightez/structurework/tree/master/pdbsum-utilities) for the associated scripts.

- My [pdbsum-utilities sub-repo](https://github.com/fomightez/structurework/tree/master/pdbsum-utilities) has a number of scripts, although I note the interface handling ones only deal with chains of protein-protein interfaces. These scripts are demonstrated in sessions that can be launched by pressing the `launch binder` button at my repo [pdbsum-binder](https://github.com/fomightez/pdbsum-binder).

- My repo [pdbsum-binder](https://github.com/fomightez/pdbsum-binder) demonstrates scripts from my [pdbsum-utilities sub-repo](https://github.com/fomightez/structurework/tree/master/pdbsum-utilities) that enable handling data from the [PDBsum](http://www.ebi.ac.uk/thornton-srv/databases/cgi-bin/pdbsum/GetPage.pl?pdbcode=index.html) with Jupyter/Python. Importantly, data from that site will only summarize interface surface area between protein chains of a structure. It does detail protein and nucleic acid residue-residue contacts but only graphically, and so I haven't found/developed a way to extract the data from there into Pandas dataframes yet.

- See [here](https://github.com/fomightez/structurework#related-binderized-utilities) for a listing of resources in a similar vein yet targeted to macromolecular structure data. In particular, see [cl_demo-binder](https://github.com/fomightez/cl_demo-binder) for the companion set to this one.





## Technical notes

This repository is set up to make use of the binder service offered by [MyBinder.org](https://mybinder.org/). See their site for more information about Binder.

I borrrowed the 'warning' highlight/introductory text about notebooks at the top of the included notebook from Tim Sherratt's notebook [here](https://github.com/GLAM-Workbench/te-papa-api/blob/master/Exploring-the-Te-Papa-collection-API.ipynb).

Click `launch binder` below to start using the demonstrations.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/fomightez/pdbepisa-binder/main?urlpath=%2Fnotebooks%2Findex.ipynb)
