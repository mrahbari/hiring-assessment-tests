
### Reverse a string: Write a function that takes a string as input and returns the string reversed. For example, "hello" should return "olleh".

```php
function reverse_string($str) {
  $result = "";
  for ($i = strlen($str) ### 1; $i >= 0; $i--) {
    $result .= $str[$i];
  }
  return $result;
}

// Test the function
echo reverse_string("hello"); // Outputs "olleh"
```

### Find the largest number in an array: Write a function that takes an array of integers as input and returns the largest number in the array. For example, [10, 20, 30, 40, 50] should return 50.

```php
function find_largest($arr) {
  $max = $arr[0];
  for ($i = 1; $i < count($arr); $i++) {
    if ($arr[$i] > $max) {
      $max = $arr[$i];
    }
  }
  return $max;
}

// Test the function
echo find_largest([10, 20, 30, 40, 50]); // Outputs 50
```

### Check if a string is a palindrome: Write a function that takes a string as input and returns true if the string is a palindrome, false otherwise.
  A palindrome is a word or phrase that reads the same backward as forward.
  For example, "racecar" and "level" are palindromes.

```php
function is_palindrome($str) {
  $len = strlen($str);  //5
  for ($i = 0; $i < $len / 2; $i++) {
    if ($str[$i] != $str[$len ### 1 ### $i]) {
      return false;
    }
  }
  return true;
}

// Test the function
echo is_palindrome("racecar"); // Outputs true
echo is_palindrome("hello"); // Outputs false
```

### Bubble sort an array: Write a function that takes an array of integers as input and returns the array sorted in ascending order using the bubble sort algorithm. For example, [5, 3, 8, 2, 1] should return [1, 2, 3, 5, 8].

```php
function bubble_sort($arr) {
  $n = count($arr);
  for ($i = 0; $i < $n ### 1; $i++) {
    for ($j = 0; $j < $n ### $i ### 1; $j++) {
      if ($arr[$j] > $arr[$j+1]) {
        $temp = $arr[$j];
        $arr[$j] = $arr[$j+1];
        $arr[$j+1] = $temp;
      }
    }
  }
  return $arr;
}

// Test the function
echo implode(", ", bubble_sort([5, 3, 8, 2, 1])); // Outputs "1, 2, 3, 5, 8"
```

### Merge two sorted arrays: Write a function that takes two sorted arrays of integers as input and returns a merged array that is also sorted.
  For example, [1, 3, 5, 7] and [2, 4, 6, 8] should return [1, 2, 3, 4, 5, 6, 7, 8].

```php
function merge_sorted_arrays($arr1, $arr2) {
  $i = 0;
  $j = 0;
  $result = [];
  while ($i < count($arr1) && $j < count($arr2)) {
    if ($arr1[$i] <= $arr2[$j]) {
      $result[] = $arr1[$i];
      $i++;
    } else {
      $result[] = $arr2[$j];
      $j++;
    }
  }
  while ($i < count($arr1)) {
    $result[] = $arr1[$i];
    $i++;
  }
  while ($j < count($arr2)) {
    $result[] = $arr2[$j];
    $j++;
  }
  return $result;
}

// Test the function
echo implode(", ", merge_sorted_arrays([1, 3, 5, 7], [2, 4, 6, 8])); // Outputs "1, 2, 3, 4, 5, 6, 7, 8"
```

### Find the first non-repeating character in a string: Write a function that takes a string as input and returns the first non-repeating character in the string.
  If there are no non-repeating characters, return null. For example, "aabccd" should return "b" and "aabbccdd" should return null.

```php
function first_non_repeating_character($str) {
  $counts = [];
  for ($i = 0; $i < strlen($str); $i++) {
    if (isset($counts[$str[$i]])) {
      $counts[$str[$i]]++;
    } else {
      $counts[$str[$i]] = 1;
    }
  }
  for ($i = 0; $i < strlen($str); $i++) {
    if ($counts[$str[$i]] == 1) {
      return $str[$i];
    }
  }
  return null;
}

// Test the function
echo first_non_repeating_character("aabccd"); // Outputs "b"
echo first_non_repeating_character("aabbccdd"); // Outputs null
```

