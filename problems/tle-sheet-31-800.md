# TLE Sheet 31 - 800

## 1881A. Don't Try to Count

{% embed url="https://codeforces.com/problemset/problem/1881/A" %}

### The straight forward Brute Force

```c++
#include <iostream>
#include <string>

using namespace std;

int main() {
    // Optimize standard I/O operations for faster execution
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int t;
    if (cin >> t) {
        while (t--) {
            int n, m;
            cin >> n >> m;
            string x, s;
            cin >> x >> s;

            int result = -1;
            
            // Since n and m are small, the required length to contain s is roughly n + m.
            // Because we double the string each time, we won't need more than ~10 operations
            // to reach a sufficient length (2^10 = 1024 >> 50).
            for (int ops = 0; ops <= 10; ++ops) {
                // Check if s is a substring of x
                if (x.find(s) != string::npos) {
                    result = ops;
                    break;
                }
                // Perform operation: append x to itself
                x += x;
            }
            
            cout << result << endl;
        }
    }
    return 0;
}
```

### Approach 2 : Looping in the x itself instead of concatenation

-> May be unnecessary as the constraints are very nominal, But for FUN!

```c++
#include<iostream>
using namespace std;
#include<string>
#include<bits/stdc++.h>
int main(){
    int t;
    cin >> t;
    while(t--){
        string x,s;
        int n,m;
        cin >> n >> m;
        cin >> x;
        cin >> s;
        int x_mark=0,s_ptr=0,x_ptr=0;
        int cycles_count=0;
        bool possible= false;

        while(x_mark<n && possible==false ){
          // cout << "here1\t";
            x_ptr = x_mark;
            s_ptr = 0;
            while(x[x_ptr] == s[s_ptr]){
                // cout << " \nhere2\t";
                // cout << "\n comparing :" << x_ptr << s_ptr;

                x_ptr++;s_ptr++;
                if(s_ptr==m){
                    //found it!
                    possible = true;
                    break;
                }
                if(x_ptr == n){
                    //wrap-around
                    cycles_count++;
                    x_ptr=0;
                    // cout << " here3\t";

                }
            }
            x_mark++;
        }
        if(possible){
            int req_len = cycles_count*n;
            int result = 0;
            while(n<=req_len){
              // cout << req_len;
                n = 2*n;
                result++;
            }
            cout << result << endl;
        }
        else{
            cout << "-1" << endl;
        }

    }
}
```

***

{% hint style="danger" %}
But forgot to update the cycles\_count. The real bug is that `cycles_count` is **not reset for each starting position i.e new x\_mark**.

```cpp
#include<iostream>
using namespace std;
#include<string>
#include<bits/stdc++.h>
int main() {
    int t;
    cin >> t;
    while (t--) {
        string x, s;
        int n, m;
        cin >> n >> m;
        cin >> x;
        cin >> s;
        int x_mark = 0, s_ptr = 0, x_ptr = 0;
        int cycles_count = 0;
        bool possible = false;

        while (x_mark < n && possible == false ) {
            // cout << "here1\t";
            x_ptr = x_mark;
            s_ptr = 0;
            cycles_count = 0; // UPDATED Here
            while (x[x_ptr] == s[s_ptr]) {
                // cout << " \nhere2\t";
                // cout << "\n comparing :" << x_ptr << s_ptr;

                x_ptr++; s_ptr++;
                if (s_ptr == m) {
                    //found it!
                    possible = true;
                    break;
                }
                if (x_ptr == n) {
                    //wrap-around
                    cycles_count++;
                    x_ptr = 0;
                    // cout << " here3\t";

                }
            }
            x_mark++;
        }
        if (possible) {
            int req_len = cycles_count * n;
            int result = 0;
            while (n <= req_len) {
                // cout << req_len;
                n = 2 * n;
                result++;
            }
            cout << result << endl;
        }
        else {
            cout << "-1" << endl;
        }

    }
}
```
{% endhint %}

#### 🧠 Approach: Simulating Repeated String Matching

We are given two strings `x` and `s`. The goal is to determine the **minimum number of times we need to double `x`** (i.e., `x = x + x`) such that `s` becomes a substring of the resulting string.

***

#### 🔍 Key Idea

Instead of explicitly building large strings by doubling, we simulate the process by treating `x` as a **cyclic string** and try to match `s` starting from every possible index in `x`.

***

#### ⚙️ Steps

1. **Try all starting positions in `x`:**
   * For each index `x_mark` from `0` to `n-1`, attempt to match `s`.
   * Update the varaibles
2. **Match `s` character by character:**
   * Use two pointers:
     * `x_ptr` → moves over `x`
     * `s_ptr` → moves over `s`
   * If characters match, advance both pointers.
3. **Handle wrapping (cyclic behavior):**
   * If `x_ptr` reaches end (`n`), wrap it to `0` and increment a **local cycle counter**.
   * This simulates repeating `x`.
4. **If full match of `s` is found:**
   * Compute the **minimum number of doublings** required so that total length of `x` after doubling ≥ required matched length.
5. **If no match is found for any starting position:**
   * Output `-1`.

***

#### 📌 Important Details

*   The total required length depends on:

    ```
    needed_length = starting_index + length_of_s
    ```
*   Then find minimum `k` such that:

    ```
    n * (2^k) >= needed_length
    ```

***

#### ⏱️ Complexity

* Matching attempt: `O(n * m)` in worst case
* No actual string building → memory efficient

***

#### 💡 Insight

This approach avoids repeatedly constructing large strings and instead simulates the process using pointer movement and wrap-around logic.

***



