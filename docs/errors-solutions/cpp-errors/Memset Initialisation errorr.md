## Summary

- `memset` can't be used to initialise `int` array with `1`, an array of `int` can only be initialised with `0` or `-1` using memset
---
## **Lessons Learned**

Therefore, `memset` cannot be used to initialise an `int` array with `1` because if an `int` is represented by 4 bytes, then `memset` will initialise each byte with `1`.

The value `16843009` is equivalent to `0x01010101`. Each of the 4 bytes is initialised with `01`.

Using `memset`, an array of `int` can only be initialised with `0` or `-1` because `0` and `-1` both have all bits `0` and `1` respectively in the two's complement binary representation, regardless of the size of the `int` data type.

---
## Reference URL

- [Stack Overflow](https://stackoverflow.com/questions/35839179/why-isnt-memset-assigning-1#:~:text=The memset() function fills,initialize each bytes with 1])
---
## Tags

#cpp #memset #initialisation 