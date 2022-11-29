# Algolisto

Code to generate https://algolisto.org

It uses sphinx documentation generator (https://www.sphinx-doc.org/en/master/) to render the HTML. 

## Usage
0. Configure the python environement:
```
conda env create -f environment.yml
conda activate sphinx-blog
```  
1. Render as HTML
```
sphinx-build -b html docs/source/ docs/build/html
```  

## Other commands
`make clean` to delete the build directory.  

## TODO  
- Feature: Create a sphinx Extension to automatically download README.md from a github public project link and render it in a page.


## ERRORS
- quotes are ugly rendered, find out why


## ThirdParty / Extensions
- extension to embed PDF: **sphinxcontrib-pdfembed** https://github.com/SuperKogito/sphinxcontrib-pdfembed  
- Theme: **Read the Docs** https://sphinx-rtd-theme.readthedocs.io/en/stable/index.html
