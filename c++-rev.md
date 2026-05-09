# C++ Rev

## C++ `<algorithm>` Library Guide for Coding Interviews & CP

The `<algorithm>` library in C++ provides a collection of functions specifically designed to be used on ranges of elements (like arrays or vectors). Mastering these functions is crucial for solving Basic-Medium coding problems efficiently, as they save time, reduce bugs, and show interviewers your familiarity with the standard library.

### 1. Sorting Operations

#### `std::sort`

Sorts the elements in the range `[first, last)` in ascending order by default.

* **Time Complexity:** $$O(N \log N)$$ (Introsort - highly optimized).
*   **Usage:**

    ```
    vector<int> v = {4, 1, 3, 5, 2};
    sort(v.begin(), v.end()); // Result: {1, 2, 3, 4, 5}
    ```
*   **Custom Comparators:** You can sort in descending order or by custom logic using lambda functions.

    ```
    // Descending order using greater<int>()
    sort(v.begin(), v.end(), greater<int>()); 

    // Custom logic: Sort pairs by the second element
    vector<pair<int, int>> pairs = {{1, 4}, {3, 2}, {5, 1}};
    sort(pairs.begin(), pairs.end(), [](const pair<int, int>& a, const pair<int, int>& b) {
        return a.second < b.second; 
    });
    ```

#### `std::stable_sort`

Similar to `sort`, but preserves the relative order of elements with equivalent values.

* **Time Complexity:** $$O(N \log^2 N)$$ if not enough memory, $$O(N \log N)$$ otherwise.

### 2. Binary Search & Bounds (Crucial for Medium Problems)

_Note: The sequence MUST be sorted before using these functions._

#### `std::binary_search`

Returns `true` if an element exists, `false` otherwise.

*   **Time Complexity:** $$O(\log N)$$

    ```
    bool exists = binary_search(v.begin(), v.end(), 3);
    ```

#### `std::lower_bound`

Returns an iterator pointing to the **first element that is NOT LESS than (i.e.,** $$\ge$$**)** the target value.

*   **Time Complexity:** $$O(\log N)$$

    ```
    vector<int> v = {10, 20, 30, 30, 40};
    auto it = lower_bound(v.begin(), v.end(), 30);
    int index = distance(v.begin(), it); // index = 2
    ```

#### `std::upper_bound`

Returns an iterator pointing to the **first element that is STRICTLY GREATER than (i.e.,** $$>$$**)** the target value.

*   **Time Complexity:** $$O(\log N)$$

    ```
    auto it = upper_bound(v.begin(), v.end(), 30);
    int index = distance(v.begin(), it); // index = 4
    ```

> **Note on Set/Map:** If you are using a `std::set` or `std::map`, **do not** use `std::lower_bound(s.begin(), s.end(), val)`. It takes $$O(N)$$ time because sets use non-random-access iterators. Instead, use the member function: `s.lower_bound(val)`, which is $$O(\log N)$$.

### 3. Min/Max Operations

#### `std::min` & `std::max`

Finds the smaller/larger of two values, or of an initializer list.

*   **Time Complexity:** $$O(1)$$ or $$O(N)$$ for lists.

    ```
    int a = max(10, 20); // 20
    int b = min({10, 20, 5, 30}); // 5 (Initializer list)
    ```

#### `std::min_element` & `std::max_element`

Finds the smallest/largest element in a range. **Returns an iterator**, so you must dereference it using `*`.

*   **Time Complexity:** $$O(N)$$

    ```
    vector<int> v = {4, 1, 3, 5, 2};
    int max_val = *max_element(v.begin(), v.end()); // 5
    int min_val = *min_element(v.begin(), v.end()); // 1
    ```

### 4. Modifying Sequence Operations

#### `std::reverse`

Reverses the order of the elements in the range `[first, last)`.

*   **Time Complexity:** $$O(N)$$

    ```
    string s = "hello";
    reverse(s.begin(), s.end()); // "olleh"
    ```

#### `std::unique`

Eliminates all except the first element from every consecutive group of equivalent elements.

* **Crucial Note:** It **only removes adjacent duplicates**. To remove all duplicates, you MUST sort the vector first. It does not resize the container; it returns an iterator to the new logical end.
*   **Time Complexity:** $$O(N)$$

    ```
    vector<int> v = {1, 2, 2, 3, 3, 3, 4, 1};
    sort(v.begin(), v.end()); // {1, 1, 2, 2, 3, 3, 3, 4}

    // The "Erase-Unique" Idiom
    v.erase(unique(v.begin(), v.end()), v.end()); 
    // v is now {1, 2, 3, 4}
    ```

