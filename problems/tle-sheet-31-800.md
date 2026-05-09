# TLE Sheet 31 - 800

## 1881A. Don't Try to Count

3 May 2026

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

> May be unnecessary as the constraints are very nominal, But for FUN!

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

{% hint style="danger" %}
But forgot to update the cycles\_count. The real bug is that `cycles_count` is **not reset for each starting position i.e new x\_mark**.

<pre class="language-cpp"><code class="lang-cpp">#include&#x3C;iostream>
using namespace std;
#include&#x3C;string>
#include&#x3C;bits/stdc++.h>
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

        while (x_mark &#x3C; n &#x26;&#x26; possible == false ) {
            // cout &#x3C;&#x3C; "here1\t";
            x_ptr = x_mark;
            s_ptr = 0;
<strong>            cycles_count = 0; // UPDATED Here
</strong>            while (x[x_ptr] == s[s_ptr]) {
                // cout &#x3C;&#x3C; " \nhere2\t";
                // cout &#x3C;&#x3C; "\n comparing :" &#x3C;&#x3C; x_ptr &#x3C;&#x3C; s_ptr;

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
                    // cout &#x3C;&#x3C; " here3\t";

                }
            }
            x_mark++;
        }
        if (possible) {
            int req_len = cycles_count * n;
            int result = 0;
            while (n &#x3C;= req_len) {
                // cout &#x3C;&#x3C; req_len;
                n = 2 * n;
                result++;
            }
            cout &#x3C;&#x3C; result &#x3C;&#x3C; endl;
        }
        else {
            cout &#x3C;&#x3C; "-1" &#x3C;&#x3C; endl;
        }

    }
}
</code></pre>
{% endhint %}

#### 🧠 Approach: Simulating Repeated String Matching

We are given two strings `x` and `s`. The goal is to determine the **minimum number of times we need to double `x`** (i.e., `x = x + x`) such that `s` becomes a substring of the resulting string.

#### 🔍 Key Idea

Instead of explicitly building large strings by doubling, we simulate the process by treating `x` as a **cyclic string** and try to match `s` starting from every possible index in `x`.

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

#### 📌 Important Details

*   The total required length depends on:

    ```
    needed_length = starting_index + length_of_s
    ```
*   Then find minimum `k` such that:

    ```
    n * (2^k) >= needed_length
    ```

#### ⏱️ Complexity

* Matching attempt: `O(n * m)` in worst case
* No actual string building → memory efficient

#### 💡 Insight

This approach avoids repeatedly constructing large strings and instead simulates the process using pointer movement and wrap-around logic.

> Z AI analyzed it well ; But couldn't trace out the fault.
>
> ChatGPT also couldn't at first; but with the `thinking` mode enabled, it found it out!!!

***



## 1877A : Goals of Victory

{% embed url="https://codeforces.com/problemset/problem/1877/A" %}

\#silly-mistake

3 May 2026

```c++
#include<iostream>
using namespace std;

int main(){
    int t;
    cin >> t;
    while(t--){
        int n,sum=0,curr;
        cin >> n;

        while(n--){
          cin  >> curr;
          sum+= curr;
        }
        sum = -sum;
        std::cout << sum<< std::endl;

    }
}
```

{% hint style="warning" %}
Read the input format carefully.

The first line of an individual test case `n` is the no.of teams and the next line of input contains `n-1` ints not `n` ints.

<pre class="language-c++"><code class="lang-c++">#include&#x3C;iostream>
using namespace std;

int main(){
    int t;
    cin >> t;
    while(t--){
        int n,sum=0,curr;
        cin >> n;
<strong>        n--; //UPDATED
</strong>        while(n--){
          cin  >> curr;
          sum+= curr;
        }
        sum = -sum;
        std::cout &#x3C;&#x3C; sum&#x3C;&#x3C; std::endl;

    }
}
</code></pre>
{% endhint %}

***

## 1862B : Sequence Game&#x20;

4 May 2026

{% embed url="https://codeforces.com/problemset/problem/1862/B" %}

#### First Trail

```cpp
#include<iostream>
#include<bits/stdc++.h>

using namespace std;

int main(){
    int t;
    cin >> t;
    while(t--){
        int n,m;
        cin >> n;
        m = 1+ (n-1)*2;
        n--;
        int temp,first_num;
        cin >> first_num;
        cout << m << endl;
        cout << first_num << ' ';

        while(n--){
            cout << first_num - 1 << ' ';
            cin >> temp;
            cout << temp << ' ';
        }
        cout << endl;
    }
}
```

{% hint style="warning" icon="face-frown-slight" %}
Silly Mistake

Check the output format correctly, It has range \[1,inf). \
But here, the `first_num - 1` could be `0`
{% endhint %}

```cpp
#include<iostream>
#include<bits/stdc++.h>

using namespace std;

int main(){
    int t;
    cin >> t;
    while(t--){
        int n,m;
        cin >> n;
        m = n;
        vector <int> arr(n);
        cin >> arr[0];
        for(int i = 1; i< n;i++){
            cin >> arr[i];
            if(arr[i]<arr[i-1]){
                m++;
            }
        }

        cout << m << endl;
        cout << arr[0] << ' ';

        for(int i=1;i<n;i++){
            if(arr[i]<arr[i-1]){
                cout << arr[i] << ' ';
            }
            cout << arr[i] << ' ';
        }

        cout << endl;

    }
}
```

***

## Count Binary strings

Count Binary\
Write a C/C++ program to recursively count the number of binary strings of length N containing exactly K ones\
such that no two ones are adjacent to one another.\
Example: If N = 4 and K = 2, then the desired number of binary strings will be 3. These strings are:\
1010\
0101\
1001\
Input Format:\
● The first line contains two positive integers N and K, separated by a space\
Output Format:\
● A single integer representing the number of binary strings of length n containing exactly k ones with no\
two ones adjacent to one another.\
Assumptions on Input :\
● Assume that the value of N entered by the user will be between 1 and 10, both inclusive\
● Assume that the value of K entered by the user will be between 1 and 10, both inclusive\
● If it is not possible to construct a binary string of this sort, print 0.\
Practice Testcases :\
Input : Output&#x20;\
4 2 : 3\
5 3 : 1\
6 5 : 0



```
// Actually straight forward (N-K+1) C k, But the question asked to implement recursively.
```

```
#include <iostream>
using namespace std;

// Actually straight forward (N-K+1) C k, But the question asked to implement recursively.

int solve(int n, int k, bool prevOne)
{
    // Exact number of ones used
    if (n == 0)
    {
        return (k == 0);
    }

    // Impossible cases
    if (k < 0)
        return 0;

    int count = 0;

    // Place 0
    count += solve(n - 1, k, false);

    // Place 1
    if (!prevOne)
    {
        count += solve(n - 1, k - 1, true);
    }

    return count;
}

int main()
{
    int N, K;
    cin >> N >> K;

    cout << solve(N, K, false);

    return 0;
}
```











***

***

## END CARD
