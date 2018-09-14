---
layout: page
title: Solr
permalink: /solr/
---

## Apache Solr

---

### What is it?

Solr is an Indexing tool. Instead of software looking through the text or metadata of files directly each time a search is performed,
Solr creates and searches an index of the key fields, text, and metadata that you would like to search on.

Solr thinks of all of your data in terms of a Document. There is an index of content. It contains multiple documents with many fields of text or metadata that Solr searches.

Solr is built on top of Lucene, a Java library for indexing, searching, highlighting, etc. They are both maintained by Apache. Solr just makes it easier to use Lucene (think of it as a kinda/sorta wrapper around Lucene).

A lot of tutorials for Solr use this comparison, a document is a table row in a database, a field is a table column.


### Basic Components

#### schema.xml:

The schema.xml file tells Solr about the contents of your documents and what it should index.

#### solrconfig.xml

Configuration file for your Solr instance. You can alter location of the core in this file for example.

#### field types:

Every field in Solr has a type.

   There are:

            - float
            - long
            - double
            - date
            - text

#### analysis:

Solr performs analysis and can transform content of fields before they are added to the index.

Examples of analysis: lower-casing words before they are indexed.

Any field that is indexed goes through an analysis.

#### storage:

A field might go through analysis to lower case, shorten the content to a snippet of the full content of the document, etc.
The full content of the field can be stored so that the full content of the document can be displayed to users on a front-end web application.


#### core (i.e. collection):

This is the configuration files (schema, solrconfig.xml, etc.) as well as the index of your docouments is located