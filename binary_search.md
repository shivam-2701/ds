# Binary Search

---

## Implementation of lower and upper bounds

```cpp
int upperBound(vector<int> arr, int n, int x) {
	int size = arr.size(), si = 0, ei = size - 1;

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		if(arr[mi] > x) ei = mi - 1;
		else si = mi + 1;
	}
	return si;
}

int lowerBound(vector<int> arr, int n, int x) {
	int size = arr.size(), si = 0, ei = size - 1;

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		if(arr[mi] < x) si = mi + 1;
		else ei = mi - 1;
	}
	return ei;
}
```

---

## Approach - Lower and Upper Bounds

### 1. Search insert position

[https://leetcode.com/problems/search-insert-position/](https://leetcode.com/problems/search-insert-position/)

```cpp
int searchInsert(vector<int>& nums, int target) {
    int size = nums.size();
    vector<int>::iterator lowerBound = lower_bound(nums.begin(), nums.end(), target);
    return (lowerBound - nums.begin());
}
```

### 2. Number of occurences

[https://www.codingninjas.com/studio/problems/occurrence-of-x-in-a-sorted-array_630456](https://www.codingninjas.com/studio/problems/occurrence-of-x-in-a-sorted-array_630456)

```cpp
int numberOfOccurences(vector<int>& arr, int n, int x) {
	auto lowerBound = lower_bound(arr.begin(), arr.end(), x);
	auto upperBound = upper_bound(arr.begin(), arr.end(), x);
	return upperBound - lowerBound;
}
```

---

## Approach - Pseudo sorted arrays | One of two halves is sorted

### 1. Search in rotated sorted array

[https://leetcode.com/problems/search-in-rotated-sorted-array/](https://leetcode.com/problems/search-in-rotated-sorted-array/)

```cpp
int helper(vector<int>& nums, int si, int ei, int target){
	if(si > ei) return -1;
	int mi = si + (ei - si) / 2;

	if(nums[mi] == target) return mi;
	if(nums[si] <= nums[mi]){
		if(nums[si] <= target && nums[mi] > target) ei = mi - 1;
		else si = mi + 1;
	}
	else{
		if(nums[mi] < target && nums[ei] >= target) si = mi + 1;
		else ei = mi - 1;
	}
	return helper(nums, si, ei, target);
}
int search(vector<int>& nums, int target) {
	return helper(nums, 0, nums.size() - 1, target);
}
```

### 2. Minimum in rotated sorted array

[https://leetcode.com/problems/find-minimum-in-rotated-sorted-array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array)

```cpp
int findMin(vector<int>& nums) {
	int size = nums.size(), i = 0, minEl = INT_MAX;
	int si = 0, ei = size - 1;
	while(si <= ei){
		int mi = si + (ei - si) / 2;
		// Handles the edge case where the array is rotated exactly at the middle
		minEl = min(minEl, nums[mi]);
		if(nums[mi] < nums[ei]) ei = mi - 1;
		else si = mi + 1;
	}
	return minEl;
}
```

---

## Approach - 1 of n halves is sorted ( sorta )

### 1. Find peak element

[https://leetcode.com/problems/find-peak-element/](https://leetcode.com/problems/find-peak-element/)

```cpp
int findPeakElement(vector<int>& nums) {
	int size = nums.size(), si = 1, ei = size - 2;
	// Edge cases
	if(size == 1) return 0;
	if(nums[0] > nums[1]) return 0;
	if(nums[size - 2] < nums[size - 1]) return size - 1;

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		if(nums[mi] > nums[mi - 1] && nums[mi] > nums[mi + 1]) return mi;
		if(nums[mi] > nums[mi - 1] && nums[mi] < nums[mi + 1]) si = mi + 1;
		else ei = mi - 1;
	}
	return -1;
}
```

---

## Approach - Index-based mapping

### 1. Single element in a sorted array

[https://leetcode.com/problems/single-element-in-a-sorted-array/](https://leetcode.com/problems/single-element-in-a-sorted-array/)

```cpp
int singleNonDuplicate(vector<int>& nums) {
	int size = nums.size();
	int si = 1, ei = size - 2;

	if(size == 1) return nums[0];
	if(nums[0] != nums[1]) return nums[0];
	if(nums[size - 1] != nums[size - 2]) return nums[size - 1];

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		if(mi == si && si == ei) return nums[mi];
		if(nums[mi] != nums[mi - 1] && nums[mi] != nums[mi + 1]) return nums[mi];
		if(mi % 2 == 0){
			if(nums[mi] == nums[mi + 1]) si = mi + 1;
			else ei = mi - 1;
		}
		else{
			if(nums[mi] == nums[mi + 1]) ei = mi - 1;
			else si = mi + 1;
		}
	}
	return -1;
}
```

### 2. Kth missing positive integer

[https://leetcode.com/problems/kth-missing-positive-number](https://leetcode.com/problems/kth-missing-positive-number)

```cpp
int findKthPositive(vector<int>& arr, int k) {
	int size = arr.size(), si = 0, ei = size - 1;
	while(si <= ei){
		int mi = si + (ei - si) / 2;
		int missing = arr[mi] - (mi + 1);
		if(missing >= k) ei = mi - 1;
		else si = mi + 1;
	}
	// missing@arr[ei] = arr[ei] - (ei + 1);
	// kthMissing      = arr[ei] + (k - missing@arr[ei]);
	// => kthMissing   = arr[ei] + (k - (arr[ei] - (ei + 1)));
	// =>              = arr[ei] + k - arr[ei] + ei + 1;
	// =>              = k + ei + 1
	// =>              = k + si
	return si + k;
}
```

---

## Approach - Answer within a range of answers

### 1. Koko eating bananas

[https://leetcode.com/problems/koko-eating-bananas/](https://leetcode.com/problems/koko-eating-bananas/)

```cpp
long long timeTaken(vector<int>& piles, int time){
	long long size = piles.size(), i = 0, ans = 0;
	while(i < size){
		ans += ceil((double)piles[i] / (double)time);
		i++;
	}
	return ans;
}

int minEatingSpeed(vector<int>& piles, int h) {
	int si = 1, ei = *max_element(piles.begin(), piles.end());

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		if(timeTaken(piles, mi) > h) si = mi + 1;
		else ei = mi - 1;
	}
	return si;
}
```

### 2. Minimum number of days to make m bouquets

[https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)

```cpp
bool isPossible(vector<int>& bloomDay, int day, int m, int k){
	int size = bloomDay.size(), i = 0, tempK = k;
	while(i < size){
		if(bloomDay[i] > day) tempK = k;
		else tempK--;
		if(tempK == 0){
			tempK = k;
			m--;
		}
		i++;
	}
	if(m <= 0) return true;
	return false;
}

int minDays(vector<int>& bloomDay, int m, int k) {
	int size = bloomDay.size(), ans = INT_MAX;
	int si = 1, ei = *max_element(bloomDay.begin(), bloomDay.end());
	while(si <= ei){
		int mi = si + (ei - si) / 2;
		bool possible = isPossible(bloomDay, mi, m, k);
		if(possible){
			ans = min(ans, mi);
			ei = mi - 1;
		}
		else{
			si = mi + 1;
		}
	}
	return ans == INT_MAX ? -1 : ans;
}
```

### 3. Smallest divisor given a threshold

[https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/)

```cpp
int divSum(vector<int>& nums, int divisor){
	int size = nums.size(), i = 0, ans = 0;
	while(i < size){
		ans += ceil((double)nums[i] / (double)divisor);
		i++;
	}
	return ans;
}

int smallestDivisor(vector<int>& nums, int threshold) {
	int size = nums.size(), si = 1, ei = *max_element(nums.begin(), nums.end());

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		int tempAns = divSum(nums, mi);
		cout << mi << ' ' << tempAns << endl;
		if(tempAns > threshold) si = mi + 1;
		else ei = mi - 1;
	}
	return si;
}
```

### 4. Capacity to ship within D days

[https://leetcode.com/problems/capacity-to-ship-packages-within-d-days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days)

```cpp
int calcDays(vector<int>& weights, int capacity){
	int size = weights.size(), i = 0, sum = 0, days = 0;
	while(i < size){
		if(sum + weights[i] > capacity) sum = weights[i], days++;
		else sum += weights[i];
		i++;
	}
	if(sum > 0) days++;
	return days;
}

int shipWithinDays(vector<int>& weights, int days) {
	int size = weights.size(), sum = accumulate(weights.begin(), weights.end(), 0);
	int si = *max_element(weights.begin(), weights.end()), ei = sum;

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		int daysTaken = calcDays(weights, mi);
		if(daysTaken <= days) ei = mi - 1;
		else si = mi + 1;
	}

	return si;
}
```

### 5. Aggressive Cows

[https://www.codingninjas.com/studio/problems/aggressive-cows_1082559](https://www.codingninjas.com/studio/problems/aggressive-cows_1082559)

```cpp
bool canPlaceCows(vector<int>& stalls, int distance, int cows){
    int size = stalls.size(), i = 1, previousCow = 0;
    cows--;
    while(i < size){
        if(stalls[i] - stalls[previousCow] >= distance){
            previousCow = i;
            cows--;
        }
        i++;
    }
    if(cows <= 0) return true;
    return false;
}

int aggressiveCows(vector<int> &stalls, int k)
{
    // Best solution is O(nlogn)
    sort(stalls.begin(), stalls.end());
    int size = stalls.size(), si = 1, ei = stalls[size - 1] - stalls[0];

    while(si <= ei){
        int mi = si + (ei - si) / 2;
        bool isPossible = canPlaceCows(stalls, mi, k);
        if(isPossible) si = mi + 1;
        else ei = mi - 1;
    }
    return ei;
}
```

### 6. Allocate Books

[https://www.codingninjas.com/studio/problems/allocate-books_1090540](https://www.codingninjas.com/studio/problems/allocate-books_1090540)

```cpp
int countStudents(vector<int>& nums, int pages){
    int size = nums.size(), i = 1, sum = nums[0], students = 0;
    while(i < size){
        if(sum + nums[i] > pages) sum = nums[i], students++;
        else sum += nums[i];
        i++;
    }
    if(sum > 0) students += ceil((double)sum / (double)pages);
    return students;
}

int findPages(vector<int>& arr, int n, int m) {
    if(m > n) return -1;

    int size = arr.size();
    int si = *max_element(arr.begin(), arr.end()), ei = accumulate(arr.begin(), arr.end(), 0);
    while(si <= ei){
        int mi = si + (ei - si) / 2;
        int requiredStudents = countStudents(arr, mi);
        if(requiredStudents > m) si = mi + 1;
        else ei = mi - 1;
    }
    return si;
}
```

### 7. Split array largest sum

[https://leetcode.com/problems/split-array-largest-sum/](https://leetcode.com/problems/split-array-largest-sum/)

```cpp
int countSubArrays(vector<int>& nums, int limit){
	int size = nums.size(), i = 0, currentSum = 0, subArrays = 0;
	while(i < size){
		if(nums[i] + currentSum > limit) currentSum = nums[i], subArrays++;
		else currentSum += nums[i];
		i++;
	}
	subArrays += ceil((double)currentSum / (double)limit);
	return subArrays;
}

int splitArray(vector<int>& nums, int k) {
	int size = nums.size();

	if(size == 1) return nums[size - 1];
	if(size == 0) return 0;

	int si = *max_element(nums.begin(), nums.end());
	int ei = accumulate(nums.begin(), nums.end(), 0);

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		int subArrays = countSubArrays(nums, mi);
		if(subArrays > k) si = mi + 1;
		else ei = mi - 1;
	}

	return si;
}
```

### 8. Painter's partition problem

[https://www.codingninjas.com/studio/problems/painter-s-partition-problem_1089557](https://www.codingninjas.com/studio/problems/painter-s-partition-problem_1089557)

```cpp
int getPainters(vector<int>& boards, int time){
    int size = boards.size(), i = 0, currentTime = 0, painters = 0;
    while(i < size){
        if(boards[i] + currentTime > time) currentTime = boards[i], painters++;
        else currentTime += boards[i];
        i++;
    }
    painters += ceil((double)currentTime / (double)time);
    return painters;
}

int findLargestMinDistance(vector<int> &boards, int k)
{
    int size = boards.size();
    int si = *max_element(boards.begin(), boards.end()), ei = accumulate(boards.begin(), boards.end(), 0);

    while(si <= ei){
        int mi = si + (ei - si) / 2;
        int painters = getPainters(boards, mi);
        if(painters > k) si = mi + 1;
        else ei = mi - 1;
    }
    return si;
}
```

### 9. Median of two sorted arrays

[https://leetcode.com/problems/median-of-two-sorted-arrays](https://leetcode.com/problems/median-of-two-sorted-arrays)

```cpp
double optimal(vector<int>& numsOne, vector<int>& numsTwo){
	// Optimal solution relies on splitting the min(numsOne, numsTwo) into 2 parts,
	// such that the resulting arrays are equal in length and collectively sorted
	// We can then find out the median by math magic, making it O(log(n + m))
	int sizeOne = numsOne.size(), sizeTwo = numsTwo.size(), valOne, valTwo;
	if(sizeOne > sizeTwo) return optimal(numsTwo, numsOne);
	// Here si, ei and median denote the number of elements selected from numsOne,
	// not indices
	int si = 0, ei = sizeOne, median = (sizeOne + sizeTwo + 1) / 2;

	while(si <= ei){
		int mi = si + (ei - si) / 2;
		int miTwo = median - mi;
		int leftOne = INT_MIN, leftTwo = INT_MIN;
		int rightOne = INT_MAX, rightTwo = INT_MAX;

		if(mi > 0) leftOne = numsOne[mi - 1];
		if(mi < sizeOne) rightOne = numsOne[mi];
		if(miTwo > 0 && miTwo <= sizeTwo) leftTwo = numsTwo[miTwo - 1];
		if(miTwo < sizeTwo) rightTwo = numsTwo[miTwo];

		if(leftOne <= rightTwo && leftTwo <= rightOne){
			valOne = max(leftOne, leftTwo);
			valTwo = min(rightOne, rightTwo);
			break;
		}
		if(leftOne > rightTwo) ei = mi - 1;
		else si = mi + 1;
	}
	if((sizeOne + sizeTwo) % 2 == 0){
		return ((double)valOne + (double)valTwo) / 2.0;
	}
	return valOne;
}

double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
	return optimal(nums1, nums2);
}
```

### 10. Minimise max distance to gas station

[https://www.codingninjas.com/studio/problems/minimise-max-distance_7541449](https://www.codingninjas.com/studio/problems/minimise-max-distance_7541449)

```cpp
int countStations(vector<int>& nums, double distance){
    int size = nums.size(), i = 0, sum = 0;
    while(i < size - 1){
        int difference = nums[i + 1] - nums[i];
        int stations = difference / distance;
        if(difference == distance * stations) stations--;
        sum += stations;
        i++;
    }
    return sum;
}

double optimal(vector<int>& nums, int gasStations){
    // Optimal solution is O(nlogn) time, and O(1) space
    int size = nums.size();
    double si = 0, ei = maxDifference(nums);
    while(ei - si > 1e-6){
        double mi = si + (ei - si) / 2;
        int stations = countStations(nums, mi);
        if(stations > gasStations) si = mi;
        else ei = mi;
    }
    // Either use another variable for ans,
    // or just observe the pattern on a paper
    return ei;
}

double minimiseMaxDistance(vector<int> &arr, int k){
	return optimal(arr, k);
}
```

### 11. Kth element of two sorted arrays

[https://www.codingninjas.com/studio/problems/k-th-element-of-2-sorted-array_1164159](https://www.codingninjas.com/studio/problems/k-th-element-of-2-sorted-array_1164159)

```cpp
int kthElement(vector<int> &arr1, vector<int>& arr2, int n, int m, int k){
    int sizeOne = n, sizeTwo = m;
    if(sizeOne > sizeTwo) return kthElement(arr2, arr1, m, n, k);

    int si = max(k - sizeTwo, 0), ei = min(k, sizeOne);
    int leftOne, rightOne, leftTwo, rightTwo;
    while(si <= ei){
        int mi = si + (ei - si) / 2;
        int miTwo = k - mi;

        leftOne = INT_MIN, leftTwo = INT_MIN;
        rightOne = INT_MAX, rightTwo = INT_MAX;

        if(mi > 0) leftOne = arr1[mi - 1];
        if(mi < sizeOne) rightOne = arr1[mi];
        if(miTwo > 0 && miTwo <= sizeTwo) leftTwo = arr2[miTwo - 1];
        if(miTwo < sizeTwo) rightTwo = arr2[miTwo];

        if(leftOne <= rightTwo && leftTwo <= rightOne) break;
        if(leftOne < rightTwo) si = mi + 1;
        else ei = mi - 1;
    }

    return max(leftOne, leftTwo);
}
```
