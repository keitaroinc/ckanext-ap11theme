.. You should enable this project on travis-ci.org and coveralls.io to make
   these badges work. The necessary Travis and Coverage config files have been
   generated for you.

.. image:: https://travis-ci.org/ckan/ckanext-eurovoc.svg?branch=master
    :target: https://travis-ci.org/ckan/ckanext-eurovoc

.. image:: https://coveralls.io/repos/ckan/ckanext-eurovoc/badge.png?branch=master
  :target: https://coveralls.io/r/ckan/ckanext-eurovoc?branch=master

===============
ckanext-eurovoc
===============

Add top-level dcat AP theme categories to CKAN for search and filtering.


------------
Requirements
------------

Compatible with CKAN 2.2 and 2.3.


------------
Installation
------------

.. Add any additional install steps to the list below.
   For example installing any non-Python dependencies or adding any required
   config settings.

To install ckanext-eurovoc:

1. Activate your CKAN virtual environment, for example::

     . /usr/lib/ckan/default/bin/activate

2. Install the ckanext-ap11theme Python package into your virtual environment::

     pip install ckanext-ap11theme

3. Add ``ap11theme`` to the ``ckan.plugins`` setting in your CKAN
   config file (by default the config file is located at
   ``/etc/ckan/default/production.ini``).

4. Restart CKAN. For example if you've deployed CKAN with Apache on Ubuntu::

     sudo service apache2 reload


------------------------
Development Installation
------------------------

To install ckanext-ap11theme for development, activate your CKAN virtualenv and
do::

    git clone https://github.com/ckan/ckanext-eurovoc.git
    cd ckanext-ap11theme
    python setup.py develop
    pip install -r dev-requirements.txt


-------------
Configuration
-------------

The ap11theme plugin doesn't automatically change CKAN templates or add a dcat ap11 theme
category field to your dataset schema.

If you want to add dcat ap11 theme category values to your schema, you will need to
modify the dataset schema in your own extension and add form widgets to the
appropriate templates.

You can use the example templates in ``ap11theme/templates/package/snippets`` to
add a form field for creating or editing dcat ap11 theme category values in a dataset.

If you aren't adding your own extension, or you aren't modifying the dataset
schema, you can add the optional ``ap11theme_dataset`` plugin to
``ckan.plugins`` to integrate the dcat ap11 theme category field into your schema and
templates.

If you are defining your own dcat ap11 theme category field name, ensure you have set
it as the value for ``ckanext.ap11theme.category_field_name``, as mentioned
below.


ckanext.ap11theme.categories
++++++++++++++++++++++++++

The display language for dcat ap11 theme category labels and additional solr search
terms are defined in category configuration files. These should be placed in
``ap11theme/categories/categories_*.json``, where '*' is the two-letter
country code for the language used.

The category config file to be used is defined in ckan config::

    ckanext.ap11theme.categories = categories_se.json  # sweden

If no categories file is defined, ``categories_en.json`` is used.

If the category file is changed, the solr search index will need to be rebuilt
for the changes to fully take effect::

    paster search-index rebuild


ckanext.ap11theme.category_field_name
+++++++++++++++++++++++++++++++++++

It is sometimes necessary to customise the dataset schema field name being
used to store the dcat ap11 theme category value. This can be set in the ckan config,
e.g.::

    ckanext.ap11theme.category_field_name = theme

The default value is ``eurovoc_category``.

Note: Changing the value of ``category_field_name`` will not migrate previous
values assigned to the old field name.


Search facet
++++++++++++

The dcat ap11 themes plugin will add a faceted search option to dataset, group and
organization search page.

You can make adjustments to the ap11 themes facet in your own extension by
updating the ``ap11theme_category_label`` entry in the facet dictionary using
the appropriate ``IFacets`` interface methods.


-----------------
Running the Tests
-----------------

To run the tests, do::

    nosetests --nologcapture --with-pylons=test.ini

To run the tests and produce a coverage report, first make sure you have
coverage installed in your virtualenv (``pip install coverage``) then run::

    nosetests --nologcapture --with-pylons=test.ini --with-coverage --cover-package=ckanext.ap11theme --cover-inclusive --cover-erase --cover-tests

