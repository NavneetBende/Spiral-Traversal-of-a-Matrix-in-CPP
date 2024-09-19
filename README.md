Spiral Traversal of a Matrix in C++
Here, in this page we will discuss the program to print the spiral traversal of the matrix in C++ programming language. We are given with the elements of the array in two-dimensional form and we need to traverse the entire matrix in spiral form and print the corresponding element.

Spiral Traversal of a Matrix in C++
While loop in C
Method discussed :
Method 1 : Using Brute force.
Method 2 : Using DFS
Method 1 :
Declare a 2-d array of size rXc.
Now, create variable top and initialize it with 0 denotes starting of row, and variable bottom initialized it with (r-1) denotes the end of row, another variable say left set it to 0, it indicate the starting of the column and last variable is right that will be declared with value c-1 which indicate the end of the column.
Run a loop until all the squares of loops are printed.
Print the top row, i.e. Print the elements of the top-th row from column index left to right, and increase the count of top.
Print the right column, i.e. Print the last column or right-th column from row index top to bottom and decrease the count of right.
Print the bottom row, i.e. if top < bottom, then print the elements of bottom-th row from column right to left and decrease the count of bottom
Print the left column, i.e. if left < right, then print the elements of left-th column from bottom-th row to top-th row and increase the count of left.
Time and Space Complexities :
Time-Complexity :O(r*c)
Space-Complexity : O(1)
Spiral Traversal
Run
#include <bits/stdc++.h>
#define r 4
#define c 4
using namespace std;

int main()
{   
    int a[4][4] = { { 1, 2, 3, 4 },

                    { 5, 6, 7, 8 },

                    { 9, 10, 11, 12 },

                    { 13, 14, 15, 16 } };

    int i, left = 0, right = c-1, top = 0, bottom = r-1;

    while (left <= right && top <= bottom) {

        /* Print the first row
        from the remaining rows */
        for (i = left; i <= right; ++i) {
            cout<<a[top][i]<<" ";
        }
        top++;

        /* Print the last column
        from the remaining columns */
        for (i = top; i <= bottom; ++i) {
         cout<<a[i][right]<<" ";
        }
        right--;
    
        /* Print the last row from
        the remaining rows */
        if (top <= bottom) { for (i = right; i >= left; --i) {
            cout<<a[bottom][i]<<" ";
          }
          bottom--;
        }
    
        /* Print the first column from
        the remaining columns */
        if (left <= right) { for (i = bottom; i >= top; --i) {
            cout<<a[i][left]<<" ";
        }
        left++;
        }
    }
    
    return 0;
}
Output
1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10
Method 2  :
Create a DFS function which takes matrix, cell indices and direction.
Now, we will check the cell indices pointing to a valid cell (that is, not visited and in bounds),if not, skip this cell
Print the value of the cell.
And Mark matrix cell pointed by indicates as visited by changing it to a value not supported in the matrix
Now, check the surrounding cells valid. If not stop algorithm, else continue.
If direction given is right then check, is the cell to the right valid. If so, DFS to the right cell given the steps above, else change the direction to down and DFS downwards given the steps above.
Else, if the direction given is down then check, is the cell to the down valid. If so, DFS to the cell below given the steps above else, change the direction to left and DFS leftwards given the steps above.
Else, if the direction given is left then check, is the cell to the left valid. If so, DFS to the left cell given the steps above,      else change the direction to up and DFS upwards given the steps above.
Else, if the direction given is up then check, is the cell to the up valid. If so, DFS to the upper cell given the steps above,        else, change the direction to right and DFS rightwards given the steps above.
Time and Space Complexities :
Time-Complexity :O(r*c)
Space-Complexity : O(1)
Run
#include <bits/stdc++.h>
using namespace std;

#define R 4
#define C 4

bool isInBounds(int i, int j){
    if (i < 0 || i >= R || j < 0 || j >= C)
        return false;
    return true;
}

// check if the position is blocked
bool isBlocked(int matrix[R][C], int i, int j){
   if (!isInBounds(i, j))
        return true;
   if (matrix[i][j] == -1)
        return true;
   return false;
}

// DFS code to traverse spirally
void spirallyDFSTravserse(int matrix[R][C], int i, int j, int dir, vector<int>& res)
{
    if (isBlocked(matrix, i, j))
        return;
    bool allBlocked = true;
    for (int k = -1; k <= 1; k += 2) {
        allBlocked = allBlocked && isBlocked(matrix, k + i, j) && isBlocked(matrix, i, j + k);
    }

    res.push_back(matrix[i][j]);
    matrix[i][j] = -1;
    if (allBlocked) {
        return;
    }

    // dir: 0 - right, 1 - down, 2 - left, 3 - up
    int nxt_i = i;
    int nxt_j = j;
    int nxt_dir = dir;

    if (dir == 0) {
        if (!isBlocked(matrix, i, j + 1)) {
            nxt_j++;
        }
        else {
            nxt_dir = 1;
            nxt_i++;
        }
    }
    else if (dir == 1) {
        if (!isBlocked(matrix, i + 1, j)) {
            nxt_i++;
        }
        else {
            nxt_dir = 2;
            nxt_j--;
        }
    }
    else if (dir == 2) {
        if (!isBlocked(matrix, i, j - 1)) {
            nxt_j--;
        }
        else {
            nxt_dir = 3;
            nxt_i--;
        }
    }
    else if (dir == 3) {
        if (!isBlocked(matrix, i - 1, j)) {
            nxt_i--;
        }
        else {
            nxt_dir = 0;
            nxt_j++;
        }
    }
    spirallyDFSTravserse(matrix, nxt_i, nxt_j, nxt_dir, res);
}

// to traverse spirally
vector<int> spirallyTraverse(int matrix[R][C])
{
    vector<int> res;
    spirallyDFSTravserse(matrix, 0, 0, 0, res);
    return res;
}

// Driver Code
int main(){
    int a[R][C] = { { 1, 2, 3, 4 },

                    { 5, 6, 7, 8 },

                    { 9, 10, 11, 12 },

                    { 13, 14, 15, 16 } };

    // Function Call
    vector<int> res = spirallyTraverse(a);
    int size = res.size();
    for (int i = 0; i < size; ++i)
        cout << res[i] << " ";
    cout << endl;
    return 0;
}
Output
1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10
