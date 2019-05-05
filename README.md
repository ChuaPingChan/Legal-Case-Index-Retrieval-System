This is the README file for A0126623L-A0126576X's submission

== Python Version ==

We're using Python Version 3.6.4 for
this assignment.

== General Notes about this assignment ==

** Architecture Overview **
index.py
    - Index class represents the 'dictionary'
    - Mostly procedural programming paradigm

search.py
    - Query class is an abstraction for a query, which records the expansion version of the query, as well as if the query is a boolean retrieval query.
    - QueryParser accepts the raw query and create Query objects. It is able to determine whether the query is a boolean retrieval query or a free text query.
    - The SearchEngine class is the main search mechanism to interpret the index and postings as well as getting the result from the query objects
    

** Scalable Indexing **
One challenge of this homework is the indexing of the large corpus consisting more than 17,000 documents. A modified SPIMI was implemented, where there is a notion of 'blocks' or 'chunks'. Each chunk consists of 1000 documents. We index the documents chunk-by-chunk, storing the block dictionary and block postings as intermediate files, and finally merging them in the end.

After we have a scalable index mechanism, we can use the concurrent features in python3 to run indexing and merging concurrently. We indexed in the Tembusu cluster, which took around 40-50 minutes, which is a huge speedup from a single-threaded version, which uses around 2~3 hours.

** Query Expansion **
Query expansion and pseudo relevance feedback were used to expand the query to obtain more results. This is discussed in greater detail in BONUS.docx.

** Phrasal Query **
Two methods were considered and implemented for answering phrasal queries.
    i) N-gram index
    ii) Positional index
We started off with using N-gram index. However, we later found that the number of terms (i.e. unigrams, bigrams, trigrams) becomes too large, making the entire index files too large. To save space, positional index is used, where by the structure of postings are changed to (docID, [pos1, pos2, ...], tf-idf). Changing from N-gram index to positional index reduced the size of `dictionary.txt` from 1.2 GB to 7.5 MB and the size of `postings.txt` from more than 2 GB to roughly 1 GB.

In addition to the above, we also decided to make our our system's phrasal search more accommodating by appending the results of the phrasal search with the results obtained by treating the phrasal queries as free text queries.
For example, to answer a phrasal query of "fertility treatment", the top results would first be documents with "fertility treatment" as a phrase, followed by documents with "fertility" and/or "treatment".

** Ranked Phrasal Query **
Phrasal queries are ranked by first doing one round of boolean retrieval, and subsequently computing the cosine similarity of all discovered document to the query and subsequently sort them in non-increasing score.

** Law Report Citation **
Law report citations are unique for each case and lawyers often search for law reports using citations. As citations play such a central role in law report searching, the law report citations are extracted to facilitate searches.

** Court Hierarchy **
The corpus of this homework assignment consists of law reports from courts of the United Kingdom, Australia, Singapore, Hong Kong, and the US. Different courts have different levels of authority depending on the position of the court in the country's court hierarchy. A court with higher authority therefore has more influential reports, which can be factored into the way search results are ranked.

The court hierarchies for all the countries mentioned aboved were investigated online, and for court of each country, we assigned 'authority values' ranging between 0 and 1, where 1 indicates the highest level of authority. These values are moderated across all countries such that the authority values of courts of different countries are comparable.
During search, the cosine similarity values for a document is multiplied by the 'authority value' of its corresponding court before sorting the search result.

However, during the evaluation phase, it was found that finding the right weights for the courts is non-trivial and due to time-constraints, we decided to turn off the influence of courts in the ranking of our results using a flag.

** Autocorrect (Experimental) **
During indexing, it was discovered that the documents in dataset.csv has many spelling errors, such as 'ludqment', 'distlnction', 'llkellhood', etc. This is probably due to OCR technologies when trying to convert pdf or images of law documents into csv. We attempted to use the `autocorrect` package to do the corrections. However, the experiment that attempts to correct each and every words in the corpus took more than 24 hours to index. Therefore, correcting spelling is impractical. We believe that the result will be more informed and more useful if we are able to correct spelling, or have a fuzzy searcher that also returns results that has spelling errors.

** Allocation of work **
A0126576X
- search.py
- Boolean and free text search
- Query expansion and pseudo-relevance feedback
- Helped in optimization to speed up indexing
- Multiprocessing to speed up indexing

A0126623L
- index.py
- N-gram indexing (experimental)
- Scalable indexing - Modified SPIMI
- Report


== Files included with this submission ==

index.py        - Performs indexing
search.py       - Performs searching
dictionary.txt  - Pickled dictionary mapping terms to (offset, size, idf)
postings.txt    - Pickled postings
citation-docID.txt  - Pickled dictionary mapping law report citations to document ID
docID-court.txt - Pickled dictionary mapping document ID to courts
BONUS.docx      - Outlines and discusses the query expansion techniques used in this homework assignment

== Statement of individual work ==

Please initial one of the following statements.

[x] I, A0126623L-A0126576X, certify that I have followed the CS 3245 Information
Retrieval class guidelines for homework assignments.  In particular, I
expressly vow that I have followed the Facebook rule in discussing
with others in doing the assignment and did not take notes (digital or
printed) from the discussions.  

[ ] I, A0000000X, did not follow the class rules regarding homework
assignment, because of the following reason:

<Please fill in>

I suggest that I should be graded as follows:

<Please fill in>

== References ==

Textbook:
"Introduction to Information Retrieval", written by Christopher D Manning, Prabhakar Raghavan, Hinrich Schutze
