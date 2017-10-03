Count the number of score combinations from [2, 3, 7]:
```javascript
function combination(n) {
    const points = [2, 3, 7];
    const comb = points.map(() => [1].concat(Array(n).fill(0)));
    points.forEach((val, i) => {
        for (let j = 1; j <= n; j++) {
            const withoutScore = i >= 1 ? comb[i - 1][j] : 0;
            const withScore = j >= points[i] ? comb[i][j - points[i]] : 0;
            comb[i][j] = withoutScore + withScore;
        }
    });
    return comb[points.length - 1][n];
}

combination(12);
```
Use the Levenshtein Algorithm to calculate the minimum changes require for two words:
```javascript
function levenshtein(a, b) {
    const distance = a.split("").map(() => Array(b.length).fill(-1));
    return (function compute(aIndex, bIndex) {
        if (aIndex < 0) {
            return bIndex + 1;
        }
        if (bIndex < 0) {
            return aIndex + 1;
        }
        if (distance[aIndex][bIndex] === -1) {
            if (a[aIndex] === b[bIndex]) {
                distance[aIndex][bIndex] = compute(aIndex - 1, bIndex - 1);
            } else {
                const subLast = compute(aIndex - 1, bIndex - 1);
                const addLast = compute(aIndex - 1, bIndex);
                const deleteLast = compute(aIndex, bIndex - 1);
                distance[aIndex][bIndex] = Math.min(subLast, addLast, deleteLast) + 1;
            }
        }
        return distance[aIndex][bIndex];
    })(a.length - 1, b.length - 1);
}

levenshtein("Saturday", "Sundays"); // 4
```
Count the Number of Ways to traverse a 2D Array:
```javascript
function nWays(n, m) {
    const ways = Array(n).fill().map(() => Array(m).fill(0));
    return (function computed(x, y) {
        if (x === 0 && y === 0) {
            return 1;
        }
        if (ways[x][y] === 0) {
            const waysTop = x === 0 ? 0 : computed(x - 1, y);
            const waysLeft = y === 0 ? 0 : computed(x, y - 1);
            ways[x][y] = waysTop + waysLeft;
        }
        return ways[x][y];
    })(n - 1, m - 1);
}

nWays(5, 5);
```
Calculate Binomial Coefficient:
```javascript
function binomialCoefficient(n, k) {
    const memo = Array(n + 1).fill().map(() => Array(k + 1).fill(0));
    return (function compute(x, y) {
        if ([0, x].includes(y)) {
            return 1;
        }
        if (memo[x][y] === 0) {
            const withoutY = compute(x - 1, y);
            const withY = compute(x - 1, y - 1);
            memo[x][y] = withoutY + withY;
        }
        return memo[x][y];
    })(n, k);
}

binomialCoefficient(5, 2);
```
Search for for the sequence in a 2D array:
```javascript
const arr = [
    [1, 2, 3],
    [3, 4, 5],
    [5, 6, 7]
];

function patternSearch(grid, pattern) {
    const result = [];
    return (function search(row, col) {
        if (!result.length && pattern[result.length] !== grid[row][col]) {
            return false;
        }
        result.push(grid[row][col]);
        if (result.length === pattern.length) {
            return true;
        }
        if (col < grid[0].length - 1 && pattern[result.length] === grid[row][col + 1]) {
            return search(row, col + 1);
        } else if (row < grid.length - 1 && pattern[result.length] === grid[row + 1][col]) {
            return search(row + 1, col);
        }
        return false;
    })(0, 0);
}

patternSearch(arr, [1, 3, 4, 6]);
```
Knapsack Problem:
```javascript
const items = [
    { value: 65, weight: 20 },
    { value: 35, weight: 8 },
    { value: 245, weight: 60 },
    { value: 195, weight: 55 },
    { value: 65, weight: 40 },
    { value: 150, weight: 70 },
    { value: 275, weight: 85 },
    { value: 155, weight: 25 },
    { value: 120, weight: 30 },
    { value: 320, weight: 65 },
    { value: 75, weight: 75 },
    { value: 40, weight: 10 },
    { value: 200, weight: 95 },
    { value: 100, weight: 50 },
    { value: 220, weight: 40 },
    { value: 99, weight: 10 }
];

function optimalCapacity(items, cap) {
    const v = items.map(() => Array(cap + 1).fill(-1));
    return (function compute(k, cap) {
        if (k < 0) {
            return 0;
        }
        if (v[k][cap] === -1) {
            const withoutCurr = compute(k - 1, cap);
            const withCurr = items[k].weight > cap ? 0 : items[k].value + compute(k - 1, cap - items[k].weight);
            v[k][cap] = Math.max(withoutCurr, withCurr);
        }
        return v[k][cap];
    })(items.length - 1, cap);
}

optimalCapacity(items, 130);
```
Find the minimum weight path in triangle:
```javascript
const tri = [[2], [4, 4], [8, 5, 6], [4, 2, 6, 2], [1, 5, 2, 3, 4]];

function minimumPathWeight(tri) {
    let minPath = [0];
    tri.forEach(row => {
        minPath = row.map((_, i) => row[i] + Math.min(minPath[Math.max(i - 1, 0)], minPath[Math.min(i, minPath.length - 1)]));
    });
    return Math.min(...minPath);
}

minimumPathWeight(tri);
```
Pick up coins for Maximum gain:
```javascript
function maxRevenue(coins) {
    const max = coins.map(() => Array(coins.length).fill(0));
    return (function compute(a, b) {
        if (a > b) {
            return 0;
        }
        if (max[a][b] === 0) {
            const bothAandB = compute(a + 1, b - 1);
            const maxA = coins[a] + Math.min(compute(a + 2, b), bothAandB);
            const maxB = coins[b] + Math.min(bothAandB, compute(a, b - 2));
            max[a][b] = Math.max(maxA, maxB);
        }
        return max[a][b];
    })(0, coins.length - 1);
}

maxRevenue([5, 25, 10, 1]);
```