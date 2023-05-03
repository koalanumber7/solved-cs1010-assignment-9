Download Link: https://assignmentchef.com/product/solved-cs1010-assignment-9
<br>
# Digits——————-

In this assignment, your goal is to write a program thatcan recognize handwritten digits. This is a classic problemin pattern recognition and machine learning. The state ofthe art techniques can now achieve a recognition rate ofover 99%. We, however, will implement a simple algorithmcalled k-nearest neighbor algorithm, which has a loweraccuracy but is simpler.

## Supervised Machine Learning

The k-nearest neighbor algorithm belongs to a category ofalgorithm called _supervised machine learning_ algorithms.These algorithms teach or train the machine to learn to dosomething, by showing the machine many samples.  These arecalled _training samples_.  After showing the machine abunch of these training samples, we can then _test_ themachine how well it has learn, by showing the machine some_testing samples_.

In our case, we want to train the machine to recognizehandwritten digits.  During the training phase, we willpresent a set of handwritten digits in the form of images(as 2D arrays) to the machine, together with a labelindicating which digits it is (i.e., “this picture showsthe digit 1, this picture shows the digit 4, etc.”).

During the testing phase, we will then present a set ofhandwritten digits to the machine, and ask, “ok, whichdigit to you think this is?”

We will then compare the machine’s answer with the labelof the handwritten digit (also called the _ground truth_)to see whether the machine recognize correctly or not.

## Representation of a handwritten digit

A handwritten digit is an image, represented as a 28×28array of characters, consisting of . and #. An example is:

“`……………………….……………………….……………………….……….#######………..………########………..……..##########……….…….####….###……….…….###….####……….…………..####……….………….####………..…………#####………..…………####…………………..####………….………..###…………..……….####…………..………####……………………####…………………..####…………….……..###……………..……..###………..####..……..###################.……..###################.………###########……..……………………….……………………….……………………….……………………….……………………….“`

The image above contains the digit 2.

## Training Data

For this assignment, we will use the data provided by theMNIST Handwritten Digit Database. The original data hasbeen post-procesed into CSV file by Joseph Redmon, thenpost-processed again by yours truly into the format above.

The dataset from above contains 60,000 training samples. Ihave created two smaller subsets for you to test your code.

The first subset contains 10 samples for each digit —this is given in the file `train100.in`.

The second subset contains 6 samples for digits 0 and 1 —this is given in the file `train6.in`.

The full training set from MNIST is too big to be includedon GitHub. You can read it from `~cs1010/as09/train60000.in`if you want to play with it.

## Testing Samples

We provide two sets of testing samples. The first containsthree testing samples per digit. The second contains onlya single digit corresponding to the example below.

The full training dataset from MNIST contains10000 digits. It takes a long time to test everyhandwritten digit in this file, so you should dothis only after you have made sure that your code isfast and is correct. Again, the file is too big tobe posted on GitHub. You can read it directly from`~cs1010/as09/test10000.in`.

## The Algorithm

We will use the k-nearest neighbor algorithm for thisassignment.  The idea of this algorithm is that, givena testing sample, the machine will compare it to allthe training samples, and find out which digit thistesting sample is most similar to.

Let’s define the distance between the two handwrittendigits d(x1,x2) as the number of pixels that are differentbetween them, i.e., how many pixels are `#` in one imagebut is `.` in the other image.

Given a test sample, q, we find the distance betweenq and all the available training samples and find thek training samples with the smallest distance (i.e.,k nearest neighbors).

k is usually small — we use k=5 in this assignment.

The intuition is that q must be “close” to the trainingdata that has the same labels (i.e., the same digits). Sowe look at the these k nearest neighbors and find themost common digit d among them. We then recognize q ascontaining the digit d. If there are more than one mostcommon digits or more than k nearest neighbors , thenwe break ties by returning the nearest (i.e., shorterdistance) digit.  In the case where even the distance isthe same, we return the smaller digit.

## Efficiency

Suppose we have n training samples, recognizing a digitshould take no more than O(kn) time (or O(n) since k isa constant).

There is also an opportunity to stop early the calculationof the distance between two handwritten digits if thedistance is too large, pruning away redundant work.

## Example

Consider the following simple example with six trainingsamples and a test sample.