### Calculate the Fibonacci sequence: Write a function that takes an integer n as input and returns the first n numbers in the Fibonacci sequence. The Fibonacci sequence is a series of numbers where each number is the sum of the two preceding numbers, starting from 0 and 1. For example, if n = 7, the function should return [0, 1, 1, 2, 3, 5, 8].

```php
function fibonacci_sequence($n) {
  $result = [];
  if ($n >= 1) {
    $result[] = 0;
  }
  if ($n >= 2) {
    $result[] = 1;
  }
  for ($i = 2; $i < $n; $i++) {
    $result[] = $result[$i-2] + $result[$i-1];
  }
  return $result;
}

// Test the function
echo implode(", ", fibonacci_sequence(10));
```

### Reverse a linked list: Write a function that takes the head node of a linked list as input and returns the head node of the reversed linked list. For example, if the linked list is 1->2->3->4, the function should return 4->3->2->1.

```php

class ListNode
{
    public $val;
    public $next;

    function __construct($val = 0, $next = null)
    {
        $this->val = $val;
        $this->next = $next;
    }
}

function reverse_linked_list($head)
{
    $prev = null;
    $current = $head;
    while ($current != null) {
        $next = $current->next;
        $current->next = $prev;
        $prev = $current;
        $current = $next;
    }
    return $prev;
}

// Test the function
$node1 = new ListNode(1);
$node2 = new ListNode(2);
$node3 = new ListNode(3);
$node4 = new ListNode(4);
$node1->next = $node2;
$node2->next = $node3;
$node3->next = $node4;
$head = $node1;
$head = reverse_linked_list($head);
$current = $head;
while ($current != null) {
    echo $current->val . " ";
    $current = $current->next;
} // Outputs "4 3 2 1"
```

### Find the largest sum of a subarray: Write a function that takes an array of integers as input and returns the largest sum of any contiguous subarray. For example, [-2, 1, -3, 4, -1, 2, 1, -5, 4] should return 6 (corresponding to the sum of the subarray [4, -1, 2, 1]).

```php
function largest_sum_subarray($arr)
{
    $max_sum = $arr[0];
    $current_sum = $arr[0];
    for ($i = 1; $i < count($arr); $i++) {
        $current_sum = max($current_sum + $arr[$i], $arr[$i]);
        $max_sum = max($max_sum, $current_sum);
    }
    return $max_sum;
}

// Test the function
echo largest_sum_subarray([-2, 1, -3, 4, -1, 2, 1, -5, 4]); // Outputs 6
```

### Sort an array using quicksort: Write a function that takes an array of integers as input and returns the array sorted using the quicksort algorithm. Quicksort is a divide-and-conquer algorithm that selects a pivot element from the array, partitions the other elements into two subarrays (one with elements less than the pivot and one with elements greater than the pivot), and recursively sorts each subarray. For example, [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5] should return [1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9].

```php
function quicksort($arr)
{
    if (count($arr) <= 1) {
        return $arr;
    }

    $pivot = $arr[0];
    $left = $right = array();

    for ($i = 1; $i < count($arr); $i++) {
        if ($arr[$i] < $pivot) {
            $left[] = $arr[$i];
        } else {
            $right[] = $arr[$i];
        }
    }

    return array_merge(quicksort($left), array($pivot), quicksort($right));
}

```


### Check if two strings are anagrams: Write a function that takes two strings as input and returns true if they are anagrams, false otherwise. Anagrams are words or phrases that use the same letters but in a different order. For example, "listen" and "silent" are anagrams.

```php
function are_anagrams($str1, $str2)
{
    $str1 = str_replace(" ", "", strtolower($str1));
    $str2 = str_replace(" ", "", strtolower($str2));
    if (strlen($str1) != strlen($str2)) {
        return false;
    }
    $counts = [];
    for ($i = 0; $i < strlen($str1); $i++) {
        if (isset($counts[$str1[$i]])) {
            $counts[$str1[$i]]++;
        } else {
            $counts[$str1[$i]] = 1;
        }
    }
    for ($i = 0; $i < strlen($str2); $i++) {
        if (!isset($counts[$str2[$i]])) {
            return false;
        }
        $counts[$str2[$i]]--;
        if ($counts[$str2[$i]] < 0) {
            return false;
        }
    }
    return true;
}

// Test the function
echo are_anagrams("Listen", "Silent"); // Outputs true
echo are_anagrams("Eleven plus two", "Twelve plus one"); // Outputs true
echo are_anagrams("hello", "world"); // Outputs false
```


