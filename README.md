## HMM and RNN that Generate Poems
Given the entire corpus of Shakespeare's sonnets, we trained a Hidden Markov Model and Recurrent Neural Network to generate Shakespearen sonnets and other types of poems!
Caltech CS155: Machine Learning and Data Mining (Winter 2024) - By Mia Mutadich, Lana Lubecke, Jena Alsup and Ava Penn.

**raw_data**

Contains raw data used for training:

* shakespeare.txt: all 154 Shakespeare's sonnets.
* spenser.txt: Amoretti written by Edmund Spenser in the 16th century. All 139 of Spenser’s sonnets in the Amoretti follow the same rhyme scheme and meter as Shakespeare’s sonnets.
* syllable_dictionary.txt: syllable count information in shakespeare.txt.
* syllable_dict_explanation.pdf: explanation of the file Syllable_dictionary.txt

# Pre-Processing

Sonnets are restricted to having ten syllables per line so we created a hashmap that maps dictionary words to their corresponding number of syllables by parsing syllable_dict.py. In the case of a word having a different number of syllables at the end of a line than in the middle of a line, we used solely latter becasue we figured the word would have a higher frequency of appearing in the middle of a line. We then cleaned the corpus of Shakespeare's sonnets to remove all formatting but maintain the order of the lines of the sonnets. We did the same to the Amoretti sonnets by Spenser to get cleaned Spenser's data.

**For Hidden Markov Model**

We tokenized the cleaned Shakespear data into words and verified that our words were valid using the syllable dictionary. We also tokenized the cleand Spenser's data into words, removing any words that were not included in the Shakespear dataset, so that we have more training data that follows the same sonnet pattern. We created a 2D array from the cleaned Shakespear data where each line of a sonnet is an array of the tokens of words that form a line in a sonnet and these arrays are ordered in a way that mirrors the line order of the sonnets.

**For Recurrent Neural Network**

We first tokenized the cleaned Shakespear data and into characters. Using the cleaned Shakespear data and given a window length of *n* characters and a step length of *s* characters, we created *input* and *output* tensors where the first dimension is the number of _n_ length sequences with step length _s_ that make up the sonnet corpus, the second dimension is the number of characters in the sequence and the third dimension is the one-hot vector representation of each character in the sequence. Since we are training the network to predict each subsequent character from the given context of previous characters, the *input* seqeunce will be the first *n - 1* characters in the *n* length sequence and the *output* sequence will be the last n-1* characters of that sequence.


# Hidden Markov Model
We used the Baum-Welch Algorithm, testing the model with 4,8,10 and 16 hidden states. When we tested models with more than 16 hidden states there was overfititng and whern we tested the model with less than 4 hidden staten the ouptut was unintelligible. When generating our sonnets, if a line had more than 10 syllables, we would keep regenerating it until it met this requirement. 

**Example Sonnet**

This poem was generated by our HMM with 16 hidden states:

```
supposed such of now so slave doth a
appetite am from that do add reckoning
seem self and like water still more cruel
seeing you reason eye your her thy fair

unused once time’s into knows not thine
honey smell of admit men’s why moods my
with some glazed mine rude thought to that though
dull it chide your pleasure night and lives thou

sin of thy comfort who of to give and
own love’s with feast those rose dateless to that
joy shame hath compare when faults vows happy
in were thou kingdoms that your hasten the

mind but the coward augurs with still my
for strength of crime shadows thee torn love no

```


# Recurrent Neural Network
We designed a naive RNN using PyTorch to produce Shakespearean Sonnets and tested its performance for step sizes of 10, 20 and 30 with a fixed window length of 40. 

**Example Sonnet**

```
weary with toil i haste me to my bed
the world that thou art as the proud love is strange
and then the eyes thee that thou art the world to thee
then thou art thee thou art

and therefore when thou art thee thou art thee thou art the stere and thee the earth thence thou art all the world with mine eyes
when i am sometime day the will of all true
and so should the beauty so suches me that thou mayst thou art
and then the self to thee the world to thee

the world to make the world to be to the will
to the tring and then should be thee is best in the stern
and therefore when thou art thee thou art thee thou art 
the stere and thee the earth thence thou art all the world with mine eyes

when i am sometime day the will of all true
and so should the beauty so suches me that thou mayst thou
```



