# TextFinder

This program searches for the occurrence of a word in all documents, simultaneously calculating the relevance.

Also used TF-IDF rank. TF means the ratio of the number of occurrences of a certain word to the total number of words in the document. And IDF is inversion of the frequency with which a certain word occurs in the collection documents.

# To use this program on Windows:

1. Clone or download the archive with code.
2. Use the Visual studio, and paste code from file "TextFinder.cpp".
3. Run the code.
4. 1st input string in program is the stop-words. These words aren't included in the query.
5. 2nd input is the n - number of your documents, it can be less than 5.
6. Next n strings are the documents(I will add the ability to enter from a file later).
7. And (n+1) string - you query(word, that you need to find in these documents).

Program will print the answer in format: document_id = a, relevance = b
Here a is the number of document in vector (a = 1...n), and b is relevance of query(in double type).


# The exapmle of input data.
cat dog
3
fat dog ate a big sasuage
funny bird is flying in the jungle
At most one grammar option must be chosen out of ECMAScript, basic, extended, awk, grep, egrep. If no grammar is chosen, ECMAScript is assumed to be selected.
grammar

# Output for the example
document_id = 2, relevance = 0.0813787

System for GNU/Linux wil be added later.
