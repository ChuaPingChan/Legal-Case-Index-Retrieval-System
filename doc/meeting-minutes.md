# Meeting Minutes
## Time and Location
5th April 2018 at R4 lounge
## Details

- Strategy: Create a basic functional system first, without too many fancy stuff. Enhance later.
- Use Python 3
- Use pandas to work with csv files
- To-Dos:
    - Searching: Process "a AND b AND c" as in homework 3, as though a, b and c are in separate queries. Then use AND-merge to combine the results
    - Use n-word indices
    - Ignore "title" field because the title usually appear in the "content" field anyway
    - Focus on "content" field first. "court", and "date" for later.
    - Index 'neutral-citation' because lawyers sometimes search with neutral-citations.
    - During searching, search for 3-words phrase match first, then 2-words, then finally 1-word
    - Query expansion:
            (i) WordNet
    - Handle css/js boilerplate code during indexing
    - Ideas to consider for enhancements:
        - "date" field
        - "court" field for authority static indexing
        - Use multi-threading to speed up search
        - textrazor - topic labelling functionality
        - Tiered postings
        - Champion lists
        - nltk.similar() function for thesaurus generation
        - Pseudo relevance feedback for query expansion
        - Possibility of exploiting Area of Laws (AOL), e.g. criminal, tort, contract. However, AOLs may overlap, e.g. road traffic can overlap with criminal and tort.
