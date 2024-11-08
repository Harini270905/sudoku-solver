
import random

# Function to check if the grid is safe to place a number
def is_safe(grid, row, col, num):
    for x in range(9):
        if grid[row][x] == num or grid[x][col] == num:
            return False

    start_row, start_col = 3 * (row // 3), 3 * (col // 3)
    for i in range(3):
        for j in range(3):
            if grid[i + start_row][j + start_col] == num:
                return False

    return True

# Function to solve the Sudoku grid using backtracking
def solve_sudoku(grid):
    empty = find_empty_location(grid)
    if not empty:
        return True
    row, col = empty

    for num in range(1, 10):
        if is_safe(grid, row, col, num):
            grid[row][col] = num
            if solve_sudoku(grid):
                return True
            grid[row][col] = 0
                return False
    for i in range(9):
        for j in range(9):
            if grid[i][j] == 0:
                return (i, j)
    return None

# Function to create a fully solved Sudoku grid
def create_full_grid():
    grid = [[0] * 9 for _ in range(9)]
    
    def fill_grid(grid):
        numbers = list(range(1, 10))
        for i in range(9):
            for j in range(9):
                if grid[i][j] == 0:
                    random.shuffle(numbers)
                    for num in numbers:
                        if is_safe(grid, i, j, num):
                            grid[i][j] = num
                            if not find_empty_location(grid) or fill_grid(grid):
                                return True
                            grid[i][j] = 0
                    return False
        return True

    fill_grid(grid)
    return grid
    # Function to create a Sudoku puzzle by removing numbers from a complete grid
def create_puzzle(grid, num_holes=40):
    puzzle = [row[:] for row in grid]
    count = num_holes

    while count > 0:
        row = random.randint(0, 8)
        col = random.randint(0, 8)
        if puzzle[row][col] != 0:
            backup = puzzle[row][col]
            puzzle[row][col] = 0
            
            # Copy grid and check if it still has a unique solution
            grid_copy = [row[:] for row in puzzle]
            if not solve_sudoku(grid_copy):
                puzzle[row][col] = backup
                continue
            count -= 1

    return puzzle

# Function to print the Sudoku grid
def print_grid(grid):
    for row in grid:
        print(" ".join(str(num) if num != 0 else '.' for num in row))

# Generate and print a complete Sudoku grid
full_grid = create_full_grid()
print("Full Sudoku Grid:")
print_grid(full_grid)

# Create and print a Sudoku puzzle
puzzle_grid = create_puzzle(full_grid, num_holes=40)
print("\nSudoku Puzzle:")
print_grid(puzzle_grid)
