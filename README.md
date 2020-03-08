# Building a Knowledge Graph using BERT based NER and Neo4j, then Predict Unknown Linkes

Building a knowledge graph could be very tidous and time consuming if the entities and relations need to be manually identified and input. In recent years, with the development of natural language processing, especially after the invention of BERT (Bidirectional Embedding Representations from Transformers) by GOOGLE in 2018, which drammatically improved the accuracy of named entity recoginition (NER), and enabled automatically extract NER and relations from documents. This make it possible to build a knowledge graph rapidly and update it promptly.

In this post, I will describe how to build a biomedical knowledge graph using BERT based named entity recognistion and Noe4j graph database, and then predict unknown connections using graph algorythm. This can be used for drug repurposing, and find new gene-disease relations.

Biomedical researchers read tons of articles to keep up with new discoveries and infer unknown connections among genes, diseases and drugs. However, human brains can’t remember everything we read, and once the interconnection degree increase, inference efficiency will not be able to handle the complexity. With knowledge graph technique and graph algorithm, we can build a knowledge graph for the subject of interest, and then run link prediction graph algorithms to discover potential new links for disease related gene identification or drug repurposing.

### Step 1. Named entity and relation extraction from lieratures
Named entity extraction is one of the successul applications of BERT in NLP. After fine tuning BERT using biomedical literatures, the derived [BioBERT](https://academic.oup.com/bioinformatics/article/36/4/1234/5566506) model have about 90% precision on biomedical named entity recognition. On top of BioBERT, [Kim et al has developed a tool called BERN](https://bern.korea.ac.kr/) that can extract biomedical named entities and identify the types of the entities, i.e. classify the entities into genes/proteins, diseases, drugs, etc.
![image](https://user-images.githubusercontent.com/44976640/76154020-e4518800-609a-11ea-8249-b250ac6e31fa.png)

If two entities exist in the same sentence, we know they are somehow connected, and record as one row of relation as below.
![image](https://user-images.githubusercontent.com/44976640/76154222-6abb9900-609e-11ea-82fc-a5ce98209b3e.png)

We we record the two entity names, ids, categories, as well as the sentence where the two entities were mentioned, and the Pubmed ID of the literature where the sentence exists.

### Step 2. Build the knowledge graph in Neo4j
With the entity relation dataset we obtained from the previous step, we can quickly build a knowledge graph using the following Cypher commands.












