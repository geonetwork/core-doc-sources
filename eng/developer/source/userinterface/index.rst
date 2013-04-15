.. _userinterface:

User Interface
==============

There are four different user interfaces on geonetwork:

- **Classic** - Perfect for hard environments, uses less javascript.

- **Search** - Uses the new widgets library. More responsive than the classic UI. `Example<http://newgui.geocat.net/geonetwork/apps/search/>`_

- **TabSearch** - Similar to the **Search** UI, but desktop-like as it uses tabs. `Example<http://newgui.geocat.net/geonetwork/apps/tabsearch/>`_

- **HTML5UI** - Also based on widgets, makes use of latest web technologies.

Classic
-------
//TODO

Search
-------

To use this UI, you have to compile the web project with **widgets** profile activated, like:
  mvn clean package -Pwidgets

.. figure:: search.png



TabSearch
-------

To use this UI, you have to compile the web project with **widgets-tab** profile activated, like:
  mvn clean package -Pwidgets-tab

.. figure:: tabsearch.png

HTML5UI
-------

To use this UI, you have to compile the web project with **html5ui** profile activated, like:
  mvn clean package -Phtml5ui


