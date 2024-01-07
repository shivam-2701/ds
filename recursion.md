# Recursion

---

## Approach - Backtracking

### 1. Binary strings with no consecutive 1s

[https://www.codingninjas.com/studio/problems/-binary-strings-with-no-consecutive-1s.\_893001](https://www.codingninjas.com/studio/problems/-binary-strings-with-no-consecutive-1s._893001)

```cpp
void helper(int n, string& tempAns, vector<string>& ans){
    if(n == 0){
        ans.push_back(tempAns);
        return;
    }
    if(tempAns.size() == 0 || tempAns.back() != '1'){
        tempAns.push_back('1');
        helper(n - 1, tempAns, ans);
        tempAns.pop_back();
    }
    tempAns.push_back('0');
    helper(n - 1, tempAns, ans);
    tempAns.pop_back();
}

vector<string> generateString(int n) {
    vector<string> ans;
    string tempAns;
    helper(n, tempAns, ans);
    sort(ans.begin(), ans.end());
    return ans;
}
```

## 2. Generate Parentheses

[https://leetcode.com/problems/generate-parentheses](https://leetcode.com/problems/generate-parentheses)

```cpp
void helper(int open, int close, string& tempAns, vector<string>& ans){
    // The intuition behind this solution, is that at no point should the close
    // parenthesis be greater than open parenthesis, and following that,
    // at any point we have 2 options whether to open a parenthesis or close one
    if(open == 0 && close == 0){
        ans.push_back(tempAns);
        return;
    }

    if(open > 0){
        tempAns.push_back('(');
        helper(open - 1, close, tempAns, ans);
        tempAns.pop_back();
    }
    if(close > open){
        tempAns.push_back(')');
        helper(open, close - 1, tempAns, ans);
        tempAns.pop_back();
    }
    return;
}

vector<string> generateParenthesis(int n) {
    vector<string> ans;
    string tempAns;
    helper(n, n, tempAns, ans);
    return ans;
}
```

### 3. Word Search

[https://leetcode.com/problems/word-search/](https://leetcode.com/problems/word-search/)

```cpp
bool helper(vector<vector<char>>& board, string word, int index, int row, int col){
    if(index == word.size()) return true;
    if(row < 0 || row >= board.size()) return false;
    if(col < 0 || col >= board[0].size()) return false;
    if(board[row][col] != word[index]) return false;

    const char tempCh = board[row][col];
    board[row][col] = '#';

    const bool tempUp = helper(board, word, index + 1, row - 1, col);
    const bool tempDown = helper(board, word, index + 1, row + 1, col);
    const bool tempLeft = helper(board, word, index + 1, row, col - 1);
    const bool tempRight = helper(board, word, index + 1, row, col + 1);

    board[row][col] = tempCh;

    return tempUp || tempDown || tempLeft || tempRight;
}

bool exist(vector<vector<char>>& board, string word) {
    int rows = board.size(), cols = board[0].size(), i = 0, j = 0;
    while(i < rows){
        j = 0;
        while(j < cols){
            if(board[i][j] == word[0]){
                const bool tempAns = helper(board, word, 0, i, j);
                if(tempAns) return true;
            }
            j++;
        }
        i++;
    }
    return false;
}
```

### 4. N Queens

[https://leetcode.com/problems/n-queens](https://leetcode.com/problems/n-queens)

```cpp
bool isPossible(vector<string>& board, int row, int col){
    // Upper left diagonal
    int i = row - 1, j = col - 1;
    while(i >= 0 && j >= 0){
        if(board[i][j] != '.') return false;
        i--, j--;
    }
    // Upper right diagonal
    i = row - 1, j = col + 1;
    while(i >= 0 && j < board[0].size()){
        if(board[i][j] != '.') return false;
        i--, j++;
    }
    // Vertical
    i = 0;
    while(i < row){
        if(board[i][col] != '.') return false;
        i++;
    }
    return true;
}

void helper(int n, vector<vector<string>>& ans, vector<string>& board, int row){
    if(row == n){
        ans.push_back(board);
        return;
    }

    int i = 0;
    while(i < n){
        const bool possible = isPossible(board, row, i);
        if(possible){
            board[row][i] = 'Q';
            helper(n, ans, board, row + 1);
            board[row][i] = '.';
        }
        i++;
    }
    return;
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> ans;
    vector<string> board(n, string(n, '.'));
    helper(n, ans, board, 0);
    return ans;
}
```

### 5. Rat in a maze

[https://www.codingninjas.com/studio/problems/rat-in-a-maze-\_8842357](https://www.codingninjas.com/studio/problems/rat-in-a-maze-_8842357)

```cpp
void helper(vector<vector<int>>& maze, vector<string>& ans, string& tempAns, int row, int col){
    if(row == maze.size() - 1 && col == maze[0].size() - 1){
        ans.push_back(tempAns);
        return;
    }
    if(row == maze.size() || col == maze[0].size()) return;
    // Down
    if(row < maze.size() - 1 && maze[row + 1][col] == 1){
        maze[row][col] = -1, tempAns.push_back('D');
        helper(maze, ans, tempAns, row + 1, col);
        maze[row][col] = 1, tempAns.pop_back();
    }

    // Left
    if(col > 0 && maze[row][col - 1] == 1){
        maze[row][col] = -1, tempAns.push_back('L');
        helper(maze, ans, tempAns, row, col - 1);
        maze[row][col] = 1, tempAns.pop_back();
    }

    // Right
    if(col < maze[0].size() - 1 && maze[row][col + 1] == 1){
        maze[row][col] = -1, tempAns.push_back('R');
        helper(maze, ans, tempAns, row, col + 1);
        maze[row][col] = 1, tempAns.pop_back();
    }

    // Up
    if(row > 0 && maze[row - 1][col] == 1){
        maze[row][col] = -1, tempAns.push_back('U');
        helper(maze, ans, tempAns, row - 1, col);
        maze[row][col] = 1, tempAns.pop_back();
    }
    return;
}

vector<string> ratMaze(vector<vector<int>> &mat) {
    vector<string> ans;
    string tempAns;
    helper(mat, ans, tempAns, 0, 0);
    return ans;
}
```

---

## Approach - Generate all possible solutions

### 1. Power Set

[https://leetcode.com/problems/subsets](https://leetcode.com/problems/subsets)

```cpp
void recursive(vector<int>& nums, int index, vector<int>& tempAns, vector<vector<int>>& ans){
    if(index == nums.size()){
        ans.push_back(tempAns);
        return;
    }
    tempAns.push_back(nums[index]);
    recursive(nums, index + 1, tempAns, ans);
    tempAns.pop_back();
    recursive(nums, index + 1, tempAns, ans);
    return;
}

vector<vector<int>> iterative(vector<int>& nums){
    int size = nums.size(), totalSubsets = pow(2, size), i = 0;
    vector<vector<int>> ans;

    while(i < totalSubsets){
        vector<int> tempAns;
        for(int j = 0; j < size; j++){
            if(i & (1 << j)){
                tempAns.push_back(nums[j]);
            }
        }
        ans.push_back(tempAns);
        i++;
    }
    return ans;
}

vector<vector<int>> subsets(vector<int>& nums) {
    auto ans = iterative(nums);
    return ans;
}
```

---

## Approach - Generate all unique combinations

### 1. Combination Sum II

[https://leetcode.com/problems/combination-sum-ii/](https://leetcode.com/problems/combination-sum-ii/)

```cpp
void helper(vector<int>& candidates, int target, vector<vector<int>>& ans, vector<int>& tempAns, int index){
    if(target == 0) ans.push_back(tempAns);
    if(index == candidates.size()){
        return;
    }

    int size = candidates.size(), i = index;
    while(i < size){
        const int temp = candidates[i];
        if (temp > target) break;
        tempAns.push_back(temp);
        helper(candidates, target - temp, ans, tempAns, i + 1);
        tempAns.pop_back();
        while(i < size && candidates[i] == temp) i++;
    }
    return;
}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());
    vector<vector<int>> ans;
    vector<int> tempAns;
    helper(candidates, target, ans, tempAns, 0);
    return ans;
}
```

### 2. Palindrome Partition

[https://leetcode.com/problems/palindrome-partitioning](https://leetcode.com/problems/palindrome-partitioning)

```cpp
bool isPalindrome(string& s, int si, int ei){
    while(si < ei){
        if(s[si] != s[ei]) return false;
        si++, ei--;
    }
    return true;
}

void helper(string& s, vector<vector<string>>& ans, vector<string>& tempAns, int index){
    if(index == s.size()){
        ans.push_back(tempAns);
        return;
    }
    int size = s.size();
    for(int i = index; i < size; i++){
        const bool isPossible = isPalindrome(s, index, i);
        if(isPossible){
            tempAns.push_back(s.substr(index, i - index + 1));
            helper(s, ans, tempAns, i + 1);
            tempAns.pop_back();
        }
    }
    return;
}

vector<vector<string>> partition(string s) {
    vector<vector<string>> ans;
    vector<string> tempAns;
    helper(s, ans, tempAns, 0);
    return ans;
}
```

### 3. Word break

[https://leetcode.com/problems/word-break/](https://leetcode.com/problems/word-break/)

```cpp
bool helper(string s, vector<string>& wordDict, int si, map<int, bool>& dp){
    if(si == s.size()) return true;
    if(dp.find(si) != dp.end()) return dp[si];

    int size = wordDict.size();
    for(int i = 0; i < wordDict.size(); i++){
        int k = si;
        for(int j = 0; j < wordDict[i].size(); j++){
            if(wordDict[i][j] != s[k]) break;
            k++;
        }
        if(k - si == wordDict[i].size()){
            const bool tempAns = helper(s, wordDict, k, dp);
            dp[si] = tempAns;
            if(tempAns) return tempAns;
        }
    }
    dp[si] = false;
    return false;
}

bool wordBreak(string s, vector<string>& wordDict) {
    map<int, bool> dp;
    return helper(s, wordDict, 0, dp);
}
```

### 4. Sudoku solver

[https://leetcode.com/problems/sudoku-solver](https://leetcode.com/problems/sudoku-solver)

```cpp
bool check(vector<vector<char>>& board, int row, int col, char target){
    // Check in row and column
    for(int i = 0; i < 9; i++){
        if(board[row][i] == target) return false;
        if(board[i][col] == target) return false;
    }
    // Check in sub-board
    int subBoardRow = row / 3, subBoardCol = col / 3;
    for(int i = subBoardRow * 3; i < (subBoardRow + 1) * 3; i++){
        for(int j = subBoardCol * 3; j < (subBoardCol + 1) * 3; j++){
            if(board[i][j] == target) return false;
        }
    }
    return true;
}

bool helper(vector<vector<char>>& board){
    int rows = board.size(), cols = board[0].size();
    for(int i = 0; i < rows; i++){
        for(int j = 0; j < cols; j++){
            if(board[i][j] != '.') continue;
            for(int k = 1; k < 10; k++){
                char target = to_string(k)[0];
                const bool isPossible = check(board, i, j, target);
                if(isPossible){
                    board[i][j] = target;
                    const bool tempAns = helper(board);
                    if(tempAns) return true;
                    board[i][j] = '.';
                }
            }
            return false;
        }
    }
    return true;
}

void solveSudoku(vector<vector<char>>& board) {
    helper(board);
    return;
}
```

---

## Approach - Count all possible solutions

### 1. More Subsequence

[https://www.codingninjas.com/studio/problems/more-subsequence_8842355](https://www.codingninjas.com/studio/problems/more-subsequence_8842355)

```cpp
int countSub(string a){
    /*
    * For each character the number of subsequences increase by a factor of 2,
    * but for every repeated character, the number of subsequences need to be decreased
    * because of the repeation, which turns out to be half of total subsequeces
    * after the addiion of its previous occurence, which we store in a map for
    * easy access
    */
    unordered_map<char, int> mp;
    int size = a.size(), count = 1, i = 0;
    while(i < size){
        const char ch = a[i];
        if(!mp[ch]){
            mp[ch] = count;
            count = count * 2;
        }
        else{
            const int temp = mp[ch];
            mp[ch] = count;
            count = count * 2, count = count - temp;
        }
        i++;
    }
    return count;
}

string moreSubsequence(int n, int m, string a, string b)
{
    const int countA = countSub(a);
    const int countB = countSub(b);

    return countB > countA ? b : a;
}
```
