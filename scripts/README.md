# Scripts

Here you will find all information related with the scripts done for this projects.

## 1. Scrapping 📑

In order to obtain the data to be analyzed, it is necessary to scrap the official documents from the main webpage and store them in a useful format.

The script that can be found inside the scrapping folder, it is splitted into two main phases:

1. Browser into the opengov.cat webpage and navigate up to the advanced search.
2. Iterate upon a list of people (politicians) to get all their available documents.

The browser navigation is performed using **Selenium**, while the database used to store the scrapped information is **MongoDB**.

### 1.1 Advanced Search

In order to get to the advanced search, we need to open a browser using selenium and go to the http://dogc.gencat.cat/ca/ url. Once there, we need to look for the "advanced search" button and click on it.

On the advanced search, we need to find the "searcher" using it's id (*textWorkds*) and introduce our search (for example: Artur Mas). once this is done, we need to click on the searcg button (*cercaAv1*).

Once the search is done, the advanced search will look something similar to the image below:

![data1](../img/data1.png)

### 1.2 Get documents and store them

Once we manage to get to results page, we have two types of information:
- Info Search: it gives us information about all the results (keywords, documents type, etc)
- Docs list: it gives us the whole list of documents.

This *Docs List* is what we want to iterate over.

To do so, we get the whole list of results by obtaining the *main_content* table information. Once we have it, we can iterate over each document.

DOGC documents are published in both html and pdf format. In our project, the automatic scraping process was performed using the html files, as they provide easier access to information and also add specific summaries for each document.

![data2](../img/data2.png)

For each document, we will get all the text but also the metadata that can be found on the left side of the document page. With this information, we can create a dataframe (from pandas) filled with all needed information: area, search, date, identificatior, issuing agency, text, document type, title and url.

![data3](../img/data3.png)


In order to gather better understanding of the information provided by the DOGC documents, we did two kinds of search. Concretely:

- 100 documents per document type (agreements, edicts, decrees and resolutions)
- 50 documents per person (José Montilla, Antoni Castells, Manuela de Madre, Montserrat Tura, Miquel Iceta, Ernest Maragall, Artur Mas, Joana Ortega, Felip Puig, Meritxell Borràs and Oriol Pujol)

These documents were ranked by relevancy prior to their download.

### 1.3 Output

The result of this first step is a mongoDB that contains all the scrapped information. For each document there is a row indexed by the author and the date, with a column for the whole text and columns with the metadata explained before.
