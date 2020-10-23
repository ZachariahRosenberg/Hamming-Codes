# Hamming Codes

Hamming codes, [invented by Richard Hamming in 1950](https://en.wikipedia.org/wiki/Hamming_code), are a simple, scalable scheme to solve the problem of detecting whether bits of data have flipped, breaking the integrity of the file (and figuring out where the error occurred). 

This jupyter notebook works through implementing an "extended" hamming code for a 16 bit data structure.

Based on Grant Sanderson's video on Hamming Codes:
https://www.youtube.com/watch?v=X8jsijhllIA


## Discussion

For a 16 bit data structure, 11 bits are data and 5 bits are redudant error checking:

|-|0*|1*|1|
|-|-|-|-|
|0*|0 |1 |1|
|1* |0 |0 |1|
|1 |0 |1 |0|

\* indicates this is a *parity bit*, a bit responsible for error checking (as opposed to a data bit)

The top left most box contains the extended parity bit. We'll get to that later, but for now we'll ignore it.

### Parity Bits

---

A parity bit is responsible for counting the number of 1's in a specified section of the block and ensuring an even count of 1's. It changes its own value, 0 or 1, to ensure the count.

**Example**

Let's assume a 2x4 block, with the top-left most bit being our parity bit:

|?*|0|1|1|
|-|-|-|-|
|1|0|0|1|

There are 4 1's in the data, so our parity bit would assume the value of 0:

|0|0|1|1|
|-|-|-|-|
|1|0|0|1|

count of 1's: 4.

<br />

### Error checking on a 16 bit block
---

Back to our 16 bit block, we have 4 active parity bits and then an *extended parity* bit in the top-left most block. For now, let's focus on the 4 primary parity bits:

|-|0*|1*|-|
|- |- |- |-|
|0*|- |- |-|
|1* |- |- |-|
|- |- |- |-|

*For this tutorial, rows and columns are 0-index based. So the left most column is column 0 and the rightmost column is column 3.*

<br />

##### Column Parities

---

The first column parity bit, in row 0 col 1, Checks all *odd columns* (0-indexed):

|-|X*|-|\*|
|-|- |-|- |
|-|\*|-|\*|
|-|\*|-|\*|
|-|\*|-|\*|

The second column parity bit looks to the right half of cols:

|-|- |X*|\*|
|-|- |-|-|
|-|- |\*|\*|
|-|- |\*|\*|
|-|- |\*|\*|

**Example**

|-|?\*|?\*|1|
|-|-|-|-|
|1|0 |1 |1|
|1|0 |0 |1|
|1|0 |1 |0|

Let's start with the first column parity bit, which checks columns 1 & 3:
- 1's in col 1: 0
- 1's in col 3: 3
- total: 3
- **parity bit: 1**

The second parity bit checks columns 2 & 3
- 1's in col 2: 2
- 1's in col 3: 3
- total: 5
- **parity bit: 1**

**Solution**

|-|1*|1*|1|
|-|-|-|-|
|1|0|1|1|
|1|0|0|1|
|1|0|1|0|

<br />

##### Row Parities

---

Similar to column parities, the first row parity (in the second row, we're skipping the first box) reviews all *odd index rows* while the second row parity reviews the bottom half of rows:

odd row parity

|-  |- |- |- |
|-  |- |- |- |
|?\*|\*|\*|\*|
|-  |- |- |- |
|\* |\*|\*|\*|

bottom half parity

|-|-|-|-|
|-|- |-|-|
|-|-|-|-|
|?*|\*|\*|\*|
|\*|\*|\*|\*|


**Example**

|-|0|1|1|
|-|-|-|-|
|?\*|0 |1 |1|
|?\*|0 |0 |1|
|1|0 |1 |0|

Starting with the odd-row parity, checking rows 1 & 3:
- 1's in row 1: 2
- 1's in row 3: 2
- total: 4
- **parity bit: 0**

The second half row parity checks rows 2 & 3:
- 1's in row 2: 1
- 1's in row 3: 2
- total: 3
**parity bit: 1**

**Solution**

|-|0|1|1|
|-|-|-|-|
|0|0|1|1|
|1|0|0|1|
|1|0|1|0|
