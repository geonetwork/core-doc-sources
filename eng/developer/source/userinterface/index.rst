.. _userinterface:

User Interface
==============

There are four different user interfaces on geonetwork:

- **Classic** - Perfect for hard environments, uses less javascript.

- **Search** - Uses the new widgets library. More responsive than the classic UI. `Example <http://newgui.geocat.net/geonetwork/apps/search/>`_

- **TabSearch** - Similar to the **Search** UI, but desktop-like as it uses tabs. `Example <http://newgui.geocat.net/geonetwork/apps/tabsearch/>`_

- **HTML5UI** - Also based on widgets, makes use of latest web technologies.

Compatibility table:

+------------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+
| Compatibility          |                                        IE                                         |    Chrome          |        Firefox     |       Safari       |
+------------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+
|                        |         7          |   8                |                9   |                10  |                    |                    |                    |
+========================+====================+====================+====================+====================+====================+====================+====================+
| Classic                | .. figure:: ok.png | .. figure:: ok.png | .. figure:: ok.png |                    | .. figure:: ok.png | .. figure:: ok.png | .. figure:: ok.png |
+------------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+
| Search                 | .. figure:: ok.png | .. figure:: ok.png | .. figure:: ok.png |                    | .. figure:: ok.png | .. figure:: ok.png | .. figure:: ok.png |
+------------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+
| TabSearch              | .. figure:: ok.png | .. figure:: ok.png | .. figure:: ok.png |                    | .. figure:: ok.png | .. figure:: ok.png | .. figure:: ok.png |
+------------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+
| HTML5UI                | .. figure:: pl.png | .. figure:: ok.png | .. figure:: ok.png |                    | .. figure:: ok.png | .. figure:: ok.png | .. figure:: ok.png |
+------------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+--------------------+

Full compatibility |okpng|

Compatibility with penalties |plpng|

No compatible |nopng|

Blank spaces means no information provided for that case.


.. |okpng| image:: ok.png
.. |plpng| image:: pl.png
.. |nopng| image:: no.png

Classic
-------

This is the default user interface in GeoNetwork. It is a complete user interface with all the functionalities the rest of the user interfaces have, but it is prepared to work on the hardest environments, using as less javascript and Ajax as possible, for example. 

You don't have to do anything special to run this user interface, as it is the default one.


Search
-------

To use this UI, you have to compile the web project with **widgets** profile activated, like:
  mvn clean package -Pwidgets

.. figure:: search.png

.. toctree::
    :maxdepth: 2

    widgets/index.rst

TabSearch
-------

To use this UI, you have to compile the web project with **widgets-tab** profile activated, like:
  mvn clean package -Pwidgets-tab

.. figure:: tabsearch.png

.. toctree::
    :maxdepth: 2

    widgets/index.rst

HTML5UI
-------

To use this UI, you have to compile the web project with **html5ui** profile activated, like:
  mvn clean package -Phtml5ui

.. figure:: html5ui2.png


.. toctree::
    :maxdepth: 2

    html5ui/index.rst
    
.. toctree::
    :maxdepth: 2

    widgets/index.rst

