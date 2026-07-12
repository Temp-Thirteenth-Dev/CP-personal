# TLE Sheet 31- 900

## [1883B - Chemistry](https://codeforces.com/contest/1883/problem/B)

<pre class="language-cpp"><code class="lang-cpp">#include &#x3C;iostream>
using namespace std;

int main() 
{
    ios::sync_with_stdio(false);
    cin.tie(0);

    int T;
    cin >> T;
    while(T--){
        int n,k;
        cin >> n >> k;
        bool chars[26];
        string strng;
        cin >> strng;

<strong>        for(int i=0;i&#x3C;26;i++){chars[i] = 0;} // forgot this first and shit happend.
</strong>
        for(char ch : strng){
            chars[ch-'a']=!chars[ch-'a'];
        }
        int parity_sum = 0;

        for(int i=0;i&#x3C;26;i++){
            parity_sum+=chars[i];
        }
        
        if(parity_sum > k+1 ){cout &#x3C;&#x3C; "NO" &#x3C;&#x3C; endl;}
        else {cout &#x3C;&#x3C; "YES" &#x3C;&#x3C; endl;}

    }

}
</code></pre>

thought its `parity_sum == k+1 || parity_sum == k`  But the test cases like these below failed!

```
1
3 2
hgh
```

From editorial :arrow\_down\_small:

{% hint style="info" %}
In our problem, it is sufficient to check that the number of letters with odd occurrences (denoted as $$x$$) is not greater than $$k+1$$. Let's prove this fact.

If $$x>k+1$$, then it is definitely impossible to obtain the answer, because with $$k$$ operations we cannot make the number of letters with odd occurrences not greater than $$1$$. On the other hand, we can simply remove the character with an odd number of occurrences on each removal iteration and decrease the number of odd occurrences. If there are no such characters, we can choose any character and remove it, thus having $$1$$ character with an odd occurrence.
{% endhint %}

***









***

***



## END CARD

***



