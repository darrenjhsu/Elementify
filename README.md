# Elementify
Convert words to periodic table look

![alt text](https://github.com/darrenjhsu/Elementify/blob/main/BaNaNaS.png "BaNaNaS")

See the notebook and the examples therein!


### Technical rant

It is actually not that trivial when you look at a word and try to figure out all ways of dividing the characters into element symbols.
Here's how I approached it:

1. If there is a character which is not an element symbol, can't form an element symbol with previous or next character, then this word has no chance of being elementified. In this case, return nothing.
1. All symbols are at most two characters long. Therefore, for words that have a chance of being converted, we are placing division bars between the characters. For example, for the word "BARNS", we can place the bars like "B|A|R|N|S|". This would mean the lengths of divided characters are 11111 (which is invalid). Other ways of doing so include "B|AR|N|S|", which corresponds to 1211, or "BA|RN|S|" (221). Both are valid divisions.
1. It then becomes a game of where you place the 2's in that string. You could do 221 (BA|RN|S|), 212 (BA|R|NS|), or 122 (B|AR|NS|) if you have two 2's. I wrote a permutationWithoutRepeat function to do that. Using Python itertools's permutation breaks down at length 10 or so as it is O(N!) complexity. 
1. The most 2's you can place is half length (or length - 1 if length is odd) of the string. Looping through the number of 2's gives you exhaustive listing of possible ways to divide the string. By the way, the number of ways to divide as string length grows is exactly the Fibonacci sequence, 1, 1, 2, 3, 5, 8, ...
1. The rest is easy - just check the character chunks against the element symbol set.
1. Drawing is done with matplotlib and may display differently on your computer.
