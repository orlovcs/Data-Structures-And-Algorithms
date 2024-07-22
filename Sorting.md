
## Merge Sort

Runtime: O(n * log(n)) avg and O(n^2) worst case.
Space: O(n * log(n)).

## Radix Sort

Runtime: O(kn) avg and worst case.
Space: Variable.
## Bubble Sort

Runtime: O(n^2) avg and worst case.
Space: O(1).

## Selection Sort

Runtime: O(n^2) avg and worst case.
Space: O(1).

## Merge Sort

Runtime: O(n * log(n)) avg and worst case.
Space: Variable.

```python3
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    # Split the array into two halves
    mid = len(arr) // 2
    left_half = arr[:mid]
    right_half = arr[mid:]

    # Recursively sort each half
    left_sorted = merge_sort(left_half)
    right_sorted = merge_sort(right_half)

    # Merge the sorted halves
    return merge(left_sorted, right_sorted)

def merge(left, right):
    sorted_list = []
    left_index = 0
    right_index = 0

    # Merge the two lists while they have elements
    while left_index < len(left) and right_index < len(right):
        if left[left_index] <= right[right_index]:
            sorted_list.append(left[left_index])
            left_index += 1
        else:
            sorted_list.append(right[right_index])
            right_index += 1

    # If there are remaining elements in the left list
    while left_index < len(left):
        sorted_list.append(left[left_index])
        left_index += 1

    # If there are remaining elements in the right list
    while right_index < len(right):
        sorted_list.append(right[right_index])
        right_index += 1

    return sorted_list
```