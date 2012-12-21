.. _fragments:

Fragments
=========

GeoNetwork supports metadata records that are composed from fragments of metadata. The idea is that the fragments of metadata can be used in more than one metadata record.

Here is a typical example of a fragment. This is a responsible party and it could be used in the same metadata record more than once or in more than one metadata record if applicable.

::

 <gmd:CI_ResponsibleParty xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:gco="http://www.isotc211.org/2005/gco" >
   <gmd:individualName>
     <gco:CharacterString>John D'ath</gco:CharacterString>
   </gmd:individualName>
   <gmd:organisationName>
     <gco:CharacterString>Mulligan & Sons, Funeral Directors</gco:CharacterString>
   </gmd:organisationName>
   <gmd:positionName>
     <gco:CharacterString>Undertaker</gco:CharacterString>
   </gmd:positionName>
   <gmd:role>
     <gmd:CI_RoleCode codeList="./resources/codeList.xml#CI_RoleCode" codeListValue="pointOfContact"/>
   </gmd:role>
 </gmd:CI_ResponsibleParty>


Metadata fragments that are saved in the GeoNetwork database are called subtemplates. This is mainly for historical reasons as a subtemplate is like a template metadata record in that it can be used as a 'template' for constructing a new metadata record.

Fragments are not handled by GeoNetwork unless xlink support is enabled. See :ref:`xlink_config` in the 'System Configuration' section of this manual. The reason for this is that XLinks are the main mechanism by which fragments of metadata can be included in metadata records.

.. figure:: xlink-mechanism.png 