#### `std::next_permutation`

Rearranges elements into the next lexicographically greater permutation. Returns `false` if no such permutation exists (i.e., array is sorted in strictly descending order).

* **Time Complexity:** $$O(N)$$ per call.
*   **Usage:** To generate _all_ permutations, sort the array ascending first.

    ```
    vector<int> v = {1, 2, 3};
    do {
        // Process {1,2,3}, {1,3,2}, {2,1,3}, etc.
    } while (next_permutation(v.begin(), v.end()));
    ```

#### `std::rotate`

Performs a left rotation on a range of elements.

*   **Time Complexity:** $$O(N)$$

    ```
    vector<int> v = {1, 2, 3, 4, 5};
    // Rotate left by 2 positions
    rotate(v.begin(), v.begin() + 2, v.end()); 
    // Result: {3, 4, 5, 1, 2}
    ```

### 5. Non-Modifying Operations (Counting & Finding)

#### `std::count` & `std::count_if`

Returns the number of elements matching a value or a condition.

*   **Time Complexity:** $$O(N)$$

    ```
    vector<int> v = {1, 2, 2, 3};
    int twos = count(v.begin(), v.end(), 2); // 2

    // Count even numbers using count_if and lambda
    int evens = count_if(v.begin(), v.end(), [](int x){ return x % 2 == 0; }); 
    ```

#### `std::find` & `std::find_if`

Finds the first element matching a value or condition. Returns `v.end()` if not found.

*   **Time Complexity:** $$O(N)$$

    ```
    auto it = find(v.begin(), v.end(), 3);
    if (it != v.end()) cout << "Found at index " << distance(v.begin(), it);
    ```

### 6. Honorable Mention (Actually in `<numeric>`)

While not strictly in `<algorithm>`, this function is almost always used alongside them in CP.

#### `std::accumulate`

Calculates the sum of a range. **(Requires `#include <numeric>`)**

* **Time Complexity:** $$O(N)$$
*   **Crucial Note:** Pay attention to the type of the third argument (the initial sum). If you pass `0`, the sum is calculated as a 32-bit `int`, which can cause integer overflow. Pass `0LL` if you expect a 64-bit `long long` result.

    ```
    #include <numeric>
    vector<int> v = {1000000000, 1000000000, 1000000000};

    // WRONG (will overflow)
    long long bad_sum = accumulate(v.begin(), v.end(), 0); 

    // CORRECT
    long long good_sum = accumulate(v.begin(), v.end(), 0LL); 
    ```

## C++ STL Searching: Numbers, Strings, and Containers

In C++, the way you search depends heavily on the data structure you are using. Using the wrong search function can turn an $$O(\log N)$$ operation into an $$O(N)$$ one, which will result in Time Limit Exceeded (TLE) in coding competitions.

### 1. Searching in Vectors/Arrays (Numbers, Objects)

#### A. Unsorted Data: Linear Search $$O(N)$$

If your array is not sorted, you must check every element. Use `<algorithm>` functions.

*   **`std::find`**: Finds the exact value.

    ```
    vector<int> nums = {4, 2, 7, 1, 9};
    auto it = find(nums.begin(), nums.end(), 7);

    if (it != nums.end()) {
        cout << "Found 7 at index: " << distance(nums.begin(), it) << "\n";
    } else {
        cout << "Not found\n";
    }
    ```
*   **`std::find_if`**: Finds the first element that matches a specific condition (using a lambda).

    ```
    // Find the first even number
    auto it = find_if(nums.begin(), nums.end(), [](int x) { return x % 2 == 0; });
    ```

#### B. Sorted Data: Binary Search $$O(\log N)$$

If your array is sorted, **always** use binary search.

*   **`std::binary_search`**: Returns `true` or `false`.

    ```
    vector<int> sorted_nums = {1, 2, 4, 7, 9};
    bool exists = binary_search(sorted_nums.begin(), sorted_nums.end(), 4); // true
    ```
