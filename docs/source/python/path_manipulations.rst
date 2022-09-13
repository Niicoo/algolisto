Path Manipulations
==================


Get path of the current script
##############################

.. code-block:: python

    import pathlib
    script_path = pathlib.Path(__file__)


Get path of the python interpreter
##################################

.. code-block:: python

    import sys
    interp_path = sys.executable


Get Current Working Directory
#############################

.. code-block:: python

    import os
    interp_path = os.getcwd()


Change Working Directory
########################

.. code-block:: python

    import os
    interp_path = os.chdir("/new/working/directory")


Get absolute path
#################

.. code-block:: python

    import pathlib
    rel_p = pathlib.Path("./relative/path")
    abs_p = rel_p.resolve()

.. important::
    Current working directory is used to resolve the absolute path. This will work even if the path does not exist.


Get parent path
###############

.. code-block:: python

    import pathlib
    p = pathlib.Path("./path/file.txt")
    p.parent # PosixPath('./path')
    p.parent.parent # PosixPath('.')

    # If you want to go further you need to transform to the absolute path first
    # Example if the working directory is "/home/username/work"
    p.resolve().parent.parent.parent # PosixPath('/home/username/work')


Join path
#########

.. code-block:: python

    import pathlib
    p = pathlib.Path("yo/hey")
    p = pathlib.Path(p, 'dir', 'hehe/hihi', 'file_name')
    # p = PosixPath('yo/hey/dir/hehe/hihi/file_name')


Check if the path is an existing file/folder
############################################

.. code-block:: python

    import pathlib
    pathlib.Path("/path/to/file.txt").is_file() # boolean
    pathlib.Path("/path/to/file.txt").is_dir() # boolean


Create a directory even if it already exists
############################################

.. code-block:: python

    import os
    os.makedirs("hi/how/are/you", exist_ok=True)


Get extension
#############

.. code-block:: python

    import pathlib
    ext = pathlib.Path("/path/to/file.TXT").suffix.lower() # ext = ".txt"


Get string filepath from a pathlib.Path object
##############################################

.. code-block:: python

    import pathlib
    path_str = pathlib.Path("/path/to/file.txt").as_posix() # "/path/to/file.txt"


Iterate over all files in a folder
##################################

.. code-block:: python

    import os
    import pathlib

    # Not recursive: Only the files contain in the specified folder
    for filename in os.listdir('.'):
        p = pathlib.Path('.', filename)
        if p.is_file():
            print(p)

    # Recursively: All files including sub-directories
    for root, dirs, files in os.walk('.'):
        for filename in files:
            # Filepath
            p = pathlib.Path(root, filename)
            print(p)


Resolve path using pattern expansion ("?", "*", "[]", "**") using glob
######################################################################

.. code-block:: python

    import glob

    list_filepaths = glob.glob("path_?/with/**/patterns/*.txt")


Example - Typical use case
##########################

The user inputs a path:

- If the path has an extension: we create the directory if it doesn't exist and the file
- If the path don't have extension, 2 options:

    - EITHER: we consider the path as a file path without extension: we add the extension ourself
    - OR: we consider the path as a directory path: we add the filename and extension ourself

.. code-block:: python

    import argparse
    import pathlib
    import os

    parser = argparse.ArgumentParser(
        description="Description of the program",
        formatter_class=argparse.RawTextHelpFormatter,
    )
    # Positional string Argument
    parser.add_argument("path", type=str,
        help="Input path")

    args = parser.parse_args()

    path = args.path

    filepath = None
    suffix = pathlib.Path(path).suffix.lower()
    if suffix == ".txt":
        # The input path is a correct filepath with the expected extension
        filepath = pathlib.Path(path)
    elif suffix != "":
        raise ValueError("Invalid file extension")
    else:
        # If we consider the path as a directory
        # We add the file name and its extension
        filepath = pathlib.Path(path, "output.txt")
        # If we consider the path as a filename without extension
        # We add the file name and its extension
        filepath = pathlib.Path(path + ".txt")
    
    # Creating the directory if it doesn't exist
    os.makedirs(filepath.parent.as_posix(), exists_ok=True)


------------------------------------------------------------

**Sources**:

- Glob: https://docs.python.org/3/library/glob.html

