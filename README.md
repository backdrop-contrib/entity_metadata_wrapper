Entity Metadata Wrapper module
=================

**This module is deprecated. Use [Entity Plus](https://github.com/backdrop-contrib/entity_plus) instead.**

This module is intended to port the "wrapper" bits from the Drupal Entity API
module.

This module extends the entity API of Backdrop core in order to provide a unified
way to deal with entities and their properties.

This is an API module. You only need to enable it if a module depends on it or
you are interested in using it for development.

HOW TO INSTALL:
---------------
- Install this module using the official Backdrop CMS instructions at 
https://backdropcms.org/guide/modules


Usage
---------------

* This module introduces a unique place for metadata about entity properties:
  hook_entity_property_info(), whereas hook_entity_property_info() may be
  placed in your module's {YOUR_MODULE}.info.inc include file. For details
  have a look at the API documentation, i.e. hook_entity_property_info() and
  at http://drupal.org/node/878876.

* The information about entity properties contains the data type and callbacks
  for how to get and set the data of the property. That way the data of an
  entity can be easily re-used, e.g. to export it into other data formats like
  XML.

* For making use of this information (metadata) the module provides some
  wrapper classes which ease getting and setting values. The wrapper supports
  chained usage for retrieving wrappers of entity properties, e.g. to get a
  node author's mail address one could use:

     $wrapper = entity_metadata_wrapper('node', $node);
     $wrapper->author->mail->value();

  To update the user's mail address one could use

     $wrapper->author->mail->set('sepp@example.com');

     or

     $wrapper->author->mail = 'sepp@example.com';

  The wrappers always return the data as described in the property
  information, which may be retrieved directly via entity_get_property_info()
  or from the wrapper:

     $mail_info = $wrapper->author->mail->info();

  In order to force getting a textual value sanitized for output one can use,
  e.g.

     $wrapper->title->value(array('sanitize' => TRUE));

  to get the sanitized node title. When a property is already returned
  sanitized by default, like the node body, one possibly wants to get the
  not-sanitized data as it would appear in a browser for other use-cases.
  To do so one can enable the 'decode' option, which ensures for any sanitized
  data the tags are stripped and HTML entities are decoded before the property
  is returned:

     $wrapper->body->value->value(array('decode' => TRUE));

  That way one always gets the data as shown to the user. However if you
  really want to get the raw, unprocessed value, even for sanitized textual
  data, you can do so via:

    $wrapper->body->value->raw();

      
LICENSE
---------------    

This project is GPL v2 software. See the LICENSE.txt file in this directory 
for complete text.

CURRENT MAINTAINERS
---------------    

Looking for maintainers

CREDITS   
--------------- 

Original Drupal version by Wolfgang Ziegler, nuppla@zites.net
