# Building a Knowledge Graph using BERT based NER and Neo4j, then Predict Unknown Links

Building a knowledge graph could be very tedious and time consuming if the entities and relations need to be manually identified and inputted. In recent years, with the development of natural language processing, especially after the invention of BERT (Bidirectional Embedding Representations from Transformers) by Google in 2018, which dramatically improved the accuracy of named entity recognition (NER), and enabled automatically extract NER and relations from documents. This makes it possible to build a knowledge graph rapidly and update it promptly.

In this post, I will describe how to build a biomedical knowledge graph using BERT based named entity recognition and the Noe4j graph database, and then predict unknown connections using graph algorithm. This can be used for drug re-purposing, and find new gene-disease relations.

Biomedical researchers read tons of articles to keep up with new discoveries and infer unknown connections among genes, diseases and drugs. However, human brains can’t remember everything we read, and once the interconnection degree increase, inference efficiency will not be able to handle the complexity. With knowledge graph technique and graph algorithm, we can build a knowledge graph for the subject of interest, and then run link prediction graph algorithms to discover potential new links for disease related gene identification or drug re-purposing.

### Step 1. Named entity and relation extraction from literatures
Named entity extraction is one of the successful applications of BERT in NLP. After fine tuning BERT using biomedical literatures, the derived [BioBERT](https://academic.oup.com/bioinformatics/article/36/4/1234/5566506) model has about 90% precision on biomedical named entity recognition. On top of BioBERT, [Kim et al has developed a tool called BERN](https://bern.korea.ac.kr/) that can extract biomedical named entities and identify the types of the entities, i.e. classify the entities into genes/proteins, diseases, drugs, etc.

![image](https://user-images.githubusercontent.com/44976640/76154020-e4518800-609a-11ea-8249-b250ac6e31fa.png)

If two entities exist in the same sentence, we know they are somehow connected, and record as one row of relation as below.

![image](https://user-images.githubusercontent.com/44976640/76154222-6abb9900-609e-11ea-82fc-a5ce98209b3e.png)

We we record the two entity names, ids, categories, as well as the sentence where the two entities were mentioned, and the Pubmed ID of the literature where the sentence exists.

### Step 2. Build the knowledge graph in Neo4j
With the entity relation dataset we obtained from the previous step, we can quickly build a knowledge graph using the following Cypher commands.

![image](https://user-images.githubusercontent.com/44976640/76154365-b66f4200-60a0-11ea-97ea-2ac405733307.png)

A small proportion of the knowledge graph we get looks like this. The red nodes are diseases, the green nodes are genes, and the blue nodes are drugs. 

![image](https://user-images.githubusercontent.com/44976640/76154404-6f358100-60a1-11ea-9f2d-00fdc78f28f1.png)

### Step 3. Predict Unknown Relations between Entities
With knowledge graph in place, we can then predict unknown relations between entities. There are many algorithms to predict unknown connections. We will use the Adamic Adar method as an example. The basic idea behind the algorithm is the probabilities of two pairs of entities (A and C, A and E) have direct connection depending on their common neighbour (B, D).  The more friends their common neighbour has, the lower probability that the common neighbour will introduce them to know each other.

![image](https://user-images.githubusercontent.com/44976640/76154501-149d2480-60a3-11ea-84d3-08a68236cc3e.png)

The above picture is adopt from [link Prediction Algorithms](http://be.amazd.com/link-prediction/).

The calculation of Adamic Adar scores using cypher query is shown as below.

![image](https://user-images.githubusercontent.com/44976640/76154543-a7d65a00-60a3-11ea-9e3a-ea0e43f56a47.png)

We have used a set of literatures of interest to build a knowledge graph, and predict drug-disease links that are not recorded in this set of literatures.

![image](https://user-images.githubusercontent.com/44976640/76154578-495dab80-60a4-11ea-9940-3da32d753dbf.png)

We found that the predicted drug-disease link with the highest Adamic Adar score actually showed up as the title in a recently published paper. This is an successful example that the knowledge graph we built can be used for drug re-purposing, or finding new gene-disease relation, etc.
