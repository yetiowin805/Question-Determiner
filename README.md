# Question Extraction

* Overview:
    * For a given text, we split it up into sentences, and extract those that represent questions.
    * Input: A csv file with a series of review texts
    * Output: A csv file with only the questions from the input texts
* Algorithm:
    * Split text into sentences
    * For each sentence, determine whether it is a question
* Performance:
    * Low false negative rate, i.e. questions are nearly always correctly identified as such
    * Good, but slightly worse, false positive rate. A small portion of the output is typically statements that are similar to questions in some ways
    * These include certain long complex sentences, certain colloquial structures, and some highly ungrammatical sentences
    * Relatively slow, primarily due to parsing

## Question Determination
* Overview:
    * For a given sentence, we determine if it represents a question, including certain types of indirect questions.
    * Input: A sentence
    * Output: A boolean representing whether the sentence is a question
* Algorithm:
    * Questions are separated into two types: wh-questions, which are those that use a wh-word (who, what, where, etc.), and yn-questions, which expect a yes or no answer.
    * For each sentence it is determined whether it is either a wh-question or a yn-question

### Wh-questions
* Algorithm:
    * Find each instance of a wh-word
    * Make sure the wh-word is in the main clause and not a subordinate clause
    * This is done since the wh-words in English have dual interrogative and relative meanings
    * Relative clauses are subordinate to the main clause, so they can be identified
    * However, do include some subordinate clauses that are objects of certain verbs (ex. "I don't know why...")

### YN-questions
* Algorithm:
    * Check if first word in the sentence is an auxiliary verb
    * If so, check if the following phrase is a noun phrase that is the subject of the verb
    * This is done to exclude certain imperative sentences ("Do it!") where a verb starts a sentence, but is followed by an object (if it is followed by a noun phrase)
* Issues:
    * In colloquial language subjects are sometimes dropped
    * If the main verb is an auxiliary verb, and it is followed by a noun phrase, typically the direct object, the noun phrase will be falsely identified as the subject, causing the sentence to be falsely categorized as a question

## Future Steps

* Syntactic Structures
    * Syntactic structures identified as questions currently uses a rules-based approach
    * Accuracy could be improved by transitioning to a machine learning model
    * Instead of looking for certain set structures, the set of qualifying structures would be learned from a corpus
    * Would be much better adapted to colloquial language, grammatical mistakes, misparsings by the parser, as well as any possible domain-specific language
    * One model is described by Wang and Chua: Wang, Kai, and Tat-Seng Chua. "Exploiting salient patterns for question detection and question retrieval in community-based question answering." Proceedings of the 23rd International Conference on Computational Linguistics (Coling 2010). 2010.
    * Uses lexical and syntactic rules; F1 score is about 91%
* Rhetorical or sarcastic questions
    * Syntactic questions can be rheotical or sarcastic, which are not true inquiries
    * Topic modelling may be a solution to this; questions not assigned a meaningful topic can be assumed to be rhetorical or otherwise lacking in substance
    * Duan et al. describe a machine learning model that can detect sarcasm: Diao, Yufeng, et al. "A Multi-Dimension Question Answering Network for Sarcasm Detection." IEEE Access 8 (2020): 135152-135161.
    * Their model has an F1 score of around 70-75%