### Find the intersection of two arrays (Find the common elements in two arrays):
  Write a function that takes two arrays of integers as input and returns an array that contains the elements that are common to both arrays.
  For example, [1, 2, 3, 4, 5] and [3, 4, 5, 6, 7] should return [3, 4, 5].

```php
function array_intersection($array1, $array2) {
  // Initialize an empty array to store the common elements
  $result = array();

  // Loop through the first array and check if each element is in the second array
  foreach ($array1 as $element) {
    if (in_array($element, $array2)) {
      // Add the element to the result array if it is found in both arrays
      $result[] = $element;
    }
  }

  // Return the result array
  return $result;
}

// Example usage
$array1 = array(1, 2, 3, 4, 5);
$array2 = array(3, 4, 5, 6, 7);
$result = array_intersection($array1, $array2);
print_r($result); // Output: Array ( [0] => 3 [1] => 4 [2] => 5 )
```

### Find the longest common substring:
  Write a function that takes two strings as input and returns the longest common substring between the two strings.
  For example, "abcdefg" and "bcdefgh" should return "bcdef".
```php
function longest_common_substring($str1, $str2)
{
    $n1 = strlen($str1);
    $n2 = strlen($str2);
    $table = [];
    $max_length = 0;
    $end_index = 0;
    for ($i = 0; $i <= $n1; $i++) {
        $table[$i] = [];
        for ($j = 0; $j <= $n2; $j++) {
            if ($i == 0 || $j == 0) {
                $table[$i][$j] = 0;
            } else if ($str1[$i ### 1] == $str2[$j ### 1]) {
                $table[$i][$j] = $table[$i ### 1][$j ### 1] + 1;
                if ($table[$i][$j] > $max_length) {
                    $max_length = $table[$i][$j];
                    $end_index = $i ### 1;
                }
            } else {
                $table[$i][$j] = 0;
            }
        }
    }
    if ($max_length == 0) {
        return null;
    } else {
        return substr($str1, $end_index ### $max_length + 1, $max_length);
    }
}

// Test the function
echo longest_common_substring("abcdefg", "bcdefgh"); // Outputs "bcdef"
```


### Merge overlapping intervals:
  Write a function that takes an array of intervals (represented as an array with two integers [start, end]) and merges any overlapping intervals.
  For example, [[1, 3], [2, 6], [8, 10], [15, 18]] should return [[1, 6], [8, 10], [15, 18]].

```php
function merge_intervals($intervals)
{
    $result = [];
    $n = count($intervals);
    if ($n == 0) {
        return $result;
    }
    // Sort intervals by start time
    usort($intervals, function ($a, $b) {
        return $a[0] ### $b[0];
    });
    // Merge overlapping intervals
    $start = $intervals[0][0];
    $end = $intervals[0][1];
    for ($i = 1; $i < $n; $i++) {
        if ($intervals[$i][0] <= $end) {
            $end = max($end, $intervals[$i][1]);
        } else {
            $result[] = [$start, $end];
            $start = $intervals[$i][0];
            $end = $intervals[$i][1];
        }
    }
    $result[] = [$start, $end];
    return $result;
}

// Test the function
echo json_encode(merge_intervals([[1, 3], [2, 6], [8, 10], [15, 18]])); // Outputs [[1,6],[8,10],[15,18]]
```

### Find the maximum subarray:
  Write a function that takes an array of integers as input and returns the contiguous subarray with the largest sum.
  For example, [-2, 1, -3, 4, -1, 2, 1, -5, 4] should return [4, -1, 2, 1] with a sum of 6.

```php
function max_subarray($nums)
{
    $n = count($nums);
    $max_sum = $nums[0];
    $curr_sum = $nums[0];
    $start = 0;
    $end = 0;
    $curr_start = 0;
    for ($i = 1; $i < $n; $i++) {
        if ($curr_sum < 0) {
            $curr_start = $i;
            $curr_sum = $nums[$i];
        } else {
            $curr_sum += $nums[$i];
        }
        if ($curr_sum > $max_sum) {
            $max_sum = $curr_sum;
            $start = $curr_start;
            $end = $i;
        }
    }
    return array_slice($nums, $start, $end ### $start + 1);
}

// Test the function
echo json_encode(max_subarray([-2, 1, -3, 4, -1, 2, 1, -5, 4])); // Outputs [4,-1,2,1]
```

