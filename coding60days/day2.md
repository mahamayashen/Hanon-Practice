# Day 2 Array II
## [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
We can use sliding window technique for this question because it is about subarray (**contiguous sequence**) and requests **minimum length** as the return value.
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        ans = len(nums) + 1 
        left = 0
        s = 0

        for (right, x) in enumerate(nums): # x = nums[right]
            s += x
            while s >= target:
                ans = min(ans, right - left + 1)
                s -= nums[left]
                left += 1
        
        return ans if ans <= len(nums) else 0
```
Here, I set `ans` to a value larger than any possible valid subarray length (`float('inf')` also works).This ganrantees the first valid subarray found will be smaller than the initial value, so the `min()` function will correctly update it. Using a sliding window, **I move the right pointer to expand the window and the left pointer to contract it**, always keeping track of the smallest window whose sum meets or exceeds the target.

## [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        m = [[0] * n for i in range(n)]
        dirs = [(0,1), (1,0), (0,-1), (-1,0)] # right, down, left, up
        row, col, dir_idx = 0, 0, 0

        for num in range(1, n**2 + 1):
            m[row][col] = num
            # check if we need to change direction
            nxt_row, nxt_col = row + dirs[dir_idx][0], col + dirs[dir_idx][1]
            # change direction if we hit the boundary or a non-zero cell
            if nxt_row < 0 or nxt_row >= n or nxt_col < 0 or nxt_col >= n \
            or m[nxt_row][nxt_col] != 0:
                # change direction clockwise and recalculate next cell
                dir_idx = (dir_idx + 1) % 4
                nxt_row, nxt_col = row + dirs[dir_idx][0], col + dirs[dir_idx][1]
            row, col = nxt_row, nxt_col
        
        return m
```
The idea is to fill an n x n matrix with numbers from 1 to n² in a spiral, clockwise order. Instead of keeping track of multiple boundaries or layers, I use something called **“direction vectors.”** These are just pairs of numbers that tell me how to move: right, down, left, or up.

Each time I move, I check if I’m about to go out of the matrix’s bounds or if I’m about to step on a cell that’s already filled. If either of those happens, I “turn” by switching to the next direction and then keep going.

The code is pretty efficient because it only passes through each cell once, and it’s easy to follow because the direction changes are handled automatically by cycling through the direction list.

Siimilarily, for the next question, we can also use **“direction vectors”** to read values from the matrix clockwisely. Additionally, we use **the visited set (or visited tracking)** to prevent revisiting the same cells, which is a common technique in grid problems.

## [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        m, n = len(matrix), len(matrix[0])
        dirs = [(0,1), (1,0), (0,-1),(-1,0)]
        i, j, dir_idx = 0, 0, 0
        visited = set()
        ans = []

        for _ in range(m * n):
            ans.append(matrix[i][j])
            visited.add((i,j))
            nxt_i, nxt_j = i + dirs[dir_idx][0], j + dirs[dir_idx][1]
            if nxt_i < 0 or nxt_i >= m \
            or nxt_j < 0 or nxt_j >= n \
            or (nxt_i, nxt_j) in visited:
                dir_idx = (dir_idx + 1) % 4
                nxt_i, nxt_j = i + dirs[dir_idx][0], j + dirs[dir_idx][1]
            i, j = nxt_i, nxt_j
        
        return ans
```
## [58. 区间和](https://kamacoder.com/problempage.php?pid=1070)
这道题需要用到ACM输入与输出方式，这里是首次尝试这种写法。

I want to enhance the readability, therefore introducing modular functions.

```python
import sys

def read_array(data, index):
    n = int(data[index])
    index += 1
    arr = [int(data[index + i]) for i in range(n)]
    index += n
    return arr, index

def compute_prefix_sum(arr):
    prefix_sum = [0] * len(arr)
    current_sum = 0
    for i in range(len(arr)):
        current_sum += arr[i]
        prefix_sum[i] = current_sum
    return prefix_sum

def process_queries(data, index, prefix_sum):
    results = []
    while index < len(data):
        a = int(data[index])
        b = int(data[index + 1])
        index += 2
        if a == 0:
            results.append(prefix_sum[b])
        else:
            results.append(prefix_sum[b] - prefix_sum[a - 1])
    return results

def main():
    data = sys.stdin.read().split()

    arr, index = read_array(data, 0)
    prefix_sum = compute_prefix_sum(arr)
    results = process_queries(data, index, prefix_sum)

    for result in results:
        print(result)

if __name__ == "__main__":
    main()
```
## [44. 开发商购买土地](https://kamacoder.com/problempage.php?pid=1044)
```python
import sys

def read_grid(data):
    """Reads grid dimensions and values from input data."""
    idx = 0
    n = int(data[idx])
    idx += 1
    m = int(data[idx])
    idx += 1
    
    grid = []
    total_sum = 0
    for _ in range(n):
        row = []
        for _ in range(m):
            num = int(data[idx])
            idx += 1
            row.append(num)
            total_sum += num
        grid.append(row)
    
    return grid, n, m, total_sum

def compute_row_sums(grid):
    """Calculates cumulative sums for each row."""
    return [sum(row) for row in grid]

def compute_col_sums(grid):
    """Calculates cumulative sums for each column."""
    return [sum(row[j] for row in grid) for j in range(len(grid[0]))]

def calculate_min_cut(sums, total_sum):
    """Calculates minimum difference for either horizontal or vertical cuts."""
    min_diff = float('inf')
    current_cut = 0
    for s in sums:
        current_cut += s
        min_diff = min(min_diff, abs(total_sum - 2 * current_cut))
    return min_diff

def main():
    data = sys.stdin.read().split()
    grid, n, m, total_sum = read_grid(data)
    
    # Calculate potential minimum differences
    row_sums = compute_row_sums(grid)
    col_sums = compute_col_sums(grid)
    
    horizontal_min = calculate_min_cut(row_sums, total_sum)
    vertical_min = calculate_min_cut(col_sums, total_sum)
    
    # Find overall minimum difference
    print(min(horizontal_min, vertical_min))

if __name__ == "__main__":
    main()
```
