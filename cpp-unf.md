---
description: Unfamiliar concepts in C++
---

# CPP Unf

## The stringstream

include the header `<sstream>`

**Compress string Question :**

<pre class="language-c++"><code class="lang-c++">#include &#x3C;iostream>
using namespace std;
#include &#x3C;string>
<strong>#include&#x3C;sstream>
</strong>int main(){
    string inp;
    cin >> inp;
    
<strong>    stringstream ss;
</strong>    
    // ss &#x3C;&#x3C; inp[0];
    int curr_count = 1;
    
    int lenght = inp.size();
    
    for(int i = 1; i&#x3C;lenght ; i++){
        if(inp[i-1]==inp[i]){
            curr_count++;
            continue;
        }
        ss &#x3C;&#x3C; inp[i-1];
<strong>        ss &#x3C;&#x3C; curr_count;
</strong>        curr_count = 1;
    }
        
    ss &#x3C;&#x3C; inp[lenght-1];
    ss &#x3C;&#x3C; curr_count;
    
<strong>    cout &#x3C;&#x3C; ss.str();
</strong>    
    
}
</code></pre>

Can also easily convert int to string and str to int.

## \`\r\` escape char

**Print a Progress bar - updates inplace**

```cpp
#include<>

int main() {
     int s = 0,count = 0;
      for(int j = 0; j < 10; j++,count+=10 ) { 
          for(int i = 0; i < 100000000; i++ ){ 
              s += i; // Long running task! 
}
cout << '\r' <<count << "%  " ;
for(int temp =0; temp<=count; temp+=10){
    cout << "=";    
}
cout  << flush; // clear leftover + force update
}
}
```





***

***

## END Card
