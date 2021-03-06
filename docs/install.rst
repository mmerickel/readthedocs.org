.. _installing-read-the-docs:

Installation
=============

Here is a step by step plan on how to install Read the Docs.
It will get you to a point of having a local running instance.

First, obtain Python_ and virtualenv_ if you do not already have them. Using a
virtual environment will make the installation easier, and will help to avoid
clutter in your system-wide libraries. You will also need Git_ in order to
clone the repository.


.. _Python: http://www.python.org/
.. _virtualenv: http://pypi.python.org/pypi/virtualenv
.. _Git: http://git-scm.com/

Once you have these, create a virtual environment somewhere on your disk, then
activate it::

    virtualenv rtd
    cd rtd
    source bin/activate


You will need to verify that your pip version is higher than 1.5 you can do this as such::

    pip --version

If this is not the case please update your pip version before continuing::

    pip install --upgrade pip

Create a folder in here, and clone the repository::

    mkdir checkouts
    cd checkouts
    git clone https://github.com/rtfd/readthedocs.org.git

Next, install the dependencies using ``pip`` (included with virtualenv_)::

    cd readthedocs.org
    pip install -r requirements.txt

.. note::

    If you are having trouble on OS X Mavericks (or possibly other versions of
    OS X) with building ``lxml``, you probably might need to use Homebrew_
    to ``brew install libxml2``, and invoke the install with::

        CFLAGS=-I/usr/local/opt/libxml2/include/libxml2 \
        LDFLAGS=-L/usr/local/opt/libxml2/lib \
        pip install -r requirements.txt

.. note::

    Linux users may find they need to install a few additional packages
    in order to successfully execute ``pip-install -r requirements.txt``.
    For example, a clean install of Ubuntu 14.04 LTS will require the
    following packages::

        sudo apt-get install build-essential
        sudo apt-get install python-dev
        sudo apt-get install libxml2-dev libxslt1-dev zlib1g-dev

    Users of other Linux distributions may need to install the equivalent
    packages, depending on their system configuration.

.. _Homebrew: http://brew.sh/

This may take a while, so go grab a beverage. When it's done, build your
database::

    cd readthedocs
    ./manage.py syncdb

This will prompt you to create a superuser account for Django. Do that. Then::

    ./manage.py migrate

Go ahead and load in a couple users and a test projects::

    ./manage.py loaddata test_data

.. note::

    If you do not opt to install test data, you'll need to create an account for
    API use and set ``SLUMBER_USERNAME`` and ``SLUMBER_PASSWORD`` in order for
    everything to work properly.

Finally, you're ready to start the webserver::

    ./manage.py runserver

Visit http://127.0.0.1:8000/ in your browser to see how it looks; you can use
the admin interface via http://127.0.0.1:8000/admin (logging in with the
superuser account you just created).

While the webserver is running, you can build documentation for the latest version of
a project called 'pip' with the ``update_repos`` command.  You can replace 'pip'
with the name of any added project::

   ./manage.py update_repos pip

What's available
----------------

After registering with the site (or creating yourself a superuser account),
you will be able to log in and view the `dashboard <http://readthedocs.org/dashboard/>`_.

From the dashboard you can import your existing
docs provided that they are in a git or mercurial repo.


Creating new Docs
^^^^^^^^^^^^^^^^^

One of the goals of `readthedocs.org <http://readthedocs.org>`_ is to make it
easy for any open source developer to get high quality hosted docs with great
visibility!  We provide a simple editor and two sample pages whenever
a new project is created.  From there its up to you to fill in the gaps - we'll
build the docs, give you access to history on every revision of your files,
and we plan on adding more features in the weeks and months to come.


Importing existing docs
^^^^^^^^^^^^^^^^^^^^^^^

The other side of `readthedocs.org <http://readthedocs.org>`_ is hosting the
docs you've already built.  Simply provide us with the clone url to your repo,
we'll pull your code, extract your docs, and build them!  We make available
a post-commit webhook that can be configured to update the docs on our site
whenever you commit to your repo, effectively letting you 'set it and forget it'.
