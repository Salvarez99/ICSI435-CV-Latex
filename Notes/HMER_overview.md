# Handwritten Mathematical Expression Recognition (HMER) Overview


Stephen Alvarez , Seoyeon Choi , Connor Hing, Marc McAdoo


## Introduction


Handwritten Mathematical Expression Recognition (HMER) is an interesting problem in the field of AI. There are many different challenges to over come and a variety of methods may be used to solve these problems. In an attempt to bring attention to HMER, as well as standardize comparisons of contemporary models, a competition was organized: *CHROME* Competition on Recognition of On-line Handwritten Mathematical Expressions. They introduced a large open source data-set available to everyone. This allowed for more standard comparisons of models. Having a centralized competition also allowed for a more holistic view of the problem landscape (How to determine which expressions are more difficult to interpret than others., Which models solve which problems most efficiently?). Since its inception in 2011 significant progress has been made in the field of HMER. This document will limit the scope to the first 4 years of *CHROME* as this is just an introduction.


The rest of this document will be broken up into sections 2.) Why is this problem interesting?, 3.) What is the data we will be working with? 4.) What are the main challenges? 5.) How are models evaluated? and 6.) What are some approaches taken to address HMER.


## Why is this problem interesting?


While converting handwritten text to typed text is a similar problem with similar challenges there are a few distinct challenges presented when handling math equations. One of the first problems that differentiate HMER is abuse of mathematical notation. Depending on the context/field/author's-personal-whims notation and its meaning may vary. This problem may be circumvented. By limiting the admissable expressions to standard algebra and calculus , whose notation is fairly standard , this removes the majority (if not all) ambiguity from the expressions themselves. But limiting to a specific set of symbols not mean expressions will be limited in complexity. As the way the symbols are structured may vary dramatically. (You may have both subscript, superscript as well as multi-layered expression, such as a fraction, all in one expression.) 


This problem is also exceptionally difficult and far from considered being "solved". When the competition started the percent of correct outputs on the testing data was hovering around 50% for the best models. And while there has been progress and more difficult expressions have been introduced the modern solutions still achieve at best roughly 80% accuracy. So we are still far from having a generalized framework to handle HMER, and new technologies and methods will need to be developed. 


Another interesting aspect is how to both 1.) Given two expressions determine which is more complex, and 2.) How to evaluate a models predictive power. There are many ways one could evaluate the complexity of a given expression. One could analyze the raw number of symbols, the number of baselines(vertical positions), number of distinct baselines, and max tree depth as metrics to gauge complexity. For evaluating models many different methods have been used. One useful method is report the percentage of correct symbols/sub-expressions with at most *n* incorrect symbols, with the scores for *n*=0,1,2,3 being reported in each competition. One can also use symbol and relationship detection, which measures the precision and recall of classified symbols. The competition also introduced a notion of *stroke label graphs* for precise structure error analysis, using either Label Hamming Distances or Stroke-Level Error. [More information can be found in the *CHROME* 2011-2014 revision]


## What data are we going to working with?


The files are stored in CHROME InkML (XML) which contains stroke data of the expression. Each stroke has a unique identifier and sequence of (x,y) locations. And of course the image of said expression. 


Outside of CHROME there are also other existing datasets. 


## What are the main challenges?


During training the model is responsible for multiple sub-problems to address, 1.) Symbol Segmentation, 2.) Symbol Classification, 3.) Spacial Relationship Classification and 4.) Parsing.


1. Symbol Segmentation: This problem involves grouping strokes into symbols. Once strokes have been grouped into symbols how they are used depends on the model.


2. Symbol Classification: This deals with classifying symbols. Firstly there are twos types of *features* available. The first in considered *on-line* which uses the sequence of points in strokes. And *offline* which uses the image of the symbol candidate. The advantage of on0line features is that this leverages information not present in the image, such as timing data, exact pen position and paths in crossing or overwriting. However, there will also be mre variability, different stroke orders, variation in writing speed, typical human-free-will related problems etc. It comes as no surprise that the best models leverage both features. The classification algorithms which may be used are diverse.


3. Spacial Relationship Classification: This deals with how symbols/sub-expressions relate to each other. Again there are a rich amount of methods and classifiers that may be used here, such as SVM.


4. Parsing: This deals with combining all of the symbols/sub-expressions into the correct syntax. Usually defined as a global optimization problem, which is maximizing the the join probability for symbols and their relationships. There are many approaches that may be used, some common examples of parsing algorithms used are Cocke-Yonger-Kasami (CYK), Ungers algorithm, or a type of baseline detection using MST's. 



## How are models evaluated?

There are few metrics that have been used to determine the effectiveness and accuracy of models.


1. Percent correct. *Chrome* reports the number of expressions which the model made at most *n* incorrect symbol or relationship errors, where *n*=1,2,3.

2. Symbol and relationship detection. This reports the precision and recall of recognized symbols.

3. Label graphs. Using a complete label graph they can measure Hamming distance and Stroke level error. Hamming distance calculate the exact disagreement between strokes for two interpretations. Stroke level error is a more detailed analysis of precisely which strokes were grouped correctly or incorrectly for a target symbol. 