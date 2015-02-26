# whoosh-tutorial
A first look at how to use whoosh in a project. A very simple tutorial.

# Setup
Follow the steps below to get setup.
```bash
$ git clone https://github.com/chadgh/whoosh-tutorial.git
$ cd whoosh-tutorial
$ virtualenv env  # make sure the version of Python is 3.3 or greater.
$ source env/bin/activate
$ pip install -r requirements.txt
$ cp search_starter.py search.py
```
**Note**: You might need to `deactivate` and re-source (`source env/bin/activate`) in order for the newly installed ipython and things to work correctly.

You are now ready to start editing the `search.py` file to implement a whoosh search solution for the content in the `pdfs` and `text` directories.

# Data
The `pdfs` and `text` directories contain similarly named files from each other. The text files in `text` are the plain text versions of the pdf files in `pdfs`.

# Goal
The goal of this tutorial is to build a searcher, using whoosh, to search the files for user input keywords and return the matching files. Execution of your solution might look something like this:

```bash
$ ./search.py "art city days"
11 files matched:
  * pdfs/060314CCMinutesApproved.pdf
  * pdfs/061714CCMinutesApproved1.pdf
  * pdfs/021814CCMinutesApproved.pdf
  * pdfs/050614CCMinutesApproved.pdf
  * pdfs/012114CCMinutes.pdf
  * pdfs/091614CCMinutesApproved.pdf
  * pdfs/031914_CCMinutesApproved.pdf
  * pdfs/081914CCMinutesApproved.pdf
  * pdfs/041514CCMinutesApproved.pdf
  * pdfs/070114CCMinutesApproved1.pdf
```
