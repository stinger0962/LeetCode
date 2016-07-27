# ZigZag Conversion

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R```
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:

```string convert(string text, int nRows);```


convert```("PAYPALISHIRING", 3)``` should return ```"PAHNAPLSIIGYIR".```

To our clarification, when the number of rows is 2, and the string is ABCD, we should return ACBD
```
A  C
B  D
```


---

Before we start, let's have a look into another case with nRow of 4 to make sure we understand the pattern.

convert```("GOODMORNINGYAHOO", 4)``` should return ```"GRAOONYHOMIGODNO".```
```
G     R     A
O   O N   Y H
O M   I G   O
D     N     O```

The pattern is clear by now. 

The first nRow chars form a straight column. 

The next nRow-2 chars form a ladder like shape. 

The next nRow chars form form a straight column again.

...