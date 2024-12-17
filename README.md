#include <stdio.h>
#include <stdbool.h>

#define N 9 


void printGrid(int grid[N][N]);
bool isSafe(int grid[N][N], int row, int col, int num);
bool solveSudoku(int grid[N][N]);
bool findUnassignedLocation(int grid[N][N], int *row, int *col);
bool usedInRow(int grid[N][N], int row, int num);
bool usedInCol(int grid[N][N], int col, int num);
bool usedInBox(int grid[N][N], int boxStartRow, int boxStartCol, int num);


void printGrid(int grid[N][N]) {
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++) {
            if (col % 3 == 0 && col != 0) {
                printf("| ");
            }
            printf("%d ", grid[row][col]);
        }
        printf("\n");
        if ((row + 1) % 3 == 0 && row != N - 1) {
            printf("---------------------\n");
        }
    }
}


bool isSafe(int grid[N][N], int row, int col, int num) {
    return !usedInRow(grid, row, num) &&
           !usedInCol(grid, col, num) &&
           !usedInBox(grid, row - row % 3, col - col % 3, num);
}


bool usedInRow(int grid[N][N], int row, int num) {
    for (int col = 0; col < N; col++) {
        if (grid[row][col] == num) {
            return true;
        }
    }
    return false;
}


bool usedInCol(int grid[N][N], int col, int num) {
    for (int row = 0; row < N; row++) {
        if (grid[row][col] == num) {
            return true;
        }
    }
    return false;
}


bool usedInBox(int grid[N][N], int boxStartRow, int boxStartCol, int num) {
    for (int row = 0; row < 3; row++) {
        for (int col = 0; col < 3; col++) {
            if (grid[row + boxStartRow][col + boxStartCol] == num) {
                return true;
            }
        }
    }
    return false;
}


bool findUnassignedLocation(int grid[N][N], int *row, int *col) {
    for (*row = 0; *row < N; (*row)++) {
        for (*col = 0; *col < N; (*col)++) {
            if (grid[*row][*col] == 0) {
                return true;
            }
        }
    }
    return false;
}


bool solveSudoku(int grid[N][N]) {
    int row, col;

    
    if (!findUnassignedLocation(grid, &row, &col)) {
        return true;
    }

    
    for (int num = 1; num <= 9; num++) {
    
        if (isSafe(grid, row, col, num)) {
            grid[row][col] = num;

            
            if (solveSudoku(grid)) {
                return true;
            }

            
            grid[row][col] = 0;
        }
    }
    return false; 
}


int main() {
    int grid[N][N] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    printf("Original Sudoku Grid:\n");
    printGrid(grid);

    if (solveSudoku(grid)) {
        printf("\nSolved Sudoku Grid:\n");
        printGrid(grid);
    } else {
        printf("\nNo solution exists\n");
    }

    return 0;
}




