.. _export:

Export facilities
=================

This tool is located under the Management tool on the left panel and
allows you to export a set of metadata using the MEF format. Selecting the Export
tool opens the option panel.

- **Output folder** - 
  The target folder in your file system where GAST will put the
  exported metadata. You can either select the Browse button to navigate through
  your file system to choose a better location or enter it manually in the
  text field.

- **Format** - 
  Here you can specify the metadata’s output format. See the MEF
  specification for more information.

- **Skip UUID** - 
  Normally this option is not required. If you
  select it, you will loose the metadata’s unique identifier (UUID) but you will
  be able to re-import that metadata over and over again. This is useful to fill
  the system with test data.

- **Search** - Allows to specify free text search criteria to limit the set of exported records.
  
- **Export** - This will start the export process. A progress dialogue will be opened to show the export status.

.. note:: The result of the export will depend on the access privileges of the user. If you do not authenticate, you will get only public metadata.

.. warning::
   Skipping the UUID on import or export can cause metadata to be duplicated.
   This should normally always be avoided
