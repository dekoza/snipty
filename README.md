# snipty

Snipty is a handy package manager tool that allows to organize snippets in the similar way how pip is 
handling python packages and ansible-galaxy is handling roles. The motivation behind snipty is to make 
snippets DRY again. There is lots of good code to small to be legitimate packages on their own that you 
end up copy over and over again to your codebase. Snipty wants to:

* automate the process of downloading them,
* create a `snippets.txt` file to explicitly enumerate your snippet dependencies in the code,
* allow easy tracking of snippets changes.

## Quickstart

### Single snippet installation

Install a single snippet into current directory project using providing path/name as destination:

    $ snipty install https://ghostbin.com/paste/egbue helpers/example_1
    ✔️ Snippet helpers/example_1 installed from https://ghostbin.com/paste/egbue
    
    $ ls helpers
    __init__.py  example_1.py
    
Note that your snippet will have automatically prepended `.py` extension and `__init__.py` files will be created
on all subdirectories up to the project root path, so your snippet will be importable from python.


### Handling requirements file

Snipty remembers what was installed (and where), by dumping some JSON into `.snipty` in your project 
root (current directory).


Create a `snippets.txt` files with snippets requirements:

    $ snipty freeze > snippets.txt
    
    $ cat snippets.txt
    helpers/example_1 from https://ghostbin.com/paste/egbue

Install snippets from `snippets.txt` file:

    $ snipty install -r snipty.txt
    $ Snippet 'helpers/example_1' has been already installed.

### Snippets with multiple files inside

Some snippet sites like gist allows to define multiple files under single URL. Snipty handles this by creating 
a directory (rather than a `.py` module file) and places all snippet files under this directory.

Install snippets that have multiple files inside:

    $ snipty install https://gist.github.com/cypreess/6670a99b2c1cd7b52b24057f68a1debd helpers/django
    ✔️ Snippet helpers/django installed from https://gist.github.com/cypreess/6670a99b2c1cd7b52b24057f68a1debd
    
    $ ls helpers/django
    __init__.py  left_pad.py  middleware.py


### Deleting and moving snippets

Once snippet is installed it cannot be installed again to different location as snipty will warn you about 
possible code duplication.

    $ snipty install https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5 snippets/a
    ✔️ Snippet snippets/a installed from https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5
    
    $ snipty install https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5 snippets/b
    Error: Snippet 'snippets/a' has been already from the same source https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5.

To move snippet to other location simply delete the snippet from the previous location and reinstall snippet to 
another one (snipty will automatically detect this):

    $ snipty install https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5 snippets/a
    ✔️ Snippet snippets/a installed from https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5
    
    $ rm snippets/a.py
    
    $ snipty install https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5 snippets/b
    ✔ ️ Snippet snippets/b installed from https://gist.github.com/cypreess/bc7b4d7c46b9a4cf1411c87b5c65d3d5


## Helpful environment variables:

* `SNIPTY_PYTHON` - python interpreter that snipty should use to run itself
* `SNIPTY_ROOT_PATH` - default snipty behaviour is to treat all relative paths according to current directory; 
it can be ovveriden using this path or `-p`/`--path` argument
* `SNIPTY_TMP` - ovveride temporary directory for downloading snippets


## Help needed

Pull requests are very welcome.

- More downloaders needed
- Tests needed!
- Docs needed
