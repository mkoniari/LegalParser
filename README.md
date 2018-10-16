# Towards Automatic Structuring and Semantic Indexing of Legal Documents

This page is a companion for the [PCI 2016](http://pci2016.teiwest.gr/) paper on [Towards Automatic Structuring and Semantic Indexing of Legal  Documents](http://dx.doi.org/10.1145/3003733.3003801), written by Koniaris Marios (me), George Papastefanatos and Yannis Vassiliou. This page hosts additional info, as to encourage progress on the Automatic Structuring and Semantic Indexing of Legal Documents.

## Intro
Legal documents are usually stored and offered to the end user in presentation oriented manifestation, making impossible for the end users to inquiry semantics about the documents, such as date of enactment, date of repeal, jurisdiction, etc. or to reuse information and establish an interconnection with similar repositories. We present an approach for extracting a machine readable semantic representation of legislation, from unstructured document formats. Our method exploits common formats of legal documents to identify blocks of structural and semantic information and models them according
to a popular legal meta-schema, [Akoma Ntoso](http://www.akomantoso.org/)

## Approach Overview

Several guidelines/ principles of good legislative drafting, both at 
National and E.U. level (e.g., [Joint Practical Guide for persons involved in the drafting of European Union legislation](https://eur-lex.europa.eu/content/techleg/KB0213228ENN.pdf)) have established common formats, which most legal documents abide by. In a simplified view, a legislative legal
document has the following structure:
* Introductory part
* Text body
* End part

A visual aid of the aforementioned structure for a law, where we manually annotated structural parts and metadata values is given in the following figure:
![greek law structure](https://raw.githubusercontent.com/mkoniari/LawParser/master/figures/fig1.png "greek law structure")

Extraction and structuring of pdf files is done in three subsystems:
* Extractor: Extracting text / images from the pdf.
* Parser: Structural Text parsing and modeling 
* Categorizer: Semantic analysis & linkage analysis.

### Extractor
#### Text Extract
Text is acquired either form pdf files or from an ocr program.
#### Image Extract
We obtain images directly from the pdf.

### Parser
Our parser implementation can handle the following types of documents

| TYPE  | URI | STRUCTURAL ANALYSIS|
| ------------- | ------------- | ------------- |
| Law| act | YES |
| Presidential  Decree| pd |YES |
| Regulatory Act  |ap | YES/NO |
| Information circulars| egk| NO |
| Other Legal Docs| other | NO |



The Structural Analysis refers to whether the hierarchical structure of a document is considered by the parser e.g., the contents analyzed at structural hierarchical levels (article, paragraph) or not.

#### Law
Laws strictly follow the structure offered by the [National Printing Service](http://www.et.gr). Because of the strict hierarchical structuring of laws, files not conforming to the pdf layout of the National Printing Service are currently not supported.

### Presidential Decree
Presidential Decrees must follow the structure offered by the [National Printing Service](http://www.et.gr). Files not conforming to the pdf layout of the National Printing Service are currently not supported. Since a Presidential Decree may actually contain several others, a single pdf will lead to the creation of multiple legal documents. The parser detects from the Table of Contents and both the number and the type, i.e. Presidential Decree or Ministerial Decision, of the decisions contained within. 

### Regulatory Acts   
Regulatory Acts may follow:
 - [National Printing Service](http://www.et.gr) structure, as noted above
 - General form: In this case due to the lack of proper guidelines, the text is not hierarchically structured, only metadata are identified.

### Information circulars - Other Legal Docs
In this case due to the lack of proper guidelines, the text is not hierarchically structured, only metadata are identified.

## Categorizer

### Semantic analysis

The semantic analysis includes, among others, the detection of: 

* Issuing Authority: Specially customized rules expressed as regular expressions that model the hierarchical organization of the issuing agency.

* Signer: Supported levels are:
  1. PRESIDENT OF THE GREEK REPUBLIC
  2. PRIME MINISTER
  3. VICE PRESIDENT OF GOVERNMENT
  4. MEMBERS OF THE CABINET OF MINISTERS
  5. MINISTERS
  6. DEPUTY MINISTER
  7. GENERAL SECRETARY
  8. SPECIAL SECRETARY
  9. OTHER

* Categorization/ classification:
Document classification works with customized rules closely related to the issuing authority.

* Keywords:
The most frequent words, except for stop words.

### Linkage analysis
In alignment with the EU proposed standard for a European Legislation Identifier (ELI) that provides, among others, a solution to uniquely identify and access national and European legislation online, our approach offers the minimum set of metadata required by the ELI standard and assigns a URI at each different legal block modeled in Akoma Ntoso. For example the '.../gr/act/2008/3643/main/art/1/...' URI identifies article 1 of the main part of the act with no 3643, published in 2008 by the Greek Parliament. In this way the mark-up of each structural unit of the document complies with the ELI standard, as to facilitate the precise linkage of legal citations for each respective structural unit.

## DSL Language for legal documents

Domain-specific modeling is a software engineering methodology for designing and developing systems directly from
the domain-specific models, offering tailor-made solutions to problems in a particular domain. For the identification of the syntax rules, we heavily rely on domain knowledge from the legal experts who provide with feedback on the structural parts and their relationships (nesting, succession, etc.) within the legal documents.  

A visual aid of the context-free grammar (CFG), described in Extended Backus-Naur Form,  is given in the following figure, where nonterminals such as body and conclusions are defined separately:
![greek legal documents EBNF](https://raw.githubusercontent.com/mkoniari/LawParser/master/figures/fig2.png "greek legal documents EBNF")

## Parsing Process

### Document Structure Parser
Based on the defined GFG and the set of syntax rules defined, we employ [ANTLR](http://www.antlr.org/) parser generator as to implement the lexer, parser, and tree walker. 
An overview of the document structure parser, is given in the following figure:
![Overview of Document Structure Parser](https://raw.githubusercontent.com/mkoniari/LawParser/master/figures/fig3.png "Overview of Document Structure Parser")

### Parsing Strategy
The main steps of our approach are:
* identify the structure of the legal documents
* identify legal documents metadata
* validate produced files against the selected schema

We follow a pipeline strategy, utilizing a top-down approach, that can be summarized into the following 5 steps:

1. Document Type Identification. 
2. Structural Analysis. 
3. Legal Blocks Isolation
4. Legal Modeling
5. Semantic check and validation

## Samples
File [Greek law 4330](https://github.com/mkoniari/LawParser/blob/master/samples/law_4330_2015.pdf) contains Greek law 4330 as published in the official government gazette (volume 59 / year 2015). The resulting Akoma Ntoso file is [xml Greek law 4330](https://github.com/mkoniari/LawParser/blob/master/samples/law_4330_2015.xml)

## Acknowledgements
Within this work we utilize 
* [Akoma Ntoso](http://www.akomantoso.org/) a popular XML schemas for representing legal documents
* [akomantoso-lib](http://kohsah.github.io/akomantoso-lib/) a Java API for creating and editing Akoma Ntoso XML documents
* [ANTLR](http://www.antlr.org/) ANother Tool for Language Recognition is a powerful parser generator widely used to build languages, tools, and frameworks. From a grammar, ANTLR generates a parser that can build and walk parse trees.

We wish to thank the [General Secretariat of Public Revenue](http://www.publicrevenue.gr) for providing  a corpus of more than 600 legal documents, such as laws, presidential decrees and regulatory acts, in pdf format, on which iterative tests have been carried out, aiming at modeling and refining our method.

Finally our parsing mechanism has been deployed in a real-world web platform [e-Lib](http://www.publicrevenue.gr/elib/), aiming to provide semantic access to Greek tax legislation, under the supervision of the [General Secretariat of Public Revenue](http://www.publicrevenue.gr).
