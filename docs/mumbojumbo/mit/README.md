# My IT (MIT)

## Big-O

In computer science Big-O notation is used to classify algorithms according to how their running time or space requirements grow as the input size grows. Big Oh lets us describe that growth asymptotically (independent of the machine or the specific implementation) in the worst case.

| Complexity     | Running time     | Characteristic     | Explanation     |
| :------------- | :------------- |:------------- |:------------- |
| O(1)       | Constant       | set of operations, recursive call  | constant problem no matter what |
| O(log n)       | Logarithmic       | bisection search | break problem in half or portions as quickly as possible, reducing the size of the problem by a constant factor on each stage |
| O(n)       | Linear       | loops, recursive calls | in general doing things a number of times based on the size of the problem |
| O(n log n)       | Log-Linear       | \* | \* |
| O(n^c)       |  Polynomial       | nested loops (c = 2), recursive calls | c is a constant e.g. O(n^2) is quadratic complexity/running time |
| O(c^n)       | Exponential       | recursive function | breaking the problem down into multiple calls each time around (calls up more than once in each call to itself) |

Footnote: Bear in mind that the cost of sorting becomes irrelevant in case of many searches, because the sort time is divided over many searches. In case of a unsorted list the search for an element is linear O(n) but in case of a sorted list it is logarithmic O(log n). The question becomes: when is the cost of the sort plus the cost of a number of logarithmic searches less then the cost of the same number of linear searches? Or: `SORT + K*O(log n) < K*O(n)`

## Search

| Search     | Characteristic     |Complexity     |
| :------------- | :------------- |:------------- |
| Brute force     | aka British Museum, list does not have to sorted just look at all on them        | O(n) |
| Bisection     | list must be sorted     | O(log n) |

## Sort

| Sort     | Characteristic     |Complexity     |
| :------------- | :------------- |:------------- |
| Monkey     | aka Bogo, Stupid, Slow, Permutation, Shotgun. Randomly put elements in a list and ask if they are sorted, if not repeat       | O(?) unbounded |
| Bubble     | compare consecutive pairs of elements, swap if needed, repeat until sorted (moving highest number to last position, repeat) Compare 1/2, 2/3, 3/4 and so on     | O(n^2) |
| Selection     | select smallest element and put in place 0, repeat with remainder of list to position +=1  | O(n^2) |
| Merge     | break problem into smaller problems  | O(n log n) |

## Indirection

In computer programming, indirection (also called "dereferencing") is the ability to reference something using a name, reference, or container instead of the value itself. The most common form of indirection is the act of manipulating a value through its memory address. For example, accessing a variable through the use of a pointer.

- By Value
- By Reference