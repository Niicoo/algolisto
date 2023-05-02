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

## SOURCES

I'm doing my best to quotes the sources I used to create the content of this website. The sources are written at the bottom of each page.
Since the emergence of large language models such as chatGPT by openai, I'm using it a lot to help me to write the pages more quickly, but I'm making sure the codes provided by these models are working.

## TODO  
- Feature: Create a sphinx Extension to automatically download README.md from a github public project link and render it in a page.


## ERRORS
- quotes are ugly rendered, find out why


## ThirdParty / Extensions
- extension to embed PDF: **sphinxcontrib-pdfembed** https://github.com/SuperKogito/sphinxcontrib-pdfembed
- extension to add tab feature: **sphinx-tabs** https://github.com/executablebooks/sphinx-tabs  
- Theme: **Read the Docs** https://sphinx-rtd-theme.readthedocs.io/en/stable/index.html
