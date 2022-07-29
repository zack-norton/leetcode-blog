---
title: LeetCode 73. Set Matrix Zeroes
date: "2022-07-28T21:46:03.284Z"
description: "Solution to LeetCode 73. Set Matrix Zeroes"
---

[Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

>Given an ```m x n``` integer matrix ```matrix```, if an element is ```0```, set its entire row and column to ```0's```.
>
>You must do it in place.

### Memory Array

To solve this problem, we need a way to identify what rows and columns should be zeroed without causing later rows and columns to be incorrectly zeroed out. One approach we could take is to create an array of length $Max(Row,Col)$ to store what rows and columns should be zeroed. The array should be a list of pairs indicating if the ith row or column shoud be indexed, i.e. $Array[3][0]$ identifies the 4th row, $Array[1][1]$ identifies the second column. From here we just loop through the matrix once to identify rows and columns that should be indexed. We then loop through the rows, zeroing out the appropriate rows, and finally loop through the columns, zeroing out the appropriate columns.

```
public void SetZeroes(int[][] matrix) 
{
    int rowN = matrix.Length;
    int colN = matrix[0].Length;
        
    int maxN = Math.Max(rowN,colN);
        
    int[,] zeroes = new int[maxN,2];
        
    for(int i = 0; i < rowN; i++){
        for(int j = 0; j < colN; j++){
            if(matrix[i][j] == 0){
                zeroes[i,0] = 1;
                zeroes[j,1] = 1;
            }
        }
    }
        
    for(int i = 0; i < rowN; i++){
        if(zeroes[i,0] == 1){
            for(int j = 0; j < colN; j++){
                matrix[i][j] = 0;
            }
        }
    }
        
    for(int i = 0; i < colN; i++){
        if(zeroes[i,1] == 1){
            for(int j = 0; j < rowN; j++){
                matrix[j][i] = 0;
            }
        }
    }
}
```

The time complexity for this solution is $O(mn)$ since we are traversing a matrix. The space complexity is $O(Max(Row,Col))$. Is there a way to make the space complexity constant?

### Constant Space

Instead of storing the rows and columns that should be zeroed in a separate array, we can instead mark the first row and first column of the matrix as rows and columns that should be zeroed. To prevent setting all rows to zero, we need to skip to first index (```matrix[0][0]```) and check that after we have converted the rest of the array. We also need to skip the first column and convert that later. We can use a boolean flag to let us know if the first column should be zeroed.

```
public void SetZeroes(int[][] matrix) 
{
    int rowN = matrix.Length;
    int colN = matrix[0].Length;
    bool firstColumn = false;
    
    for(int i = 0; i < rowN; i++){
        
        if(matrix[i][0] == 0){
            firstColumn = true;
        }
        
        for(int j = 1; j < colN; j++){
            if(matrix[i][j] == 0){
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    
    for(int i = 1; i < rowN; i++){
        for(int j = 1; j < colN; j++){
            if(matrix[i][0] == 0 || matrix[0][j] == 0){
                matrix[i][j] = 0;
            }
        }
    }
    
    if(matrix[0][0] == 0){
        for(int i = 0; i < colN; i++){
            matrix[0][i] = 0;
        }
    }
    
    if(firstColumn){
        for(int i = 0; i < rowN; i++){
            matrix[i][0] = 0;
        }
    }
}
```

The time complexity is still $O(mn)$, but the space complexity is now $O(1)$ since we are not using additional memory.