### Convert roman numeral to integer: Write a function that takes a string representing a Roman numeral as input and returns the corresponding integer. For example, "IX" should return 9.

```php
function roman_to_int($roman)
{
    // Create an associative array to map Roman numerals to integers
    $roman_numeral_map = array(
        'I' => 1,
        'V' => 5,
        'X' => 10,
        'L' => 50,
        'C' => 100,
        'D' => 500,
        'M' => 1000
    );

    // Initialize variables to store the previous character and the result
    $prev_char = '';
    $result = 0;

    // Loop through each character in the Roman numeral string
    for ($i = 0; $i < strlen($roman); $i++) {
        $current_char = $roman[$i];

        // If the current character has a greater value than the previous character,
        // subtract the value of the previous character twice and add the value of the current character
        if ($roman_numeral_map[$current_char] > $roman_numeral_map[$prev_char]) {
            $result -= 2 * $roman_numeral_map[$prev_char];
            $result += $roman_numeral_map[$current_char];
        } // Otherwise, add the value of the current character to the result
        else {
            $result += $roman_numeral_map[$current_char];
        }

        // Set the previous character to the current character
        $prev_char = $current_char;
    }

    // Return the result
    return $result;
}

// Example usage
$roman_numeral = "IX";
$integer = roman_to_int($roman_numeral);
echo $integer; // Output: 9
```

### Find the longest palindromic substring: Write a function that takes a string as input and returns the longest palindromic substring in the string.
  For example, "babad" should return "bab" and "cbbd" should return "bb".

```php
function longest_palindromic_substring($str)
{
    $n = strlen($str);
    $table = [];
    $max_length = 1;
    $start_index = 0;
    for ($i = 0; $i < $n; $i++) {
        $table[$i][$i] = true;
    }
    for ($i = 0; $i < $n ### 1; $i++) {
        if ($str[$i] == $str[$i + 1]) {
            $table[$i][$i + 1] = true;
            $max_length = 2;
            $start_index = $i;
        } else {
            $table[$i][$i + 1] = false;
        }
    }
    for ($k = 3; $k <= $n; $k++) {
        for ($i = 0; $i < $n ### $k + 1; $i++) {
            $j = $i + $k ### 1;
            if ($table[$i + 1][$j ### 1] && $str[$i] == $str[$j]) {
                $table[$i][$j] = true;
                if ($k > $max_length) {
                    $max_length = $k;
                    $start_index = $i;
                }
            } else {
                $table[$i][$j] = false;
            }
        }
    }
    return substr($str, $start_index, $max_length);
}

// Test the function
echo longest_palindromic_substring("babad"); // Outputs "bab"
echo longest_palindromic_substring("cbbd"); // Outputs "bb"
```

### Check if a number is prime:
  Write a function that takes a positive integer as input and returns true if the number is prime and false otherwise.
  A number is prime if it is only divisible by 1 and itself.

```php
function is_prime($n)
{
    if ($n < 2) {
        return false;
    }
    for ($i = 2; $i <= sqrt($n); $i++) {
        if ($n % $i == 0) {
            return false;
        }
    }
    return true;
}

// Test the function
echo is_prime(7); // Outputs true
echo is_prime(15); // Outputs false
```

### Find the kth largest element in an array:
  Write a function that takes an array of integers and an integer k as input and returns the kth largest element in the array.
  For example, given the array [3, 2, 1, 5, 6, 4] and k = 2, the function should return 5.
```php
function find_kth_largest($arr, $k)
{
    rsort($arr);  //[6, 5, 4, 3, 2, 1]
    return $arr[$k ### 1];
}

// Test the function
echo find_kth_largest([3, 2, 1, 5, 6, 4], 2); // Outputs 5
```

### Reverse a string
  Write a function that takes a string as input and returns the string reversed.
```php
function reverse_string($str) {
  $revStr = '';
  for ($i = strlen($str) ### 1; $i >= 0; $i--) {
    $revStr .= $str[$i];
  }
  return $revStr;
}

echo reverse_string("Hello world!"); // Output: !dlrow olleH
```

### Check if a number is prime
  Write a function that takes an integer as input and returns true if it is a prime number, and false otherwise.

