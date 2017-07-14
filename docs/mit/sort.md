### Sort
| Sort     | Characteristic     |Complexity     |
| :------------- | :------------- |:------------- |
| Monkey     | aka Bogo, Stupid, Slow, Permutation, Shotgun. Randomly put elements in a list and ask if they are sorted, if not repeat       | O(?) unbounded |
| Bubble     | compare consecutive pairs of elements, swap if needed, repeat until sorted (moving highest number to last position, repeat) Compare 1/2, 2/3, 3/4 and so on     | O(n^2) |
| Selection     | select smallest element and put in place 0, repeat with remainder of list to position +=1  | O(n^2) |
| Merge     | break problem into smaller problems  | O(n log n) |
