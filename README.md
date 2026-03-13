# OntoQA

## Not to be confused with...

A framework called [OntoQA](https://ontologforum.org/index.php/OntoQA) was published by Dr. Samir Tartir in 2013. It is no longer available.

## Purpose

Template repository with a standard folder structure for publishing ontologies, and actions allowing to perform some ontology quality checks and documentation steps:

* Consistency and completeness check, using a selected reasoner (HermiT by default)
* OWL2 profile compliance checks: DL, RL
* WIDOCO generation
* export of the integrated wiki (if present) to PDF or MD

Please note that the formal checks performed do not deliver Quality Assurance in the sense of ISO 900X: the checks are purely syntactic and do not quantify the suitability of the ontologies for any defined purpose:

Quoting from the Ontolog Forum (see above), "...there are no global criteria defining how a good ontology should be...". 

## Documentation

- [Wiki Setup Guide](docs/WIKI_SETUP.md) - How to configure GitHub Wiki documentation generation
Ontology "quality assurance" workflows and actions, to be used with multiple ontology repositories