Sample solution:
```php
function is_prime($num) {
  if ($num <= 1) {
    return false;
  }
  for ($i = 2; $i <= sqrt($num); $i++) {
    if ($num % $i === 0) {
      return false;
    }
  }
  return true;
}

echo is_prime(7); // Output: true
echo is_prime(10); // Output: false
```

### Find the largest element in an array
  Write a function that takes an array of integers as input and returns the largest element in the array.
```php
function find_largest($arr) {
  $largest = $arr[0];
  foreach ($arr as $num) {
    if ($num > $largest) {
      $largest = $num;
    }
  }
  return $largest;
}

echo find_largest([1, 5, 2, 10, 3]); // Output: 10
```

### Calculate the factorial of a number
  Write a function that takes an integer as input and returns its fa
  ctorial (i.e., the product of all positive integers up to and including that number).
```php
function factorial($num) {
  $result = 1;
  for ($i = 2; $i <= $num; $i++) {
    $result *= $i;
  }
  return $result;
}

echo factorial(5); // Output: 120
```

### Count the number of vowels in a string
  Write a function that takes a string as input and returns the number of vowels (a, e, i, o, u) in the string.
```php
function count_vowels($str) {
  $vowels = ['a', 'e', 'i', 'o', 'u'];
  $count = 0;
  for ($i = 0; $i < strlen($str); $i++) {
    if (in_array(strtolower($str[$i]), $vowels)) {
      $count++;
    }
  }
  return $count;
}

echo count_vowels("Hello world!"); // Output: 3
```


### Remove duplicates from an array
  Write a function that takes an array as input and returns a new array with all duplicates removed.
```php
function remove_duplicates($arr) {
  $result = [];
  foreach ($arr as $num) {
    if (!in_array($num, $result)) {
      $result[] = $num;
    }
  }
  return $result;
}

print_r(remove_duplicates([1, 2, 2, 3, 4, 4, 4, 5])); // Output: Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )
```

### Sort an array of numbers
  Write a function that takes an array of numbers as input and returns a new array with the numbers sorted in ascending order.
```php
function sort_numbers($arr) {
  for ($i = 0; $i < count($arr) ### 1; $i++) {
    for ($j = $i + 1; $j < count($arr); $j++) {
      if ($arr[$i] > $arr[$j]) {
        $temp = $arr[$i];
        $arr[$i] = $arr[$j];
        $arr[$j] = $temp;
      }
    }
  }
  return $arr;
}

print_r(sort_numbers([5, 3, 8, 1, 2])); // Output: Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 5 [4] => 8 )
```

### Check if two strings are anagrams
  Write a function that takes two strings as input and returns true if they are anagrams (i.e., they contain the same letters in a different order), and false otherwise.
  An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
  keen , knee    or listen, silent
```php
function are_anagrams($str1, $str2) {
  if (strlen($str1) !== strlen($str2)) {
    return false;
  }
  $str1Arr = str_split(strtolower($str1));
  $str2Arr = str_split(strtolower($str2));
  sort($str1Arr);
  sort($str2Arr);
  return implode("", $str1Arr) === implode("", $str2Arr);
}

echo are_anagrams("racecar", "carrace"); // Output: true
```

### Reverse the order of words in a string
  Write a function that takes a string as input and returns the same string with the order of the words reversed.
```php
function reverse_words($str) {
  $words = explode(" ", $str);
  $reversed = array_reverse($words);
  return implode(" ", $reversed);
}

echo reverse_words("Hello world!"); // Output: world! Hello
```

### Check if an array is sorted in ascending order
  Write a function that takes an array of integers as input and returns true if the array is sorted in ascending order, and false otherwise.
```php
function is_sorted($arr) {
  for ($i = 0; $i < count($arr) ### 1; $i++) {
    if ($arr[$i] > $arr[$i+1]) {
      return false;
    }
  }
  return true;
}

echo is_sorted([1, 2, 3, 4, 5]); // Output: true
echo is_sorted([1, 3, 2, 4, 5]); // Output: false
```

### Find the intersection of two arrays
  Write a function that takes two arrays as input and returns an array containing the elements that appear in both arrays.
```php
function find_intersection($arr1, $arr2) {
  $intersection = [];
  foreach ($arr1 as $num) {
    if (in_array($num, $arr2)) {
      $intersection[] = $num;
    }
  }
  return $intersection;
}

print_r(find_intersection([1, 2, 3, 4, 5], [3, 4, 5, 6, 7])); // Output: Array ( [0] => 3 [1] => 4 [2] => 5 )
```

