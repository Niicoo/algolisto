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
- Page: timer in python
- Page: dataclasses / pydantic / attrs
- Page: write csv / excel in python
- Page: how to add a button with action in a figure
- Page: GIT basic commands
- Page: __hash__, __eq__, __repr__, __str__, etc...
- Page: Playing with grub
    - /etc/default/grub to ignore a partition in the grub menu # uuid are used
    - /etc/grub.d/40_custom to link another grub menu # uuid are used, remove /boot if dedicaded boot partition
    - efibootmgb to add/remove/change boot order and entries
    - efibootmgr -c -d /dev/nvme1n1 -p 8 -L Ubuntu -l "\EFI\ubuntu\shimx64.efi"
    - efibootmgr -c -d /dev/nvme0n1 -p 1 -L Mint -l "\EFI\ubuntu\shimx64.efi"
    - blkid to get uuid and partuuid 
- Append to linux/kernel_update: List installed kernels + Uninstall oldest kernels
- Append to python/divers: different way to execute an OS command from python
- Feature: Create a sphinx Extension to automatically download README.md from a github public project link and render it in a page.


## ERRORS
- quotes are ugly rendered, find out why


## ThirdParty / Extensions
- extension to embed PDF: **sphinxcontrib-pdfembed** https://github.com/SuperKogito/sphinxcontrib-pdfembed  
- Theme: **Read the Docs** https://sphinx-rtd-theme.readthedocs.io/en/stable/index.html
