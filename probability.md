The course assumes that you are familiar with basic probability concepts, including:

  - Definition of random variables and events
  - Axioms of probability
  - Definition and rules of expectations of random variables and functions thereof
  - Independence
  - Commonly used probabilities for discrete (Bernoulli, Bionmial, Poisson will be ones we will use) and contionous random variables (Normal, Gamma (and special cases of it), Beta, Chi-Squared).
  

**Chapter 2** of Hoff's book provides an overview of probability. An excellent (and very approachable) reference is the book by Ross [Introduction to Probability](https://www.pearson.com/us/higher-education/program/Ross-First-Course-in-Probability-A-9th-Edition/PGM110742.html). There are pleanty of very good resources on the web...


Here is a summary of the topics I'll cover in class.


### Basic Rules of Probability (Chapter 2)

  - Random Variables, Sample Space & Events
  - Probability of the complement 
  - Probability of the Union
  - Law of Total Probability
  - Rule of Marginal Probability
  - Bayes Theorem
  
  
  ### Joint, Marginal & Conditional Probability
  
  
  
|     | Y=1| Y=2 |Y=3 | P(X) |
|-----|----|----|----|----|
| X=1 | p(X=1 & Y=1) | P(X=1 & Y=2) | P(X=1 & Y=3)| P(X=1) |
| X=2| p(X=2 & Y=1) | P(X=2 & Y=2) | P(X=2 & Y=3)| P(X=2) |
| P(Y) | p(Y=1) | P(Y=2) | P(Y=3)| 1|


**Discuss**:
  - Joint Probabilities  P(X & Y)
  - Marginal Probabilities P(X), P(Y)
  - Conditional Probabilities P(X|Y), P(Y|X)
  - Independence P(X,Y)=P(X)P(Y)
  - Conditional Independence   P(X,Y|Z)=P(X|Z)p(Y|Z)
      
      
 Consider the following joint distribution of X, Y and Z.
 
 **P(Z=1)=0.3**
 
 
 
 **P(X,Y|Z=0)**
 
 |     | Y=1| Y=2 |  |
|-----|----|----|----|
| X=1 | 0.1 | 0.1 | ? |
| X=2| 0.4 | 0.4 | ? |
|  | ? | ? |
 
 
 **P(X,Y|Z=1)**
 
|     | Y=1| Y=2 |  |
|-----|----|----|----|
| X=1 | 0.12 | 0.28 | ? |
| X=2| 0.18 | 0.42 | ? |
|  | ? | ? | 

**Find**: 

  1. The marginal probabilities of X and Y given Z.
  
  2. Are X and Y conditionally independent?
  
  3. Find P(X,Y) (marginal joint probability)

  4. Are X and Y independent?
  
  
  **Exchangeability & Conditional Independence**
 
[Main](https://github.com/gdlc/STT465/blob/master/README.md)  

