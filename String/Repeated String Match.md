[Problem Link](https://leetcode.com/problems/repeated-string-match/description/)
### Problem Statement : 

Given two strings `a` and `b`, return _the minimum number of times you should repeat string_ `a` _so that string_ `b` _is a substring of it_. If it is impossible for `b`​​​​​​ to be a substring of `a` after repeating it, return `-1`.

**Notice:** string `"abc"` repeated 0 times is `""`, repeated 1 time is `"abc"` and repeated 2 times is `"abcabc"`.

**Example 1:**

```
Input: a = "abcd", b = "cdabcdab"
Output: 3
Explanation: "abcdabcdabcd" contains "cdabcdab"
```

### Approach 1 :

1. **Base calculation**:
    `repeat_count = ceil(len(b) / len(a))`
2. **Check**:
    - Repeat `a` exactly `repeat_count` times → check if `b` is substring.
    - If not, repeat `a` one more time (`repeat_count + 1`) → check again.
3. **Return**:
    - If found in either case → return the repeat count.
    - Else return `-1`.

#### Code :


``` cpp
int repeatedStringMatch(string a, string b) {
	int count = ceil((double)b.size() / a.size());
	string repeated = "";
	
	// First try: count repeats
	for (int i = 0; i < count; i++) repeated += a;
	if (repeated.find(b) != string::npos) return count;

	// Second try: count + 1 repeats
	repeated += a;
	if (repeated.find(b) != string::npos) return count + 1;

	// Impossible
	return -1;
}
```

> `Time Complexity` : O(n * m ) in worst case ,Using `string::find()` (KMP inside STL) makes it efficient.
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

- **Rabin–Karp** Matching
- Compute hash of `pattern` (`b`) and initial window of `text` (repeated `a`).
- Slide the window and update hash in O(1) using rolling hash.
- On hash match, confirm by direct substring comparison to avoid collisions.

#### **Hash Details**

- **Base**: 256 (covers extended characters)
- **Modulus**: `1e9 + 7` (large prime to reduce collisions)
- **Hash formula**:
    `hash(s[0...n-1]) = Σ ( (s[i] - 'a') * base^(n-i-1) ) % mod`
- **Rolling hash update**:
    `remove left char's contribution multiply hash by base add new right char`

#### Code :

```cpp
class Solution {
public:

    int base = 256;
    int mod = 1e9+7;

    long long poww(long long a,long long b){

        long long res = 1;
        while(b>0){

            if(b&1){
            res = (res*a)%mod;
            }
            a = (a * a)%mod;
            b = b >> 1;
        }

        return res;  

    }


    bool match(string &text,string &pattern){

        

        long long pHash = 0, tHash = 0;
        int pn = pattern.size();
        int tn = text.size();

        if(pn > tn)
        return false;

        for(int i=0;i<pn;i++){
            pHash = (pHash + ((long long)(pattern[i]-'a') * poww(base,pn-i-1)) % mod) % mod;
            tHash = (tHash + ((long long)(text[i]-'a')*poww(base,pn-i-1)) % mod) % mod;
        }

        if(pHash==tHash){
            if(text.substr(0,pn) == pattern)return true; // collison check
        }


        for(int i=pn;i<tn;i++){
            
            // add ith char in hash and remove (i-pn) char

            tHash = (tHash - (text[i-pn]-'a')*poww(base,pn-1)%mod + mod)%mod;
            tHash = (tHash * base)%mod;
            tHash = (tHash + ((long long)(text[i]-'a')*poww(base,0))%mod)%mod;

            if(pHash==tHash){
                if(text.substr(i-pn+1,pn) == pattern)return true; // collison check
            }

        }
        return false;

    }


    int repeatedStringMatch(string a, string b) {
        int count = ceil((double)b.size() / a.size());

        string repeated = "";
        for(int i=0;i<count;i++)
        repeated+=a;

        if(match(repeated,b))
        return count;

        repeated+=a;
        if(match(repeated,b))
        return count+1;

        return -1;
    }
};
```


> `Time Complexity` : O(N), where N = length of repeated string 
> 
> `Space Complexity` : O(1) 

---