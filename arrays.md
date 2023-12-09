# Arrays

---

## Approach - 2 pointers

### 1. Remove Duplicates from Sorted Array

[https://leetcode.com/problems/remove-duplicates-from-sorted-array/
](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

```cpp
int removeDuplicates(vector<int>& nums) {
    int i = 0, j = 1, size = nums.size(), k = 1;
    while(j < size){
        if(nums[i] != nums[j]){
            nums[i+1] = nums[j];
            k++;
            i++;
        }
        j++;
    }
    return k;
}
```

### 2. Move Zeroes

[https://leetcode.com/problems/move-zeroes](https://leetcode.com/problems/move-zeroes)

```cpp
void moveZeroes(vector<int>& nums) {
    int i = 0, j = 0, size = nums.size();
    while(j < size){
        while(nums[j] == 0 && j < size - 1){
            j++;
        }
        auto const temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
        i++;
        j++;
    }
    return;
}
```

### 3. Merge two sorted arrays

[https://www.codingninjas.com/studio/problems/sorted-array_6613259](https://www.codingninjas.com/studio/problems/sorted-array_6613259)

```cpp
vector < int > sortedArray(vector < int > a, vector < int > b) {
int i = 0, j = 0, sizeA = a.size(), sizeB = b.size();
vector<int> ans;
while(i < sizeA && j < sizeB){
    auto begin = ans.begin();
    auto end = ans.end();
    if(a[i] < b[j]){
        if(begin == end || *(end - 1) != a[i])
            ans.push_back(a[i]);
        i++;
    }
    else{
        if(begin == end || *(end - 1) != b[j])
            ans.push_back(b[j]);
        j++;
    }
}
while(i < sizeA){
    auto begin = ans.begin();
    auto end = ans.end();
    if(begin == end || *(end - 1) != a[i])
        ans.push_back(a[i]);
    i++;
}
while(j < sizeB){
    auto begin = ans.begin();
    auto end = ans.end();
    if(begin == end || *(end - 1) != b[j])
        ans.push_back(b[j]);
    j++;
}
return ans;
}
```

---

## Approach - Rotating Arrays by K places

### 1. Rotate Arrays

[https://leetcode.com/problems/rotate-array/](https://leetcode.com/problems/rotate-array/)

```cpp
void rotate_substring(vector<int>& nums, int si, int ei){
    while(si < ei){
        auto const temp = nums[si];
        nums[si] = nums[ei];
        nums[ei] = temp;
        si++;
        ei--;
    }
    return;
}

void rotate(vector<int>& nums, int k) {
    int size = nums.size();
    k = k % size;
    rotate_substring(nums, 0, size - k - 1);
    rotate_substring(nums, size - k, size - 1);
    rotate_substring(nums, 0, size - 1);
    return;
}
```

---

## Approach - XorSum

### 1. Finding missing number

[https://leetcode.com/problems/missing-number/](https://leetcode.com/problems/missing-number/)

```cpp
int missingNumber(vector<int>& nums) {
    int xorSum = 0, xorN = 0;
    for(int i = 0; i < nums.size(); i++){
        xorSum ^= nums[i];
        xorN ^= i;
    }
    xorN ^= nums.size();
    return xorSum ^ xorN;
}
```

### 2. Single Number

[https://leetcode.com/problems/single-number](https://leetcode.com/problems/single-number)

```cpp
int singleNumber(vector<int>& nums) {
    int xorSum = 0;
    for(int i = 0; i < nums.size(); i++){
        xorSum ^= nums[i];
    }
    return xorSum;
}
```

---

## Approach - Kadane-esque (Resetting tempAns on some condition)

### 1. Maximum Consecutive Ones

[https://leetcode.com/problems/max-consecutive-ones/](https://leetcode.com/problems/max-consecutive-ones/)

```cpp
int findMaxConsecutiveOnes(vector<int>& nums) {
    int maxCount = 0, currentMax = 0;
    for(int i = 0; i < nums.size(); i++){
        if(nums[i] == 1){
            currentMax++;
            maxCount = max(maxCount, currentMax);
        }
        else currentMax = 0;
    }
    return maxCount;
}
```

### 2. Majority Element ( n / 2 )

[https://leetcode.com/problems/majority-element](https://leetcode.com/problems/majority-element)

```cpp
int majorityElement(vector<int>& nums) {
    int size = nums.size(), i = 0, majEl = nums[0], majCount = 0;
    while(i < size){
        if(majCount == 0) majEl = nums[i];
        if(nums[i] == majEl) majCount++;
        else majCount--;
        i++;
    }
    return majEl;
}
```

### 3. Max Subarray sum | Kadane's algorithm

[https://leetcode.com/problems/maximum-subarray/](https://leetcode.com/problems/maximum-subarray/)

```cpp
int maxSubArray(vector<int>& nums) {
    int size = nums.size(), maxSum = INT_MIN, currMax = INT_MIN, i = 0;
    while(i < size){
        if(currMax < 0) currMax = 0;
        currMax += nums[i];
        maxSum = max(maxSum, currMax);
        i++;
    }
    return maxSum;
}
```

### 4. Longest Successive Elements

[https://www.codingninjas.com/studio/problems/longest-successive-elements_6811740](https://www.codingninjas.com/studio/problems/longest-successive-elements_6811740)

```cpp
int longestSuccessiveElements(vector<int>&a) {
    int size = a.size(), count = 1, tempCount = 1;
    sort(a.begin(), a.end());

    for(int i = 1; i < size; i++){
        if(a[i - 1] == a[i]) continue;
        if(a[i - 1] + 1 != a[i]) tempCount = 1;
        else tempCount++;
        count = max(tempCount, count);
    }
    return count;
}
```

### 5. Majority Element ( n / 3 )

[https://leetcode.com/problems/majority-element-ii/](https://leetcode.com/problems/majority-element-ii/)

```cpp
vector<int> majorityElement(vector<int>& nums) {
    int majOne = nums[0], majTwo = nums[0], countOne = 0, countTwo = 0;
    vector<int> ans;
    for(int i = 0; i < nums.size(); i++){
        if(majOne == nums[i]) countOne++;
        else if(majTwo == nums[i]) countTwo++;
        else if(countOne == 0) majOne = nums[i], countOne++;
        else if(countTwo == 0) majTwo = nums[i], countTwo++;
        else countOne--, countTwo--;
    }
    countOne = countTwo = 0;
    for(int i = 0; i < nums.size(); i++){
        if(nums[i] == majOne) countOne++;
        else if(nums[i] == majTwo) countTwo++;
    }
    cout << majOne << ' ' << majTwo << endl;
    if(countOne > nums.size() / 3) ans.push_back(majOne);
    if(countTwo > nums.size() / 3) ans.push_back(majTwo);
    return ans;
}
```

---

## Approach - Hashing

### 1. Two Sum

[https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    int size = nums.size(), i = 0;
    unordered_map<int, int> freqMap;
    while(i < size){
        int curr = nums[i];
        if(freqMap.find(target - curr) != freqMap.end())
            return {freqMap[target - curr], i};
        freqMap[curr] = i;
        i++;
    }
    return {-1, -1};
}
```

---

## Approach - Dutch National Flag || N pointers

### 1. Sort colors

[https://leetcode.com/problems/sort-colors/](https://leetcode.com/problems/sort-colors/)

```cpp
void sortColors(vector<int>& nums) {
    int size = nums.size(), curr = 0, zero = 0, two = size - 1;
    while(curr <= two){
        const int currEl = nums[curr];
        if(currEl == 2) swap(nums[curr], nums[two]), two--;
        else if(currEl == 0) swap(nums[curr], nums[zero]), zero++, curr++;
        else curr++;
    }
    return;
}
```

---

## Approach - Order based max/min

### 1. Stock buy and sell

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

```cpp
int maxProfit(vector<int>& prices) {
    int minPrice = INT_MAX, ans = 0;
    for(int i = 0; i < prices.size(); i++){
        minPrice = min(prices[i], minPrice);
        ans = max(prices[i] - minPrice, ans);
    }
    return ans;
}
```

---

## Approach - Rearrange elements

### 1. Rearrange by sign

[https://leetcode.com/problems/rearrange-array-elements-by-sign/](https://leetcode.com/problems/rearrange-array-elements-by-sign/)

```cpp
vector<int> rearrangeArray(vector<int>& nums) {
    // Space complexity is a must :sadge
    vector<int> ans(nums.size());
    int size = nums.size(), positive = 0, negative = 1, current = 0;
    while(current < size){
        if(nums[current] < 0){
            ans[negative] = nums[current];
            negative += 2;
        }
        else{
            ans[positive] = nums[current];
            positive += 2;
        }
        current++;
    }
    return ans;
}
```

---

## Approach - Pattern recognition

### 1. Next permutation

[https://leetcode.com/problems/next-permutation](https://leetcode.com/problems/next-permutation)

```cpp
void nextPermutation(vector<int>& nums) {
    // Pattern resolution question :cold
    int size = nums.size(), breakpoint = -1;
    int i = size - 2;

    while(i >= 0){
        if(nums[i] >= nums[i+1]){
            i--;
        }
        else{
            breakpoint = i;
            break;
        }
    }

    if(breakpoint == -1){
        reverse(nums.begin(),nums.end());
        return;
    }

    i = size - 1;
    while(i >= 0){
        if(nums[i] > nums[breakpoint]){
            swap(nums[i],nums[breakpoint]);
            break;
        }
        i--;
    }

    reverse(nums.begin() + breakpoint + 1, nums.end());
    return;
}
```

---

## Approach - Prefix Sum

### 1. Subarray sum equal to k

[https://leetcode.com/problems/subarray-sum-equals-k](https://leetcode.com/problems/subarray-sum-equals-k)

```cpp
int subarraySum(vector<int>& nums, int k) {
    // Calculate prefix sum till each index and find out
    // whether the difference between currentPrefixSum and k
    // exists in the map
    int size = nums.size(), count = 0, prefixSum = 0;
    map<int,int> prefixMap;
    prefixMap[0] = 1;
    for(int i = 0; i < size; i++){
        prefixSum += nums[i];
        int remainder = prefixSum - k;
        count += prefixMap[remainder];
        prefixMap[prefixSum]++;
    }
    return count;
}
```

### 2. Longest subarray with sum k

[https://www.codingninjas.com/studio/problems/longest-subarray-with-sum-k_6682399](https://www.codingninjas.com/studio/problems/longest-subarray-with-sum-k_6682399)

```cpp
int longestSubarrayWithSumK(vector<int> arr, long long k){
    int size = arr.size(), i = 0, maxLength = INT_MIN, prefixSum = 0;
    unordered_map<int, int> prefixSumMp;
    while(i < size){
        prefixSum += arr[i];
        if(prefixSumMp.find(prefixSum - k) != prefixSumMp.end()){
            int length = i - prefixSumMp[prefixSum - k];
            maxLength = max(maxLength, length);
        }
        if(prefixSumMp.find(prefixSum) == prefixSumMp.end()){
            prefixSumMp[prefixSum] = i;
        }
        i++;
    }
    return maxLength;
}
```

### 3. Longest subarray with zero sum

[https://www.codingninjas.com/studio/problems/longest-subarray-with-zero-sum_6783450](https://www.codingninjas.com/studio/problems/longest-subarray-with-zero-sum_6783450)

```cpp
int getLongestZeroSumSubarrayLength(vector<int> &arr){
	int size = arr.size(), i = 0, maxLen = 0, prefixSum = 0;
	map<int, int> prefixMap;
	prefixMap[0] = 0;
	while(i < size){
		prefixSum += arr[i];
		if(prefixMap.find(prefixSum) != prefixMap.end()){
			maxLen = max(maxLen, i - prefixMap[prefixSum] + 1);
		}
		else prefixMap[prefixSum] = i + 1;
		i++;
	}
	return maxLen;
}
```

### 4. Maximum Product Subarray

[https://leetcode.com/problems/maximum-product-subarray/](https://leetcode.com/problems/maximum-product-subarray/)

```cpp
int maxProduct(vector<int>& nums) {
    int size = nums.size(), i = 0, maxP = INT_MIN, prefixProduct = 1, suffixProduct = 1;
    while(i < size){
        if(prefixProduct == 0) prefixProduct = 1;
        if(suffixProduct == 0) suffixProduct = 1;
        prefixProduct *= nums[i];
        suffixProduct *= nums[size - i - 1];
        maxP = max({maxP, prefixProduct, suffixProduct});
        i++;
    }
    return maxP;
}
```

---

## Approach - Bit manipulation

### 1. Missing and repeating numbers

[https://www.codingninjas.com/studio/problems/missing-and-repeating-numbers_6828164](https://www.codingninjas.com/studio/problems/missing-and-repeating-numbers_6828164)

```cpp
vector<int> findMissingRepeatingNumbers(vector < int > a) {
    int size = a.size(), i = 0, xorSum = 0;
    // Xor of all elements as well as first n natural numbers
    while(i < size){
        xorSum ^= a[i];
        xorSum ^= (i + 1);
        i++;
    }
    // Currently xorsum = missing ^ repeating
    int setBit = 1;
    while(true){
        if(setBit & xorSum){
            break;
        }
        setBit = setBit << 1;
    }
    // setBit now contains the last setBit of the xorSum
    // meaning either of the two has a setBit at setBit place
    int missing = 0, repeated = 0;

    i = 0;
    while(i < size){
        if(a[i] & setBit){
            missing ^= a[i];
        }
        else{
            repeated ^= a[i];
        }
        if(i + 1 & setBit){
            missing ^= i + 1;
        }
        else{
            repeated ^= i + 1;
        }
        i++;
    }
    // find out which one is missing and vice-versa
    i = 0;
    while(i < size){
        if(a[i] == missing){
            swap(missing, repeated);
            break;
        }
        i++;
    }
    return {repeated, missing};
}
```

---

## Approach - Merge Sort

### 1. Count number of inversions

[https://www.codingninjas.com/studio/problems/number-of-inversions_6840276](https://www.codingninjas.com/studio/problems/number-of-inversions_6840276)

```cpp
int helper(vector<int>& arr, int si, int ei){
    if(si >= ei){
        return 0;
    }

    int mi = si + (ei - si) / 2;
    int count = helper(arr, si, mi) + helper(arr, mi + 1, ei);
    vector<int> tempArr(ei - si + 1);

    int i = si, j = mi + 1, k = 0;
    while(i <= mi && j <= ei){
        if(arr[i] <= arr[j]){
            tempArr[k] = arr[i];
            i++;
        }
        else{
            count += (mi - i + 1);
            tempArr[k] = arr[j];
            j++;
        }
        k++;
    }
    while(i <= mi){
        tempArr[k] = arr[i];
        i++;
        k++;
    }
    while(j <= ei){
        tempArr[k] = arr[j];
        j++;
        k++;
    }
    i = si, k = 0;
    while(i <= ei){
        arr[i] = tempArr[k];
        i++;
        k++;
    }

    return count;
}

int numberOfInversions(vector<int>&a, int n) {
    return helper(a, 0, n - 1);
}
```

### 2. Count inversion ( a[i] > 2 \* a[j] )

[https://leetcode.com/problems/reverse-pairs](https://leetcode.com/problems/reverse-pairs)

```cpp
// Same as count inversion but with a slight change
static int helper(vector<int>& arr, int si, int ei){
    if(si >= ei) return 0;
    int mi = si + (ei - si) / 2;
    int count = helper(arr, si, mi) + helper(arr, mi + 1, ei);

    // Count inversions just add another nlogn to the complexity
    int i = si, j = mi + 1, k = 0;
    while(i <= mi){
        while(j <= ei && (long long)arr[i] > (long long)2 * arr[j]){
            j++;
        }
        count += (j - (mi + 1));
        i++;
    }
    i = si, j = mi + 1;
    vector<int> tempArr(ei - si + 1);
    while(i <= mi && j <= ei){
        if(arr[i] <= arr[j]){
            tempArr[k] = arr[i];
            i++;
        }
        else{
            tempArr[k] = arr[j];
            j++;
        }
        k++;
    }
    while(i <= mi){
        tempArr[k] = arr[i];
        k++;
        i++;
    }
    while(j <= ei){
        tempArr[k] = arr[j];
        k++;
        j++;
    }
    i = si, k = 0;
    while(i <= ei){
        arr[i] = tempArr[k];
        k++;
        i++;
    }
    return count;
}

int reversePairs(vector<int>& nums) {
    return helper(nums, 0, nums.size() - 1);
}
```
