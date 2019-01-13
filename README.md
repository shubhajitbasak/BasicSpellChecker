# BasicSpellChecker
This task will involve in the creation of a spellchecking system and an evaluation of its performance.

The training labelled data is in  holbrook.txt

The file consists of lines of text, with one sentence per line. Errors in the line are marked with a | as follows

My siter|sister go|goes to Tonbury .
In this case the word 'siter' was corrected to 'sister' and the word 'go' was corrected to 'goes'.

In some places in the corpus two words maybe corrected to a single word or one word to a multiple words. This is denoted in the data using underscores e.g.,

My Mum goes out some_times|sometimes .
Here we are not treating these as separate words instead we treat them like a single token.

Here we are using NLTK Library functions to build our spell checker model  

Steps are as follows  
- build a parser that can read all the lines of the file holbrook.txt and print out for each line the original (misspelled) text, the corrected text and the indexes of any changes. The indexes refers to the index of the words in the sentence.
-  calculate the frequency (number of occurrences), ignoring case, of all words and their unigram probability from the corrected training sentences
- Build a function that calculates all words with minimal edit distance to the misspelled word. Steps are as follows:

    1. Collect the set of all unique tokens in train
    2. Find the minimal edit distance, that is the lowest value for the function edit_distance between token and a word in train
    3. Output all unique words in train that have this same (minimal) edit_distance value  

- Create a function that takes a (misspelled) sentence and returns the corrected version of that sentence. The system should scan the sentence for words that are not in the dictionary (set of unique words in the training set) and for each word that is not in the dictionary choose a word in the dictionary that has minimal edit distance and has the highest unigram probability.  

## Improve the Model 

Corrective Approach
To improve the performance of the classifier I have gone through the following options :

**Add One Smoothing** - This adds up one to every word count so that it will not make the probablity as zero, but in our classifier we are using get_candidate method which will always return a value so add one smoothing will not improve the performance.  
**N - Gram Approximation** - in place of using unigram this approach will use n-grams i.e. considering n-1 previous tokens to calculate the probablities and passing the n-gram tokens to get_candidates to get the nearest match, but as the corpus is too small using bigrams or trigrams will not improve the performance  
**Parts Of Speech Tagging** - This method tags the corresponding Part Of Speech to the words of a sentence. As the classifier is trying to predict word by word its not considering the contextual data. So incorporate the context of words in the sentence we can consider the POS tag of the word in the sentence and ignore the Proper Nouns, Proper Pronouns, Cardinal Numbers while passing the sentence through classifiers. We will consider this as an improvement approach.  
**Stemming** - Stemming will return the root of a word. As the classifier is only predicting on the basis of the minimal distance from the words present in the train corpus it may change the word which is spelled correctly and is used in correct context just because the word is not present in the Training Corrected Corpus. So we can ignore those words whose stem is in the dictionary it will prevent some of the changes mentioned previously.  

So as per the options mentioned above I followed the following additional steps -

- Pass the sentence through the NLTK Parts Of Speech Tagging
- Identify the Proper noun, singular (NNP) or Proper noun, plural (NNPS) or Cardinal number (CD)
- Ignores the words with the NNP, NNPS, CD while passing the words through the classifier
- Check the stems of the words in the NLTK Dictionary, if the root is present in the dictionary ignore the words from the classifier
