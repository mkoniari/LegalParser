# Towards Automatic Structuring and Semantic Indexing of Legal Documents

This page is a companion for the PCI 2016 paper on [Towards Automatic Structuring and Semantic Indexing of Legal  Documents](http://dx.doi.org/10.1145/3003733.3003801), written by Koniaris Marios (me), George Papastefanatos and Yannis Vassiliou. This page hosts the complete dataset, ground-truth data, queries and relevance assessments we utilize in the article. Our goal is to encourage progress on the Automatic Structuring and Semantic Indexing of Legal Documents.

## Intro
Legal documents are usally stored and offered to the end user in presentation oriented manifestation, making impossible for the end users to inquiry  semantics about the documents, such as date of enactment, date of repeal, jurisdiction, etc. or to reuse information and establish an interconnection with similar repositories. We present an approach for extracting a machine readable semantic representation of legislation, from unstructured document formats. Our method exploits common formats of legal documents to identify blocks of structural and semantic information and models them according
to a popular legal meta-schema, [Akoma Ntoso](http://www.akomantoso.org/)

## Approach Overview

Several guidelines/ principles of good legislative drafting, both at 
National and E.U. level (e.g., Joint Practical Guide for persons involved in the drafting of European Union legislation6) have established common formats, which most legal documents abide by. In a simplified view, a legislative legal
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
Our parser imlementation can handle the following types of documents
TYPE    |  URI | STRUCTURAL ANALYSIS
-------- | --- | -----------
LAW| act | YES
PRESIDENTIAL DECREE| pd |YES
regulatory act   |ap | YES/NO
ΕΓΚΥΚΛΙΟΣ| egk| NO
OTHER LEGAL DOCS| other | NO

The Structural Analysis refers to whether the hierarchical structure of a document is considered by the parser e.g., the contents analyzed at levels (Article, paragraph) or not.

#### LAW
Laws  strictly follow the structure offered by the [National Printing Service](http://www.et.gr). Because of the strict hierarchical structuring of laws, files not conforming to the pdf layout of the National Printing Service are currently not supported.

### PRESIDENTIAL DECREE
Presidential  Decrees must follow the structure offered by the [National Printing Service](http://www.et.gr). Files not conforming to the pdf layout of the National Printing Service are currently not supported. Additionally supports the existence of Contents in Government Gazette  In this case the introduction of a pdf will lead to the introduction of multiple documents in the system. The system detects appropriate contents of Table of Contents and the structure of the decisions contained eg Presidential Decree or Ministerial Decision

Τα Προεδρικά Διατάγματα που εισάγονται στο σύστημα ακολουθούν αυστηρά την δομή/ μορφή Φ.Ε.Κ.  [Εθνικό Τυπογραφείο](http://www.et.gr) υποστηρίζεται επιπρόσθετα η ύπαρξη Πίνακα Περιεχομένων σε Φ.Ε.Κ. Στην περίπτωση αυτή η εισαγωγή ενός pdf θα επιφέρει την εισαγωγή πολλαπλών εγγράφων στο σύστημα. Το σύστημα ανιχνεύει κατάλληλα τα περιεχόμενα του Πίνακα Περιεχομένων καθώς και την δομή των αποφάσεων που περιέχει πχ Προεδρικό Διάταγμα ή Υπουργική Απόφαση. 

Λόγω της αυστηρής ιεραρχικής δόμησης δεν εισάγονται στο σύστημα αρχεία που δεν είναι σε μορφή Φ.Ε.Κ.
Μόνη **εξαίρεση** αποτελούν κενά εισερχόμενα έγγραφα (δηλαδή χωρίς περιεχόμενο), όπου το σύστημα παράγει τον σκελετό ενός συντακτικά δομημένου Προεδρικού Διατάγματος και το οποίο στην συνέχεια θα πρέπει να **συμπληρωθεί** από τον χειριστή.

### ΑΠΟΦΑΣΗ   
Οι αποφάσεις μπορεί να ακολουθούν:

 - Δομή/ μορφή Φ.Ε.Κ.  [Εθνικό Τυπογραφείο](http://www.et.gr), οπότε ισχύουν τα προηγούμενα
 - Δομή γενικού κειμένου: Στην περίπτωση αυτή το κείμενο δεν δομείται ιεραρχικά. Αναγνωριστικό γνώρισμα της απόφασης αποτελεί 

### Υπόλοιποι Τύποι
Χαρακτηριστικό γνώρισμα για την ανάλυση/ δόμηση όλων των μη ιεραρχικών τύπων εγγράφων αποτελεί η λεξη "ΘΕΜΑ", μέσω της οποίας γίνεται η ανίχνευση του τίτλου ενός εγγράφου. Με βάση την λέξη θέμα το κείμενο χωρίζεται σε προοίμιο και κυρίως μέρος. Δεν θα πρέπει να είναι η πρώτη σειρά/ λέξη ενός κειμένου.
Επιπρόσθετα σε περίπτωση ύπαρξης "ΠΙΝΑΚΑ ΔΙΑΝΟΜΗΣ" τα περιεχόμενα αυτού δεν αναλύονται.

##Categorizer

### Σημασιολογική ανάλυση

Η σημασιολογική ανάλυση περιλαμβάνει την ανίχνευση 

> Εκδούσα Αρχή : Η ανίχνευση της Εκδούσας Αρχής γίνεται στην προμετωπίδα του κειμένου με ειδικά παραμετροποιημένους κανόνες, οι οποίοι μοντελοποιούν την ιεραρχική οργάνωση του οργανισμού.

> Βαθμός Υπογράφοντα : Η ανίχνευση του βαθμού υπογράφοντα γίνεται στο κείμενο "Υπογραφές", με βάση 
ειδικά παραμετροποιημένους κανόνες. Τα υποστηριζόμενα επίπεδα είναι:

 1. ΠΡΟΕΔΡΟΣ ΤΗΣ ΕΛΛΗΝΙΚΗΣ ΔΗΜΟΚΡΑΤΙΑΣ
 2. ΠΡΩΘΥΠΟΥΡΓΟΣ
 3. ΑΝΤΙΠΡΟΕΔΡΟΣ ΤΗΣ ΚΥΒΕΡΝΗΣΗΣ
 4. ΤΑ ΜΕΛΗ ΤΟΥ ΥΠΟΥΡΓΙΚΟΥ ΣΥΜΒΟΥΛΙΟΥ
 5. ΥΠΟΥΡΓΟΙ
 6. ΥΦΥΠΟΥΡΓΟΣ
 7. ΓΕΝΙΚΟΣ ΓΡΑΜΜΑΤΕΑΣ
 8. ΕΙΔΙΚΟΣ ΓΡΑΜΜΑΤΕΑΣ
 9. ΠΡΟΪΣΤΑΜΕΝΟΣ
 10. ΑΛΛΟΣ

> Κατηγοριοποίηση :
Η κατηγοριοποίηση του εγγράφου γίνεται με ειδικά παραμετροποιημένους κανόνες σε συσχέτιση με την εκδούσα αρχή.

> Λέξεις κλειδιά:
Οι πιό συχνές λέξεις του κειμένου, με εξαίρεση stop words.

### Ανίχνευση συσχετίσεων 

Η ανίχνευση συσχετίσεων περιλαμβάνει
> Νόμους 
> ΠΔ 
> ΠΟΛ