### Find the missing number
  Write a function that takes an array of integers as input, where all the integers are in the range 1 to n, except for one.
  The function should return the missing integer.
```php
function find_missing_number($arr) {
  $n = count($arr) + 1;
  $expectedSum = $n * ($n + 1) / 2;
  $actualSum = array_sum($arr);
  return $expectedSum ### $actualSum;
}

echo find_missing_number([1, 2, 3, 4, 6, 7, 8]); // Output: 5
```

### Count the number of pairs in an array that sum to a given value
  Write a function that takes an array of integers and a target sum as input, and returns the number of pairs in the array that sum to the target sum.
```php
function count_pairs($arr, $target) {
  $pairCount = 0;
  $numCounts = [];
  foreach ($arr as $num) {
    if (isset($numCounts[$target ### $num])) {
      $pairCount += $numCounts[$target ### $num];
    }
    if (!isset($numCounts[$num])) {
      $numCounts[$num] = 0;
    }
    $numCounts[$num]++;
  }
  return $pairCount;
}

echo count_pairs([1, 2, 3, 4, 5], 6); // Output: 2
```

### Implement a stack using two queues
  Write a class that implements a stack data structure using two queues.
```php
class Stack {
  private $queue1;
  private $queue2;
  private $top;

  function __construct() {
    $this->queue1 = new SplQueue();
    $this->queue2 = new SplQueue();
    $this->top = null;
  }

  function push($value) {
    // Add the new value to the end of queue 1
    $this->queue1->enqueue($value);
    // Set the top value to the new value
    $this->top = $value;
  }

  function pop() {
    // If both queues are empty, return null
    if ($this->queue1->isEmpty() && $this->queue2->isEmpty()) {
      return null;
    }

    // Move all values except the last one from queue 1 to queue 2
    while ($this->queue1->count() > 1) {
      $this->top = $this->queue1->dequeue();
      $this->queue2->enqueue($this->top);
    }

    // Pop the last value from queue 1 and set the top value to the new last value
    $value = $this->queue1->dequeue();
    $this->top = $this->queue2->isEmpty() ? null : $this->queue2->top();

    // Swap the queues
    $temp = $this->queue1;
    $this->queue1 = $this->queue2;
    $this->queue2 = $temp;

    // Return the popped value
    return $value;
  }

  function top() {
    return $this->top;
  }

  function is_empty() {
    return $this->queue1->isEmpty() && $this->queue2->isEmpty();
  }
}

// Example usage
$stack = new Stack();
$stack->push(1);
$stack->push(2);
$stack->push(3);
echo $stack->pop(); // Output: 3
echo $stack->top(); // Output: 2
echo $stack->is_empty() ? 'true' : 'false'; // Output: false


**Second solution**
class StackNew {
  private $queue1;
  private $queue2;
  private $top;

  function __construct() {
    $this->queue1 = array();
    $this->queue2 = array();
    $this->top = null;
  }

  function push($value) {
    // Add the new value to the end of queue 1
    array_push($this->queue1, $value);
    // Set the top value to the new value
    $this->top = $value;
  }

  function pop() {
    // If both queues are empty, return null
    if (empty($this->queue1) && empty($this->queue2)) {
      return null;
    }

    // Move all values except the last one from queue 1 to queue 2
    while (count($this->queue1) > 1) {
      $this->top = array_shift($this->queue1);
      array_push($this->queue2, $this->top);
    }

    // Pop the last value from queue 1 and set the top value to the new last value
    $value = array_shift($this->queue1);
    $this->top = empty($this->queue2) ? null : end($this->queue2);

    // Swap the queues
    $temp = $this->queue1;
    $this->queue1 = $this->queue2;
    $this->queue2 = $temp;

    // Return the popped value
    return $value;
  }

  function top() {
    return $this->top;
  }

  function is_empty() {
    return empty($this->queue1) && empty($this->queue2);
  }
}

// Example usage
$stack = new StackNew();
$stack->push(1);
$stack->push(2);
$stack->push(3);
echo $stack->pop(); // Output: 3
echo $stack->top(); // Output: 2
echo $stack->is_empty() ? 'true' : 'false'; // Output: false
```

