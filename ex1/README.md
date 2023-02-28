# Setup
This section explains how to get the notebook up and running.

## Environment
At the route of the github project - or whithin the deliverable on Illias, you should find a requirements.txt file.\
Install all the dependencies with ```pip install -r requirements.txt```.\
Once this is setup, please make sure that the jupiter notebook is configured to use said environnement (especially for virtual environments).

## Dataset
The dataset can be placed by either:
* Adding a folder called *mnist_small* next to the notebook, containing the test and train CSVs
* Linking the actual location of the folder properly, as opening the dataset is only executed once in the first sections of the notebook. Simply change:
> KNN_TRAIN = "./mnist_small/train.csv"
> KNN_TEST = "./mnist_small/test.csv"
To your locations / values.

# Features implemented
This notebook contains a simple KNN classifier using pandas for data retrieving and numpy for data computations.

## Distances
The classifier let the user choose between mahnattan and euclidian distance for classification. The computation is done using numpy vectorized function, meaning the ```Fdistance(array, training_matrix)``` is done all at once. A distance matrix is yielded of dimensions (training_matrix_length, 1) - a 1D array with the distance from the testing array to each training matrix's array.

## Memoization
The classifier is very heavily coupled to the train/test dataset as it is able to compute an internal distance map and keep in memory. This distance map allows the classifier to compute each distance once from each test item to each trained item, and simply re-sample the relevant classified classes when K is modified. This makes testing multiple K very easy as long as the same test set is used: The distances are computed once on the first iteration. Of course, this works only on the same distance and dataset. To further facilitate the work, the precompute_distance() function can be used to generate the distance map at once. The classifier can work without distance map (very usefull for condensation - see next paragraph).\

## Dataset reduction
The KNN classifier is able to condensate the data by applying the condensate algorithm:
* Keep a single item as the dataset
* Keep iterating over the full training dataset and switch items when a 1-NN yields a bad results

The classifier uses the same classification method but avoids memoization as the distance map would make no sense: Each iteration on the overall training dataset adds new active training data, so each switch between the overall and the new dataset changes the distance map. Hence, the classification method has a avoid_memoization parameter.

# Results
This section briefly exposes the results in term of accuracy depending on K and whether a condensation has taken place.

## Full training dataset
> The results are: 
> Evaluating distance EUCLIDIAN
> KNN prepared with 1000 data
> Precomputing distances...
> Distance map complete
> Evaluating for k=1...
> For k=1, score is 0.886% (886/1000)
> **Evaluating for k=3...**
> **For k=3, score is 0.89% (890/1000)**
> Evaluating for k=5...
> For k=5, score is 0.885% (885/1000)
> Evaluating for k=10...
> For k=10, score is 0.875% (875/1000)
> Evaluating for k=15...
> For k=15, score is 0.847% (847/1000)

For the Euclidian distance, the best accuracy is 0.89% with k=3.

> Evaluating distance MANHATTAN
> KNN prepared with 1000 data
> Precomputing distances...
> Distance map complete
> **Evaluating for k=1...**
> **For k=1, score is 0.875% (875/1000)**
> Evaluating for k=3...
> For k=3, score is 0.865% (865/1000)
> Evaluating for k=5...
> For k=5, score is 0.869% (869/1000)
> Evaluating for k=10...
> For k=10, score is 0.845% (845/1000)
> Evaluating for k=15...
> For k=15, score is 0.814% (814/1000)

The manhattan distance is a bit less effective, with a max accuracy of 0.875% at k=1.

## Condensated dataset

The dataset is condensated from :
> KNN prepared with 1000 data
To:
> Dataset now has 324 elements
Through 4 iterations over the full training dataset. This operation is rather quick.

> Evaluating for k=1...
> For k=1, score is 0.834% (834/1000)
> **Evaluating for k=3...**
> **For k=3, score is 0.854% (854/1000)**
> Evaluating for k=5...
> For k=5, score is 0.849% (849/1000)
> Evaluating for k=10...
> For k=10, score is 0.844% (844/1000)
> Evaluating for k=15...
> For k=15, score is 0.816% (816/1000)

Once again, the best accuracy for the Euclidian distance is at k=3. The accuracy went from 0.89% to 0.854%, which is a very fair loss of accuracy for the gain in computation (as without memoization, computing all the distances is fatally an exponential complexity)

> Evaluating distance MANHATTAN
> Evaluating for k=1...
> For k=1, score is 0.828% (828/1000)
> Evaluating for k=3...
> For k=3, score is 0.838% (838/1000)
> **Evaluating for k=5...**
> **For k=5, score is 0.852% (852/1000)**
> Evaluating for k=10...
> For k=10, score is 0.833% (833/1000)
> Evaluating for k=15...
> For k=15, score is 0.803% (803/1000)

Lastly, the Manhattan distance with condensated data hits 0.852% accuracy at k=5, from 0.875% accuracy at k=1. Again, this can be seen as an acceptable loss for the gain in computation requirements.