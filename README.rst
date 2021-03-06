================================================================================
pyexcel-xlsxw - Let you focus on data, instead of xlsx format
================================================================================

.. image:: https://api.travis-ci.org/pyexcel/pyexcel-xlsxw.png
    :target: http://travis-ci.org/pyexcel/pyexcel-xlsxw

.. image:: https://codecov.io/github/pyexcel/pyexcel-xlsxw/coverage.png
    :target: https://codecov.io/github/pyexcel/pyexcel-xlsxw

**pyexcel-xlsxw** is a tiny wrapper library to write data in xlsx and xlsm fromat using xlsxwriter. You are likely to use it with `pyexcel <https://github.com/pyexcel/pyexcel>`__.

Known constraints
==================

Fonts, colors and charts are not supported.

Installation
================================================================================

You can install it via pip:

.. code-block:: bash

    $ pip install pyexcel-xlsxw


or clone it and install it:

.. code-block:: bash

    $ git clone http://github.com/pyexcel/pyexcel-xlsxw.git
    $ cd pyexcel-xlsxw
    $ python setup.py install

Usage
================================================================================

As a standalone library
--------------------------------------------------------------------------------

.. testcode::
   :hide:

    >>> import os
    >>> import sys
    >>> if sys.version_info[0] < 3:
    ...     from StringIO import StringIO
    ... else:
    ...     from io import BytesIO as StringIO
    >>> PY2 = sys.version_info[0] == 2
    >>> if PY2 and sys.version_info[1] < 7:
    ...      from ordereddict import OrderedDict
    ... else:
    ...     from collections import OrderedDict


Write to an xlsx file
********************************************************************************



Here's the sample code to write a dictionary to an xlsx file:

.. code-block:: python

    >>> from pyexcel_xlsxw import save_data
    >>> data = OrderedDict() # from collections import OrderedDict
    >>> data.update({"Sheet 1": [[1, 2, 3], [4, 5, 6]]})
    >>> data.update({"Sheet 2": [["row 1", "row 2", "row 3"]]})
    >>> save_data("your_file.xlsx", data)



Here's the sample code to help you read the data back. You will need to install pyexcel-xls or pyexcel-xlsx.

.. code-block:: python

    >>> from pyexcel_io import get_data
    >>> data = get_data("your_file.xlsx")
    >>> import json
    >>> print(json.dumps(data))
    {"Sheet 1": [[1, 2, 3], [4, 5, 6]], "Sheet 2": [["row 1", "row 2", "row 3"]]}


Write an xlsx to memory
********************************************************************************

Here's the sample code to write a dictionary to an xlsx file:

.. code-block:: python

    >>> from pyexcel_xlsxw import save_data
    >>> data = OrderedDict()
    >>> data.update({"Sheet 1": [[1, 2, 3], [4, 5, 6]]})
    >>> data.update({"Sheet 2": [[7, 8, 9], [10, 11, 12]]})
    >>> io = StringIO()
    >>> save_data(io, data)
    >>> # do something with the io
    >>> # In reality, you might give it to your http response
    >>> # object for downloading





Here's the sample code to help you read the data back. You will need to install pyexcel-xls or pyexcel-xlsx.

.. code-block:: python

    >>> # This is just an illustration
    >>> # In reality, you might deal with xlsx file upload
    >>> # where you will read from requests.FILES['YOUR_XLSX_FILE']
    >>> data = get_data(io, 'xlsx')
    >>> print(json.dumps(data))
    {"Sheet 1": [[1, 2, 3], [4, 5, 6]], "Sheet 2": [[7, 8, 9], [10, 11, 12]]}



As a pyexcel plugin
--------------------------------------------------------------------------------

No longer, explicit import is needed since pyexcel version 0.2.2. Instead,
this library is auto-loaded. So if you want to read data in xlsx format,
installing it is enough.


Let's assume we have data as the following.

.. code-block:: python

    >>> import pyexcel as pe
    >>> sheet = pe.get_book(file_name="your_file.xlsx")
    >>> sheet
    Sheet 1:
    +---+---+---+
    | 1 | 2 | 3 |
    +---+---+---+
    | 4 | 5 | 6 |
    +---+---+---+
    Sheet 2:
    +-------+-------+-------+
    | row 1 | row 2 | row 3 |
    +-------+-------+-------+


Writing to an xlsx file
********************************************************************************

Here is the sample code:

.. code-block:: python

    >>> sheet.save_as("another_file.xlsx")


Writing to a StringIO instance
********************************************************************************

You need to pass a StringIO instance to Writer:

.. code-block:: python

    >>> data = [
    ...     [1, 2, 3],
    ...     [4, 5, 6]
    ... ]
    >>> io = StringIO()
    >>> sheet = pe.Sheet(data)
    >>> io = sheet.save_to_memory("xlsx", io)
    >>> # then do something with io
    >>> # In reality, you might give it to your http response
    >>> # object for downloading


License
================================================================================

New BSD License

Developer guide
==================

Development steps for code changes

#. git clone https://github.com/pyexcel/pyexcel-xlsxw.git
#. cd pyexcel-xlsxw

Upgrade your setup tools and pip. They are needed for development and testing only:

#. pip install --upgrade setuptools "pip==7.1"

Then install relevant development requirements:

#. pip install -r rnd_requirements.txt # if such a file exists
#. pip install -r requirements.txt
#. pip install -r tests/requirements.txt


In order to update test environment, and documentation, additional setps are
required:

#. pip install moban
#. git clone https://github.com/pyexcel/pyexcel-commons.git commons
#. make your changes in `.moban.d` directory, then issue command `moban`

What is rnd_requirements.txt
-------------------------------

Usually, it is created when a dependent library is not released. Once the dependecy is installed(will be released), the future version of the dependency in the requirements.txt will be valid.

What is pyexcel-commons
---------------------------------

Many information that are shared across pyexcel projects, such as: this developer guide, license info, etc. are stored in `pyexcel-commons` project.

What is .moban.d
---------------------------------

`.moban.d` stores the specific meta data for the library.

How to test your contribution
------------------------------

Although `nose` and `doctest` are both used in code testing, it is adviable that unit tests are put in tests. `doctest` is incorporated only to make sure the code examples in documentation remain valid across different development releases.

On Linux/Unix systems, please launch your tests like this::

    $ make test

On Windows systems, please issue this command::

    > test.bat


.. testcode::
   :hide:

   >>> import os
   >>> os.unlink("your_file.xlsx")
   >>> os.unlink("another_file.xlsx")
