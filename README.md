# Building a Knowledge Graph using BERT based NER and Neo4j, then Predict Unknown Linkes

Building a knowledge graph could be very tidous and time consuming if the entities and relations need to be manually identified and input. In recent years, with the development of natural language processing, especially after the invention of BERT (Bidirectional Embedding Representations from Transformers) by GOOGLE in 2018, which drammatically improved the accuracy of named entity recoginition (NER), and enabled automatically extract NER and relations from documents. This make it possible to build a knowledge graph rapidly and update it promptly.

In this post, I will describe how to build a biomedical knowledge graph using BERT based named entity recognistion and Noe4j graph database, and then predict unknown connections using graph algorythm. This can be used for drug repurposing, and find new gene-disease relations.

Biomedical researchers read tons of articles to keep up with new discoveries and infer unknown connections among genes, diseases and drugs. However, human brains can’t remember everything we read, and once the interconnection degree increase, inference efficiency will not be able to handle the complexity. With knowledge graph technique and graph algorithm, we can build a knowledge graph for the subject of interest, and then run link prediction graph algorithms to discover potential new links for disease related gene identification or drug repurposing.

### Step 1. Named entity and relation extraction from lieratures
Named entity extraction is one of the successul application of BERT. After fine tuning BERT using biomedical literatures, the derived [BioBERT](https://academic.oup.com/bioinformatics/article/36/4/1234/5566506) model can have about 90% precision on biomedical named entity recognition.
