# Recursion-Code
/*
 * Name:
 * Date Submitted:
 * Lab Section:
 * Assignment Name:
 */

#include <iostream>
#include <fstream>
#include <vector>

using namespace std;

//Represents an occupied square in the grid
//Do not modify the GridSquare class or its member functions
class GridSquare
{
    private:
        int row, col;
    public:
        GridSquare() : row(0), col(0) {} //Default constructor, (0,0) square
        GridSquare(int r, int c) : row(r), col(c) {} //(r,c) square
        //Compare with == operator, used in test cases
        bool operator== (const GridSquare r) const
        {
            if ((row == r.row) && (col == r.col))
            {
                return true;
            }
            return false;
        }
        int getRow() { return row; } //return row value
        int getCol() { return col; } //return column value
        //Output using << operator, used in Grouping::printGroups()
        friend ostream& operator<< (ostream& os, const GridSquare obj);
};

//Function definition for <ostream> << <GridSquare>
ostream& operator<< (ostream& os, const GridSquare obj)
{
    os << "(" << obj.row << "," << obj.col << ")";
    return os;
}

/*
Groups squares in 10x10 grid upon construction
Additional private helper functions may be added
Need to implement the constructor that takes a file name
as well as findGroup
The findGroup function's parameter list may be changed based
on how the function is implemented
*/
class Grouping
{
    private:
        int grid[10][10];
        vector<vector<GridSquare>> groups;
    public:
        Grouping() : grid{},groups() {} //Default constructor, no groups
        Grouping(string fileName); //Implement this function
        void findGroup(int r, int c); //Implement this function (recursive)
        void printGroups() //Displays grid's groups of squares
        {
            for(int g=0; g<groups.size(); g++)
            {
                cout << "Group " << g+1 << ": ";
                for(int s=0; s<groups[g].size(); s++)
                {
                    cout << " " << groups[g][s];
                }
                cout << endl;
            }
        }
        vector<vector<GridSquare>> getGroups() //Needed in unit tests
        {
            return groups;
        }
};

//Implement the (parameterized) constructor and findGroup functions below
Grouping::Grouping(string fileName) {
    
    ifstream infile;
    infile.open(fileName);
    char check;
    
    while(infile >> check) {
        for(int r=0; r<10; r++) {
            for(int c=0; c<10; c++) {
                if(check == '.') {
                    grid[r][c] = 0; 
                }
                else {
                    grid[r][c] = 1;
                }
            }
        }
    }
    
    infile.close();
}

void Grouping::findGroup(int r, int c) {
    
    if(groups.empty()) {
        GridSquare point(r, c);
        groups.push_back(vector<GridSquare>());
        groups[0].push_back(point);
    }
    
    
    if(r >= 10 || c >= 10) {
        
    }
    else {
        if(grid[r][c] == 1) {
    
            GridSquare point(r, c);
            groups.push_back(vector<GridSquare>());
            groups[1].push_back(point);
            grid[r][c] = 0;
            
            findGroup(r+1, c);
            findGroup(r-1, c);
            findGroup(r, c+1);
            findGroup(r, c-1);
            
        }
        else {
            
        }
    }
}
            // else {
            //     for(i=0; i<groups.size(); i++) {
            //         for(j=0; j<groups[i].size(); j++) {
            //             if(grid[i][j] == 1) {
                            
            //             }
        //                 if(groups[i][j] == GridSquare(r+1, c) && 
        //                 grid[i][j] == 1) {
        //                     grid[i][j] = 0;
        //                     groups.push_back(vector<GridSquare>());
        //                     groups[i].push_back(point);
        //                     findGroup(r+1, c);
        //                 }
        //                 else if(groups[i][j] == GridSquare(r-1, c) &&
        //                 grid[i][j]==1) {
        //                     grid[i][j] = 0;
        //                     groups.push_back(vector<GridSquare>());
        //                     groups[i].push_back(point);
        //                     findGroup(r-1, c);
        //                 }
        //                 else if(groups[i][j] == GridSquare(r, c+1) && 
        //                 grid[i][j]==1) {
        //                     grid[i][j] = 0;
        //                     groups.push_back(vector<GridSquare>());
        //                     groups[i].push_back(point);
        //                     findGroup(r, c+1);
        //                 }
        //                 else if(groups[i][j] == GridSquare(r, c-1) && 
        //                 grid[i][j]==1) {
        //                     grid[i][j] = 0;
        //                     groups.push_back(vector<GridSquare>());
        //                     groups[i].push_back(point);
        //                     findGroup(r, c+1);
        //                 }
        //                 else {
        //                     while(!groups[i].empty()) {
        //                         groups[advance].push_back(groups[i].back());
        //                         groups[i].pop_back();
        //                     }
        //                     groups.erase(groups.begin()+i);
        //                 }
        //                 }
        //             }
        //         }
        //     }
        // }
//}




//Simple main function to test Grouping
int main()
{

bool correct = false;
int groupNum = 1;
int count = 0;
int groupCount = 6;
GridSquare groupTest[][4] = { {GridSquare(0,9),GridSquare(1,9)},
                              {GridSquare(1,3),GridSquare(1,4)},
                              {GridSquare(3,4)},
                              {GridSquare(3,7),GridSquare(4,7),
                               GridSquare(5,7),GridSquare(5,8)},
                              {GridSquare(4,3)},
                              {GridSquare(6,4),GridSquare(6,5)} };
int sizeTest[] = {2, 2, 1, 4, 1, 2};
bool groupTestPass[] = {false, false, false, false, false, false};
Grouping input("input1.txt");
vector<vector<GridSquare>> groups;
groups = input.getGroups();
for (int g=0; g<groupCount; g++)
{
  for (int i=0; i<groups.size(); i++)
  {
    for (int j=0; j<groups[i].size(); j++)
    {
      if (groupTest[g][0] == groups[i][j])
      {
        groupNum=i;
      }
    }
  }
  count = 0;
  for (int s=0; s<sizeTest[g]; s++)
  {
      cout << groupNum << endl;
    for (int i=0; i<groups[groupNum].size(); i++)
    {
      if (groupTest[g][s] == groups[groupNum][i])
      {
        count++;
      }
    }
    if (count == sizeTest[g])
    {
      groupTestPass[g] = true;
    }
  }
}
correct = groupTestPass[0];
for (int g=1; g<groupCount; g++)
{
  correct = correct && groupTestPass[g];
}

}