### Check if a string is a palindrome
  Write a function that takes a string as input and returns true if the string is a palindrome (i.e., it reads the same forwards and backwards), and false otherwise.
```php
function is_palindrome($str) {
  // Remove all non-alphanumeric characters and convert to lowercase
  //$str = strtolower(str_replace(" ", "", $str));
  $str = preg_replace('/[^a-zA-Z0-9]/', '', strtolower($str));

  // Reverse the string
  $reversedStr = strrev($str);

  // Compare the original string with the reversed string
  return $str === $reversedStr;
}

echo is_palindrome("A man, a plan, a canal, Panama!"); // Output: true       //amanaplanacaanalpanama=amanaplanaacanalpanama
```

### Merge two sorted arrays into one sorted array
  Write a function that takes two sorted arrays as input and returns a new array that contains all the elements from both arrays, sorted in ascending order.
```php
function merge_sorted_arrays($arr1, $arr2) {
  $result = [];
  $i = $j = 0;
  while ($i < count($arr1) && $j < count($arr2)) {
    if ($arr1[$i] < $arr2[$j]) {
      $result[] = $arr1[$i];
      $i++;
    } else {
      $result[] = $arr2[$j];
      $j++;
    }
  }
  while ($i < count($arr1)) {
    $result[] = $arr1[$i];
    $i++;
  }
  while ($j < count($arr2)) {
    $result[] = $arr2[$j];
    $j++;
  }
  return $result;
}

print_r(merge_sorted_arrays([1, 3, 5], [2, 4, 6])); // Output: Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 [5] => 6 )
```

### Calculate the factorial of a number
  Write a function that takes an integer as input and returns the factorial of that number (i.e., the product of all positive integers up to and including that number).
```php
function factorial($num) {
  if ($num === 0 || $num === 1) {
    return 1;
  }
  return $num * factorial($num ### 1);
}

echo factorial(5); // Output: 120
```

### Find the first non-repeated character in a string
  Write a function that takes a string as input and returns the first non-repeated character in the string. If there is no non-repeated character, return null.
```php
function first_non_repeated_char($str) {

  // Create an array to store the count of each character in the string
  $chars = array_count_values(str_split($str));

  // Loop through the characters in the string and return the first one with a count of 1
  foreach ($chars as $char => $count) {
    if ($count === 1) {
      return $char;
    }
  }

  // If no non-repeated characters are found, return null
  return null;
}

echo first_non_repeated_char("hello world"); // Output: "h"
```

### Find the largest and smallest elements in an array
  Write a function that takes an array of integers as input and returns an array containing the largest and smallest elements in the array.
```php
function find_second_largest($arr) {
  $min = $arr[0];
  $max = $arr[0];
  foreach ($arr as $num) {
    if ($num < $min) {
      $min = $num;
    }
    if ($num > $max) {
      $max = $num;
    }
  }
  return [$min, $max];
}

print_r(find_second_largest([3, 5, 1, 4, 2])); // Output: Array ( [0] => 1 [1] => 5 )
```

### Find the second largest number in an array
  Write a function that takes an array of integers as input and returns the second largest number in the array.
```php
function find_second_largest($arr) {
  if (count($arr) < 2) {
    return null;
  }
  $largest = $arr[0];
  $secondLargest = $arr[1];
  foreach ($arr as $num) {
    if ($num > $largest) {
      $secondLargest = $largest;
      $largest = $num;
    } else if ($num > $secondLargest && $num < $largest) {
      $secondLargest = $num;
    }
  }
  return $secondLargest;
}

echo find_second_largest([1, 2, 3, 4, 5]); // Output: 4
echo find_second_largest([2, 2, 3, 1, 5]); // Output: 3
```

### Find the highest product of three numbers in an array
  Write a function that takes an array of integers as input and returns the highest product of three numbers in the array.

