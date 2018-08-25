## About
This C program implements the backpropagation learning rule for a multi-layer feedforward neural network (with a single hidden layer) to recognize hand-draw digits. The input will be a vector of features that represents a two-dimensional binary pattern, and the output will be a classification as one of ten possible digits (0-9).  

The purpose of this neural network is to experiment with various parameters- the learning rate, the steepness of the threshold/activation function (the k value in the sigmoid function), and the number of hidden units- to determine the effects of these parameters on how quickly the weights are learned, and to find the best possible network. I also implemented various techniques for annealing the learning rate (step decay and search then converge) and implemented the ability to use a validation set to minimize overfitting.

## Usage
> ./nn learning_rate k hidden log_file out_file < digits_train.txt  

Where:   
    log_file - file to record progress of training  
    out_file - file to record final network  
    digits_train.txt - the provided filed used to train the neural network


## Results
### Annealing the Learning Rate
Having a constant small learning rate would take a long time for us to converge. Likewise, if we have a constant big learning rate, the gradient descent can overshoot the minimum, fail to converge or possible even diverge. Thus, I studied the effect of annealing the learning rate. I implemented two approaches, step decay (reduce learning rate by half every X epochs) and the search then converge schedule (converge after X initial epochs). Each case was run two times and the average errors were recorded. Each case kept all other factors constant (0.35 learning rate initially, k = 1 and 20 nodes in hidden layer).  

![Alt text](/tables/t1.PNG?raw=true )

From these results, it is apparent that search then converge is superior to the step decay strategy. It consistently took fewer Epochs to get to acceptable levels of classification errors of the training set while maintaining low classification errors with the test set. The trials for each case sometimes varied significantly, and ideally more trials would be beneficial, but it appears as if Search the Converge with X = 75 is the optimal choice.

### Introducing A Validation Set
After introducing a validation set, we generally never met the desired threshold for validation accuracy (97.5%) for us to break early. We consistently reach the 1000 Epoch upper bound limit though. Approximately 20% of the data was used for validation and 20% was used for testing. Achieving a 95% accuracy on the test data is common given the use of a validation set. Sometimes we are lucky and achieve 97%.

### Number of Nodes in the Hidden Layer
![Alt text](/tables/t2.PNG?raw=true )  
While keeping all other factors constant, the number of nodes in the hidden layer was adjusted to different amounts. Each case was run two times and the average was recorded. Although a hidden layer with 25—35 nodes would suffice with a pretty good test data error rate, I found that 40 nodes consistently performed better (most trials had less than ~4% test data error rate). However, this came with a huge sacrifice in computation time, as the test took much longer with more nodes. This would have been even larger if we had more data. After further tests (36-39 and 41-44 nodes), 40 hidden nodes was the fewest number of nodes that performed consistently.

### Sigmoid Function K Value
All other parameters were kept constant (0.35 base learning rate, 40 hidden nodes). Each case was run two times and the average was recorded.  
![Alt text](/tables/t3.PNG?raw=true )   
Thus, from these results, we can conclude that K values greater than 2.0 are not that great. it is apparent that lower values for K are more optimal. In my very small test, it appears as if 1.5 is the best value.

### Optimal Parameters
40 hidden nodes, K = 1.5 and search the converge with x = 75 seem to be the best values.

### The Variability of the Data
One of the main limitations of my study were the number of trial runs I did for each case. I noticed huge variability in training data and test data error rates for any case. It all really depended on how lucky the division of the training, test and validation data was for each of the cases. To understand these parameters, it would be more ideal to use the average over 100 different trials for each case as opposed to just two (not feasible due to time constraints). Not only that, I didn’t test all possible values in the continuous range for each of the parameters (for example, K = 1.6). It would be interesting to see how slight changes would affect the performance of the neural network.

### Articles Used
https://towardsdatascience.com/learning-rate-schedules-and-adaptive-learning-rate-methods-for-deep-learning-2c8f433990d1  
https://towardsdatascience.com/understanding-learning-rates-and-how-it-improves-performance-in-deep-learning-d0d4059c1c10  
http://cs231n.github.io/neural-networks-3/#anneal  
https://www.willamette.edu/~gorr/classes/cs449/momrate.html  
