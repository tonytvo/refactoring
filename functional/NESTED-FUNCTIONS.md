- before
```
// Adds one to each element in the list
function addOne(list) {...}
// Duplicates each element in the list
function duplicate(list) {...}
// Squares each element in the list
function square(list) {...}
// Call the list of functions in order
square(duplicate(addOne([1, 2, 3])))
```

- after
```
const addOne = (x) => x + 1;
const duplicate = (x) => x * 2;
const square = (x) => x * x;
[1, 2, 3].map(addOne).map(duplicate).map(square);
```

i.e.:
![lambda separate as map]()
