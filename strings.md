# Strings

---

## Approach - Read string word by word

### 1. Reverse words

[https://leetcode.com/problems/reverse-words-in-a-string](https://leetcode.com/problems/reverse-words-in-a-string)

```cpp
string optimal(string s){
    istringstream iss(s);
    string word;
    s = "";
    while (iss >> word) {
        if (s != "") {
            s = " " + s;
        }
        s = word + s;
    }
    return s;
}

string reverseWords(string s) {
    return optimal(s);
}
```

---

## Approach - Search in rotated strings

### 1. Rotate String

[https://leetcode.com/problems/rotate-string/](https://leetcode.com/problems/rotate-string/)

```cpp
bool rotateString(string s, string goal) {
    if(s.size() != goal.size()) return false;

    string combined = s + s;
    return combined.find(goal) != string::npos;
}
```

---

## Approach - Frequency array

### 1. Valid Anagram

[https://leetcode.com/problems/valid-anagram/](https://leetcode.com/problems/valid-anagram/)

```cpp
bool isAnagram(string s, string t) {
    int sSize = s.size(), tSize = t.size(), i = 0;
    if(sSize != tSize) return false;

    int freqArr[26] = {0};
    while(i < sSize) freqArr[s[i] - 'a']++, i++;
    i = 0;
    while(i < tSize) freqArr[t[i] - 'a']--, i++;
    i = 0;
    while(i < 26){
        if(freqArr[i] != 0) return false;
        i++;
    }
    return true;
}
```

### 2. Sort character by frequency

[https://leetcode.com/problems/sort-characters-by-frequency/](https://leetcode.com/problems/sort-characters-by-frequency/)

```cpp
string optimal(string s){
        // Optimal solution is an application of bucket sort
        // with O(n) space and O(n) time complexity, Bucket Sort is viable if the
        // values to be sorted are in a range, here each character can have a freq
        // anywhere between 1 and size, so we create a list to keep a track of that
        // and since there can be mulitple elements with the same frequency,
        // we use a list of list, vector<vector>
        int size = s.size();
        unordered_map<char,int> freqMap;
        for(int i = 0; i < size; i++){
            freqMap[s[i]]++;
        }

        // Now instead of pushing it in a priority queue making it O(nlogn)
        // we store it in a freq list of elements
        vector<vector<char>> freqList(size + 1);
        for(auto el: freqMap){
            int freq = el.second;
            freqList[freq].push_back(el.first);
        }
        string ans;
        for(int i = size; i > 0; i--){
            auto elList = freqList[i];
            for(auto el: elList){
                ans.append(i, el);
            }
        }
        return ans;
    }

    string frequencySort(string s) {
        return optimal(s);
    }
```

---

## Approach - Check by growing radially outwards

### 1. Longest palindrome substring

[https://leetcode.com/problems/longest-palindromic-substring/](https://leetcode.com/problems/longest-palindromic-substring/)

```cpp
pair<int,int> checkPalindrome(string s, int mi){
    int si = mi - 1, ei = mi + 1;
    pair<int, int> ans;
    while(si >= 0 && ei < s.size()){
        if(s[si] != s[ei]) {
            ans = {si + 1 , ei - 1};
            break;
        }
        si--, ei++;
    }
    if(si == -1) ans = {0, ei - 1};
    else ans = {si + 1, ei - 1};
    return {ans.first / 2, (ans.second - 1) / 2};
}

string better(string s){
    // O(n^2) time complexity, the creation of odd string removes even palindrome
    string oddString, ans;
    for(int i = 0; i < s.size(); i++){
        oddString += '#';
        oddString += s[i];
    }
    oddString += '#';
    for(int i = 0; i < oddString.size(); i++){
        auto tempAns = checkPalindrome(oddString, i);
        int size = tempAns.second - tempAns.first + 1;
        if(size > ans.size()){
            ans = s.substr(tempAns.first, size);
        }
    }
    return ans;
}

string longestPalindrome(string s) {
    // Best solution is O(n), manachers algorithm
    return better(s);
}
```