*   **`std::lower_bound`** & **`std::upper_bound`**: (Crucial for Medium/Hard problems).

    * `lower_bound`: First element $$\ge$$ target.
    * `upper_bound`: First element $$>$$ target.

    ```
    vector<int> v = {10, 20, 20, 20, 30};
    auto lb = lower_bound(v.begin(), v.end(), 20); // Iterator to first 20
    auto ub = upper_bound(v.begin(), v.end(), 20); // Iterator to 30

    // Trick: Count occurrences in a sorted array in O(log N)
    int count = distance(lb, ub); // 3 occurrences of '20'
    ```

### 2. Searching in Strings (`std::string`)

For strings, do **not** use `std::find` from `<algorithm>` unless you are looking for a single character. For substrings, `std::string` has highly optimized built-in member functions.

#### The Golden Rule: `std::string::npos`

When a string search fails, it returns `std::string::npos` (which represents the maximum possible value for `size_t`), NOT an iterator.

*   **`s.find()`**: Search for a substring or character from left to right.

    ```
    string text = "hello algorithm world";

    // 1. Searching for a substring
    size_t pos = text.find("algo");
    if (pos != string::npos) {
        cout << "Found 'algo' at index: " << pos << "\n";
    }

    // 2. Searching starting from a specific index
    size_t next_pos = text.find("o", 5); // Find first 'o' starting at index 5
    ```
*   **`s.rfind()`**: Reverse find. Searches from right to left (finds the _last_ occurrence).

    ```
    string file = "document.txt.bak";
    size_t dot_pos = file.rfind('.'); // Finds the dot before 'bak'
    ```
*   **`s.find_first_of()`** & **`s.find_first_not_of()`**: Character set searching.

    ```
    string s = "price: $$50";
    // Find the first character that is a digit
    size_t digit_pos = s.find_first_of("0123456789"); 

    // Find the first character that is NOT a space or punctuation
    size_t word_pos = s.find_first_not_of(" :$$"); 
    ```

### 3. Searching in Sets and Maps

Sets and Maps are implemented as Binary Search Trees (Red-Black Trees). Unordered Sets/Maps are Hash Tables. **Never use `std::find` from `<algorithm>` on these!** It will force an $$O(N)$$ linear search. Always use their member functions.

#### A. `std::set` / `std::map` (Ordered) - $$O(\log N)$$

*   **`.find()`**: Returns an iterator to the element, or `.end()` if missing.

    ```
    set<int> s = {10, 20, 30};
    if (s.find(20) != s.end()) {
        cout << "20 exists in the set\n";
    }
    ```
*   **`.count()`**: Returns `1` if it exists, `0` if it doesn't. (Cleaner syntax than `.find()` for simple existence checks).

    ```
    map<string, int> freq;
    freq["apple"] = 5;
    if (freq.count("apple")) {
        cout << "Apple is in the map\n";
    }
    ```
*   **`.lower_bound()` / `.upper_bound()`**: Works exactly like the `<algorithm>` versions, but runs in $$O(\log N)$$ using the tree structure.

    ```
    set<int> s = {10, 20, 30, 40};
    auto it = s.lower_bound(25); // Iterator pointing to 30
    ```

#### B. `std::unordered_set` / `std::unordered_map` (Hash Tables) - $$O(1)$$ Average

The syntax is exactly the same as ordered sets/maps (`.find()` and `.count()`), but the underlying search is an $$O(1)$$ hash lookup instead of an $$O(\log N)$$ tree traversal. Note that unordered containers do **not** have `.lower_bound()` or `.upper_bound()`.

### Summary Cheat Sheet

|                       |                            |                            |                              |                     |
| --------------------- | -------------------------- | -------------------------- | ---------------------------- | ------------------- |
| **Data Structure**    | **To check existence**     | **To find exact position** | **To find closest (≥ or >)** | **Time Complexity** |
| **Unsorted Vector**   | `std::find`                | `std::find`                | _Not possible_               | $$O(N)$$            |
| **Sorted Vector**     | `std::binary_search`       | `std::lower_bound`         | `std::lower_bound` / `upper` | $$O(\log N)$$       |
| **String**            | `s.find() != string::npos` | `s.find()`                 | _Not applicable_             | $$O(N \times M)$$   |
| **Set/Map**           | `s.count()`                | `s.find()`                 | `s.lower_bound()`            | $$O(\log N)$$       |
| **Unordered Set/Map** | `s.count()`                | `s.find()`                 | _Not applicable_             | $$O(1)$$ (Avg)      |