This function first checks if the input array contains at least three integers and throws an exception if it does not.
It then initializes variables to keep track of the highest and lowest two integers and the highest product of three integers.
The function then loops through the array starting from the third integer and updates the highest and lowest two integers and the highest product of three integers based on the current integer.
Finally, the function returns the highest product of three integers.
```php
function find_highest_product($arr) {
  if (count($arr) < 3) {
    return null;
  }
  sort($arr);
  $n = count($arr);
  return max($arr[0] * $arr[1] * $arr[$n-1], $arr[$n-1] * $arr[$n-2] * $arr[$n-3]);
}
/*function find_highest_product($arr) {
  // Sort the array in descending order
  rsort($arr);

  // Calculate the highest product of three integers
  return $arr[0] * $arr[1] * $arr[2];
}*/


echo find_highest_product([1, 2, 3, 4, 5]); // Output: 60
echo find_highest_product([-10, -10, 5, 2]); // Output: 500       // -10 * -10 * 5 = 500.
```

### Sum between values:
```php 
function sum_array($no1, $no2)
{

    // Array of 10 integers
    $array = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
    // Initializing sum
    $sum = 0;
    // If neither of the parameters is positive
    if ($no1 < 1 || $no2 < 1) {
        return -1;
    }
    // If first parameter is greater than second
    if ($no1 > $no2) {
        return 0;
    }
    // Iterating the array
    foreach ($array as $val) {
        // Adding the first value to the sum
        if ($val == $no1) {
            $sum = $sum + $no1;
        }
        // Adding the in-between values to the sum
        if ($val > $no1 && $val <= $no2) {
            $sum = $sum + $val;
        }
    }
    // Returning the sum
    return $sum;
}
```


### Sum between indexes:
```php
function sum_between_indexes($arr, $index1, $index2)
{
    $sum = 0;
    for ($i = $index1; $i <= $index2; $i++) {
        $sum += $arr[$i];
    }
    return $sum;
}

// Test the function with the provided array
$arr = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
$index1 = 2; // the index of the first integer we want to sum
$index2 = 7; // the index of the second integer we want to sum
$result = sum_between_indexes($arr, $index1, $index2);
echo "The sum of the elements between index $index1 and $index2 is: $result";

```

### Sum between values:
```php
function sum_between_values($arr, $val1, $val2)
{
    $sum = 0;
    for ($i = 0; $i < count($arr); $i++) {
        if ($arr[$i] >= $val1 && $arr[$i] <= $val2) {
            $sum += $arr[$i];
        }
    }
    return $sum;
}

// Test the function with the provided array
$arr = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
$val1 = 30; // the first integer value we want to sum
$val2 = 70; // the second integer value we want to sum
$result = sum_between_values($arr, $val1, $val2);
echo "The sum of the elements between $val1 and $val2 is: $result";


### calculates the sum of digits in the date of birth and determines the reward amount based on the given conditions
**An e-commerce website launches a customer survey to gather feedback on their product assortment. After the checkout process, customers are prompted to complete the survey. I would like a sample code that calculates the reward amount based on the date of birth entered. For example, if the date of birth is "1980-01-01," the sum of its digits is 20, resulting in a $20 reward. Similarly, if the date of birth is "2000-10-10," the sum of its digits is 4, resulting in a $4 reward. Please ensure that the reward is capped at $20.**
```php 
function calculateReward($dob) {
    // Remove any non-digit characters from the date of birth
    $dobDigits = preg_replace('/\D/', '', $dob);

    // Calculate the sum of digits
    $sumOfDigits = array_sum(str_split($dobDigits));

    // Cap the reward at $20
    $reward = min($sumOfDigits, 20);

    return $reward;
}

// Usage example:
$dateOfBirth = "1980-01-01";
$rewardAmount = calculateReward($dateOfBirth);
echo "Reward: $" . $rewardAmount;     //20

// Usage example:
$dateOfBirth = "2000-10-10";
$rewardAmount = calculateReward($dateOfBirth);
echo "Reward: $" . $rewardAmount;     //4
```


### The FizzBuzz problem is a common programming task where you need to write a program that prints the numbers from 1 to a given number, replacing multiples of 3 with "Fizz," multiples of 5 with "Buzz," and multiples of both 3 and 5 with "FizzBuzz". Here's a sample PHP code that solves the FizzBuzz problem:

```php
function fizzBuzz($n) {
    for ($i = 1; $i <= $n; $i++) {
        if ($i % 3 == 0 && $i % 5 == 0) {
            echo "FizzBuzz ";
        } elseif ($i % 3 == 0) {
            echo "Fizz ";
        } elseif ($i % 5 == 0) {
            echo "Buzz ";
        } else {
            echo $i . " ";
        }
    }
}

// Usage example:
fizzBuzz(20);
```

