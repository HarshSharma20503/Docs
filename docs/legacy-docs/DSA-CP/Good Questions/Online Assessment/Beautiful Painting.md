There are _n_ pictures delivered for the new exhibition. The _i_-Th painting has beauty _a__i_. We know that a visitor becomes happy every time he passes from a painting to a more beautiful one.

We are allowed to arranged pictures in any order. What is the maximum possible number of times the visitor may become happy while passing all pictures from first to last? In other words, we are allowed to rearrange elements of _a_ in any order. What is the maximum possible number of indices _i_ (1 ≤ _i_ ≤ _n_ - 1), such that _a__i_ + 1 > ai.

**Input**
The first line of the input contains integer _n_ (1 ≤ _n_ ≤ 1000) — the number of painting.

The second line contains the sequence _a_1, _a_2, ..., _a__n_ (1 ≤ _a__i_ ≤ 1000), where _a__i_ means the beauty of the _i_-Th painting.

**Output**
Print one integer — the maximum possible number of neighbouring pairs, such that ai + 1 > ai, after the optimal rearrangement.

**Examples**
input
5  
20 30 10 50 40  

output
4  

input
4  
200 100 100 200  

output
2  

Note
In the first sample, the optimal order is: 10, 20, 30, 40, 50.
In the second sample, the optimal order is: 100, 200, 100, 200.

```cpp
 #include <bits/stdc++.h>
 using namespace std;
 map<int,int> frequency;
 int main(){
	int n;
	cin>>n;
	int c=0;
	for(int i=0;i<n;i++)
	{
	   int x; cin>>x;
	   frequency[x]++;
	   c=max(c,frequency[x]);
	}
	cout<<n-c<<endl;
 }
```


https://codeforces.com/problemset/problem/651/B

#shreya-oa #oa #codeforces 