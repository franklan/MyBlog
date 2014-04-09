

This hash function is similar to a Linear Congruential Generator (LCG - a simple class of functions that generate a series of psuedo-random numbers), which generally has the form:

X = (a * X) + c;  // "mod M", where M = 2^32 or 2^64 typically

Note the similarity to the djb2 hash function... a=33, M=2^32. In order for an LCG to have a "full period" (i.e. as random as it can be), a must have certain properties:

a-1 is divisible by all prime factors of M (a-1 is 32, which is divisible by 2, the only prime factor of 2^32)
a-1 is a multiple of 4 if M is a multiple of 4 (yes and yes)
In addition, c and M are supposed to be relatively prime (which will be true for odd values of c).

So as you can see, this hash function somewhat resembles a good LCG. And when it comes to hash functions, you want one that produces a "random" distribution of hash values given a realistic set of input strings.

As for why this hash function is good for strings, I think it has a good balance of being extremely fast, while providing a reasonable distribution of hash values. But I've seen many other hash functions which claim to have much better output characteristics, but involved many more lines of code. For instance see this page about hash functions


http://stackoverflow.com/questions/10696223/reason-for-5381-number-in-djb-hash-function/13809282#13809282