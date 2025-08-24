`Binary` is the format used to store data on a computer. It consists of two boolean values (states): ==1== and ==0== (true and false respectively). Each individual 1 or 0 is stored as a `bit`. Binary uses the [[Number Systems#Base-2|base-2]] number system.

These binary values are like this to mimic how **logic gates** work inside of computer chips. ==1== represents the presence of an electrical charge, and ==0== indicates no charge.

`Bits` themselves aren't useful, but a sequence of them is, called a `byte`. Bytes are used to store numbers, letters, etc... on a computer.

A common number of bits to use is **8** (known as **8-bit**). With 8 bits, we can make 256 different patterns of ==1s== and ==0s==. In other words, one bye can store **256 different numbers** (0 - 255). 

How is this possible? Each position represents a different number. For each position that is 1, you **add the represented numbers** to get the value that `byte` represents.

![[binary.jpg|500]]

So for example:

```
00000001  = 1
00010100  = 20
00001111  = 15
```


## Addition

Compared to adding [[Number Systems#Base-10 (decimal)|decimal]] numbers together, adding binary numbers is overall simpler.  It just consists of the following rules:

![[binary_addition_rules.jpg]]

When adding, write the numbers such that their **respective place values are aligned**. Next, add the digits at ones place using binary addition rules.

If you have a carry, write a 0 and carry the 1 over to the next place value. Keep doing this until all have been added.

See [here](https://www.splashlearn.com/math-vocabulary/binary-addition) for more information.


# Characters

To store **characters,** one more tool is necessary: `encoding`. This is the process of representing characters using numbers. And since numbers can be represented using binary, it can also represent characters. 

Encoding is done using a **table** that maps characters to numbers.

## ASCII

`ASCII` (`A`merican `S`tandard `C`ode for `I`nformation `I`nterchange) encoding, published in 1963, was the first American standard for encoding primarily the English alphabet. Each number is stored in `1 byte`, making the table simple.

![[ascii.jpg|350]]

## UTF

The glaring issue with ASCII is that it primarily focuses on English. But what about other languages?

Enter `UTF` (`U`nicode `T`ransformation `F`ormat), a newer encoding system designed to represent all possible characters in one system. It is now the universal standard for encoding all human languages, and other symbols like emojis.

Like ASCII, unicode assigns a unique code, called a ==code point==, to each character. Unlike ASCII, unicode allows characters to be up to **32 bits** wide, or over 4 billion unique values. This is to make sure all possible characters can be encoded. 

`UTF-8` is the most common unicode encoding scheme used world-wide. It makes use of _variable length encoding_, meaning a character will only use as many bytes as it needs, no more, no less. This is very efficient memory wise. 

>[!example]
> `A` → `01000001` (1 byte)
> `Ω` → `11001110 10101010` (2 bytes)
> `😊` → `11110000 10011111 10011001 10001010` (4 bytes)

A unicode ==code point== is a number assigned to a character by the Unicode Standard. Code points are prefixed with ==U== and combined with a [[Number Systems#Base-16 (hexadecimal)|hexadecimal]] number. Some characters can also be represented by a combination of code points.

![[unicode.jpg|350]]



