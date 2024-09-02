## Definition

A modular multiplicative inverse of an integer a is an integer x such that a.x is congruent to 1 modular some modulus m. To write it in a formal way: **we want to find an integer x** so that

$$
a.x ≅ 1 (mod\space m)
$$

We also denote x simply with a<sup>-1</sup>.

We should note that modular inverse does not always exist. It can be proven that the modular inverse exists if and only if a and m are relatively prime (i.e. gcd(a, m) = 1).


## Finding Modular Inverse using Extended Euclidean algorithm

$$
a.x + m.y = 1
$$
This is linear Diophantine equation in two variables. When gcd(a, m)  = 1, the equation has a solution and can be found using extended Euclidean algorithm. Note that gcd(a, m) = 1 is also the condition for the modular inverse to exist.

Now, if we take modulo m of both sides, we can get rid of m.y and the equation become
$$
a.x ≅ 1 (mod\space m)
$$
Thus, the modular inverse of a is x.

## Implementation

### Brute 

```cpp
#include <bits/stdc++.h>
using namespace std;

int modInverse(int A, int M)
{
    for(int X = 1; X < M; X++)
        if (((A % M) * (X % M)) % M == 1)
            return X;
}

int main()
{
    int A = 3, M = 11;
    cout << modInverse(A, M);
    return 0;

}
```

$Time \space Complexity - O(M)$
$Space \space Complexity - O(1)$

### A and M are co-prime (GCD(A, M) = 1)

```cpp
using namespace std;
int gcdExtended(int a, int b, int &x, int &y) {
    if (a == 0) {
        x = 0;
        y = 1;
        return b;
    }

    int x1, y1;
    int gcd = extendedGCD(b % a, a, x1, y1);

    x = y1 - (b / a) * x1;
    y = x1;

    return gcd;
}
void modInverse(int A, int M)
{
	int x, y;
	int g = gcdExtended(A, M, &x, &y);
	if (g != 1)
		cout << "Inverse doesn't exist";
	else {
		int res = (x % M + M) % M;
		cout << "Modular multiplicative inverse is " << res;
	}
}
int main()
{
	int A = 3, M = 11;
	modInverse(A, M);
	return 0;
}
```

$Time \space Complexity - O(log \space m)$
$Space \space Complexity - O(log \space m)$

### Modular multiplicative inverse when M is prime

```cpp
#include <iostream> 
using namespace std; 
int power(int base, int exponent, int mod) 
{ 
	int result = 1; 
	while (exponent > 0) 
	{ 
		if (exponent % 2 == 1) 
			result = (result * base) % mod; 
		exponent >>= 1; 
		base = (base * base) % mod; 
	} 
	return result; 
} 
int modInverse(int a, int m) 
{ 
	a = (a % m + m) % m; 
	return power(a, m - 2, m); 
} 
int main() 
{ 
	int a, m; 
	cin >> a >> m; 
	int result = modInverse(a, m); 
	cout << (result == 0 ? "No modular inverse" : to_string(result)) << endl; 
	return 0; 
}
```

$Time \space Complexity - O(log \space m)$
$Space \space Complexity - O(log \space m)$


## References 
https://cp-algorithms.com/algebra/module-inverse.html
https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/

## Tags

#modulo-inverse #modulo #inverse #cp-concept #cp