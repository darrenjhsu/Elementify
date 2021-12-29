# Elementify
Convert words to periodic table look

![alt text](https://github.com/darrenjhsu/Elementify/blob/main/BaNaNaS.png "BaNaNaS")

See the notebook and the examples therein!


## Usage

```python
elementify('bananas', printing=True, verbose=True) # It's case insensitive
printWord(['Ha','Ku','Na','','Ma','Ta','Ta']) # Print any word you like
```


### Technical rant

It is actually not that trivial when you look at a word and try to figure out all ways of dividing the characters into element symbols.
Here's how I approached it:

1. If there is a character which is not itself an element symbol and can't form an element symbol with the previous or the next character, then this word has no chance of being elementified. This linear time check is useful in rejecting hopeless long words.
1. All symbols are at most two characters long. Therefore, for words that have a chance of being converted, we are placing division bars between the characters. For example, for the word "BARNS", we can place the bars like "B|A|R|N|S|". This would mean the length sequence of the divided characters is 11111 (which is invalid). Other ways of doing so include "B|AR|N|S|", which corresponds to 1211, or "BA|RN|S|" (221). Both are valid divisions.
1. It then becomes a game of where you place the 2's in that string. You could do 221 (BA|RN|S|), 212 (BA|R|NS|), or 122 (B|AR|NS|) if you have two 2's. Python's itertools's permutation breaks down at length 12 or so as it is O(N!) complexity. I wrote a permutationWithoutRepeat function to do that, which has O((N/2)!) complexity and breaks down at length 25+. Therefore, if you attempt to input a very long word like Pneumonoultramicroscopicsilicovolcanoconiosis, ~~it will be rejected by length.~~
1. The most 2's you can place is half length (or length - 1 if length is odd) of the string. Looping through the number of 2's gives you the exhaustive listing of possible ways to divide the string. By the way, the number of ways to divide as string length grows is exactly the Fibonacci sequence, 1, 1, 2, 3, 5, 8, ...
1. The new method provided by Yuan-Jia Fan is to first compile if a character can be a symbol itself or form one with the next character. There may be multiple characters than can do both. In this case one can exhaustively try the combinations by tracing through the word using the indices. This allows checking really long words unless someone abuses it with "NINININI...". (All "N", "I", "NI", and "IN" are symbols)
1. The rest is easy - just check the character chunks against the element symbol set.
1. Drawing is done with matplotlib and may display differently on your computer.
