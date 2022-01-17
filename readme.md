# Company Sector Classification

## Overview
Situations exist, where given a company name, it would be useful to know what industry that company operates in. This includes working with open banking data, targeted advertisements, or transaction analysis.

## Purpose
I wanted to see whether it was possible to classify a company's sector purely from its name. For this project I used Yelp's dataset, which contains information on c. 160k companies.

## Dataset / Labels
Yelp classifies each company into multiple categories. For example 'oskar blues taproom' might be classified as [Gastropubs, Food, Beer Gardens, Restaurant]. For this project we decided to label each company to their most common class. FOr example, 'oskar blues taproom' would be labelled as 'restaurant'. This helps reduce the number of classes to 21. The full list is shown below.

![alt text](/images/classes.png)

I would note that whilst many major classes exist, this is not a perfect approach. Ideally company names would be matched according to a common set of industrial names (e.g GICS sub-industries). It might be possible to label these classes using heuristics, but that is left for another project.

## Models
I used word embeddings (GloVe) to create information vectors which represent the words in a company name. Given company names are quite short, word embeddings are useful to capture the relationships between different words with similar meanings. These embeddings are adjusted later when training the models. Two models were built:
- **ANN**: This has 1 hidden layer, and c. 14.5m parameters (embedding layer, 64 unit dense, 21 unit dense).
- **RNN**: This is similar to the ANN, but rather than flattening the embeddings and passing them through a ANN, the embeddings are passed through a bidirectional RNN. It has c. 14.3m parameters.

## Performance
The models were not fine tuned to a large degree (no regularization, no tuning of layers / units) as the primary objective was to see if an acceptable threshold of accuracy could be achieved. The ANN achieved an accuracy of 74%, whilst the RNN achieved an accuracy of 72%. I believe the benefits of using a RNN is limited by the generally short length of company names.

The absolute level of performance is very respectable given the high number of classes (21). Furthermore, it isn't clear what 'human level performance' would be. A fraction of names are probably ambiguous which are unclassifiable.

The within class accuracy varied. 'Automotive' was the highest class at 86%, followed by 'financial services' at 84% and, 'religious organizations' at 82%. The model struggled with 'arts & entertainment' at 45% and 'education' at 52%. Interestingly, this may indicate the dispersion of names within these categories.

![alt text](/images/accuracy.png)

## Application
Whilst 73% is a reasonable level of accuracy, if these models were being applied, it would probably make sense to apply a minimum threshold of certainty before classifying a name, or potentially only label classes where accuracy is highest (see above).

























