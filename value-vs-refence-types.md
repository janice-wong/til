# Value Types vs Reference Types in C#

### Value Types
- These are primitive types like `bool`s, `int`s, or custom `struct`s. These are small and at most 8 bytes.
- This is why when value type variables are created, they're automatically allocated on _stack_, a part of memory.
- When they go out of scope, they removed from the stack at runtime.

```c#
var a = 10;
var b = a;					// b is copying the value of a, which is 10
a++;						// a becomes 11, but b stays as 10

Equals(a, b);					// returns true because their values are the same
ReferenceEquals(a, b);				// returns false bcause their references are different
                            			// a and b are pointing to different addresses in memory
```

### Reference Types
- These are `string`s, `Array`s, and custom classes.
- Unlike value types, creating reference type variables (or creating instances of these classes) is telling runtime to allocate memory to this object. This memory is allocated on _heap_ (another part of memory).
- When they go out of scope, they don't get removed from memory. The removal only happens during a process known as garbage collection.

```c#
var array1 = new[] {0, 1, 2};		// array1 points to some memory address
var array2 = array1;			// array2 points to the same memory address
array2[0] = 1;				// modifying array2 modifies the array stored in that memory address so now both array1 and array2 are [1, 1, 2];
ReferenceEquals(array1, array2);	// returns true because they point to the same memory address

array2 = new[] {3, 4, 5};		// array2 now creates a new instance of Array and points to new memory address
ReferenceEquals(array1, array2);	// returns false
```

[Source](https://www.udemy.com/course/csharp-tutorial-for-beginners/)