“`60……………………….……………………….……………………….……………………….……………#####……..……………#####……..………….########…….…………##########……………..###########…………….########.###…………….####.##…###…..………#####……###…..……..####……..###…..…….####………###…..…….###……….###…..……####……….###…..……###……….###…………###………####…………###……..###……..……###……####……………####..#######……………###########………..…….########………….……..#####…………………………………….……………………….……………………….……………………….0……………………….……………………….……………………….……………………….……………..####…….…………….######…………………#######……………….#########……………..###########…………….############……………#############…………..#####.####.###………….#####..###…##………….####………##………….##………..##…………###………..##………..####……….###………..####……….###………..####………###…….…..####…….####……..…..##############……………############……….……..########…………………######………….……………………….……………………….……………………….……………………….0……………………….……………………….……………………….……………………….……………………….…………..######……..…………..########……………….#########……………….#########………………##########…………….######..####…………….#####…####…………….#####…####…………….#####..#####…………….####…#####……………####….####…….……..#####…#####…….……..#####…####……..……..#####..#####……..……..####..#####……………..##########……….……..#########………..……..#########………..………######………….……….#####………….……………………….……………………….……………………….1……………………….……………………….……………………….……………………….……………………….………………####…………………..#####…………………..#####………………….####……..……………#####……..……………####……………………####…………………..####……….………….####………..…………####……………………####…………………..####………….………..####………….……….####…………..……….####…………..………####……………………####…………………..####…………….……..####…………….………###…………….……………………….……………………….……………………….1……………………….……………………….……………………….……………………….…………###………….…………####……………………####……………………####……………………####……………………####……………………#####………..………….####………..………….####………..………….####………..………….####………..………….####………..………….####………..………….####………..………….#####……….………….#####……….…………..####……….…………..####……….…………..####……….……………###……….……………………….……………………….……………………….……………………….1……………………….……………………….……………………….……………………….……………………….………….##………….………….##………….………….##………….………….##………….………….###…………………….###…………………….###…………………….###…………………….###…………………….###…………………….###…………………….###…………………….###…………………….###……………………..###………..…………..##……………………..##……………………..###………..…………..##……………………..##………………………………….……………………….……………………….“`

The test sample is

“`10……………………….……………………….……………………….……………………….……………………….…………..#####………………….#######……..………..##########…….……….####.##.####……………####……####…..………###……..###…..……..####………##…..……..###……….##…..…….###………..##…..…….###………..##…..…….###………..##…..……####………..##…..……####……….###…..…….###……….###…..…….###………###…………..###…….###…….……..####….#####…….………##########……………….#########…………………#####………..……………………….……………………….……………………….“`

The distances between the test sample and each of thetraining sampes are 101, 120, 162, 174, 173, 162. Thek nearest neighbors belong to digits 0, 0, 0, 1, and1. The most common digit among these neighbors is 0,so we conclude (correctly) that the test sample isdigit 0.

## Using struct

One of the objectives of this assignment is to see if youknow how to use `struct`.

For this assignment, you must define and use two `struct`:

– a `struct digit` that encapsulates the 2D array thatrepresents the handwritten digit and its label,– a `struct neighbor` that encapsulates a neighboringdigit and the distance to it.

## Input/Output

Write a program digits that reads, from the standard input,the following:

– A positive integer n, corresponding to the number oftraining samples, then repeatedly read n handwrittendigits, containing:

– a label corresponding to the digit in the next image(a number between 0 – 9)– 28 lines of texts, consisting of ‘.’ and ‘#’ only,representing a handwritten digit

– followed by another positive integer m, corresponding tothe number testing samples, then repeatedly read mhandwritten digits, containing:

– a label corresponding to the digit in the next image(a number between 0 – 9)– 28 lines of texts, consisting of ‘.’ and ‘#’ only,representing a handwritten digit

Then prints, to the standard output, the following:

For each testing sample,

– print the digit it is labeled as, followed by a space,followed by the digit it is recognized as.

Finally, print a double value that corresponds to theaccuracy, i.e, the percentage of training testing samplescorrectly recognized.

We separated the training samples and testing samples intotwo files, so the usual way of redirecting a file into yourprogram does not work anymore (since you need two files).

The way to run your program is to do the following:

“`cat &lt;training samples&gt; &lt;testing samples&gt; | ./digits“`

We use `cat` which concatenate two files into one to passboth the training samples and testing samples into theprogram using the pipe `|`.

The name convention for the output files is different forthis assignment. The name is formatted as X-Y.out where Xrefers to the training samples trainX.in and Y refers tothe testing samples testY.in.

## Sample Runs

“`<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="294646405e5d69594c181b18">[email protected]</a>:~/cs1010/as09$ cat inputs/train6.in inputs/test1.in | ./digits0 0100.0000<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5c3333352b281c2c396d6e6d">[email protected]</a>:~/cs1010/as09$ cat inputs/train100.in inputs/test30.in | ./digits0 00 00 61 11 11 12 22 52 23 33 33 34 94 44 95 55 35 36 26 66 67 77 77 78 88 18 89 99 99 973.3333“`