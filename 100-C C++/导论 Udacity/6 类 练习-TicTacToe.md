

# 练习|Tic-Tac-Toe

4*4 game board

## 1 思路

### Data

- userName
- userLabel
- gameBoard



改进版：可以**将gameBoard也看做一个对象**

- 包含属性：fourInLine
- 已有的Mark数量



### Methods

- void printInfo();
- int fourInLine(char label)
  - row
  - column
  - diagonal

```c++
//参考 
int Gameboard::fourInRow()
{
    int count;
    for(int i=0;i<4; i++)
    {
        count = 0;
        for(int j=0;j<4; j++)
        {
            if(gameSpace[i][j]=='x')
            {
                count++;
                //cout<<"count = "<<count;
            }
        }
        if(count == 4)
            return 1;
    }
    return 0;
}
```



### int main(){}

```c++
//伪代码
count = 0
while(input())
{
    count++;
    if(check(fourInLine))
    {
        return win;
        break;
    }
    
    if(count is full)
    {
        return tie;
        break;
    }
}
```



## 2 文件组织

| 文件组织          |      |
| ----------------- | ---- |
| main.cpp          |      |
| mainClasses.cpp   |      |
| mainFunctions.cpp |      |
| input.txt         |      |



```c++
//input.txt
Catherine
John
0
4
1
12
2
13
3
14
```



## 3 最终程序

### mainClasses.cpp

```c++
//注意：最好用std::string(不用到处添加using namespace std;)
#include<string>
#include <iomanip>      // std::setw

class GameUser
{
    std::string userName;   //privat：需要通过get_函数来访问
    char userLabel;
    char GameBoard[4][4]; //声明二维空数组
    int markNum; //没什么用
        
    
public:
    GameUser(std::string userNameIn, char userLabelIn); //构造器声明
    void setMark(int posIn);
    
    std::string getUserName();
    
    int fourInRowCol();
    int fourInDiag();
    int fourInLine();
    void printInfo();
};

GameUser::GameUser(std::string userNameIn, char userLabelIn)
{
    userName = userNameIn;
    userLabel = userLabelIn;
    markNum = 0;
    for(int i=0; i<4; i++)
        for(int j=0; j<4; j++)
        {
            GameBoard[i][j] = '-';   //先尝试默认字符(空格显示 不明显)
        }
}

//set
void GameUser::setMark(int posIn)
{
    int row = posIn / 4;
    int col = posIn % 4;
    GameBoard[row][col] = userLabel;
    markNum++;
    
}

//get
std::string GameUser::getUserName()
{
    return userName;
}


int GameUser::fourInRowCol()
{
    char mark = userLabel;
    int countRow;
    int countCol;
    for(int i=0;i<4; i++)
    {
        int countRow = 0;
        int countCol = 0;
        for(int j=0;j<4; j++)
        {
            if(GameBoard[i][j]==mark)
            {
                countRow++;
                //cout<<"count = "<<count;
            }
            else if(GameBoard[j][i]==mark)
            {
                countCol++;
                //cout<<"count = "<<count;
            }
        }
        if(countRow == 4 || countCol==4)
            return 1;
    }
    return 0; 
}


int GameUser::fourInDiag()
{
    char mark = userLabel;
    int countLeftDiag = 0;
    int countRightDiag = 0;
    for(int i=0; i<4;i++)
    {
        if(GameBoard[i][i]==mark)
        {
            countLeftDiag++;
        }
        else if(GameBoard[i][3-i]==mark)
        {
            countRightDiag++;
        }
        if(countLeftDiag == 4 || countRightDiag==4)
            return 1;
    }
    return 0;
}


int GameUser::fourInLine()
{
    if(this->fourInRowCol()==1 || this->fourInDiag()==1)    //类似python中的self
    {
        return 1;
    }
    return 0;
}


void GameUser::printInfo()
{
    for(int i=0; i<4;i++)
    {	//注意：如果没有这层花括号，最后输出std::endl就不属于for循环之内  for(int i)只包含了fot(int j)
        for(int j=0; j<4; j++)
        {
            std::cout << GameBoard[i][j] << std::setw(5);
        }
        std::cout<<std::endl;
    }
}
```



### main.cpp

```c++
#include <iostream>
#include "mainClasses.cpp"

using namespace std;

int main()
{
    const int SIZE=16;
    string userNameA, userNameB;
    int pos;
    int count=0;
    char flag='n';
    
    //读取txt文件中的名字
    cin>>userNameA;
    cin.ignore(1024, '\n');
    
    cin>>userNameB;
    cin.ignore(1024, '\n');
    
    GameUser userA(userNameA, 'x');
    GameUser userB(userNameB, 'o');
    
    //注意do-while结构
    do{
        //读取txt文件中的数据
        cin>>pos;
        cin.ignore(1024, '\n');
        userA.setMark(pos);
        count++;
        
        cout<<count<<'\n';
        if(userA.fourInLine()==1)
        {
            cout<<"winner is:"<<userA.getUserName();
            flag = 'y';
            break;
        }
        
        if(count==SIZE)
        {
            cout<<"tie";
            break;
        }
        
        cin>>pos;
        cin.ignore(1024, '\n');
        userB.setMark(pos);
        count++;
        
        if(userB.fourInLine()==1)
        {
            cout<<"winner is:"<<userB.getUserName();
            flag = 'y';
            break;
        }
    }while(count<SIZE);
    
    if(flag=='n')
    {
        cout<<"tie";
    }
    userA.printInfo();
    //userB.printInfo();
    
    return 0;
}
```





# 参考答案

## main.cpp

```c++
/*Goal: Practice creating classes and functions. 
**Create a program that allows two users to 
**play a 4x4 tic-tac-toe game. 
*/

#include "TicTacToeClasses.cpp"
#include "TicTacToeFunctions.cpp"


int main()
{
    Board gameBoard;
    string nameX;
    string nameO;
    
    int tieGame = 0;
    char gameWinner = 'z';	//默认获胜者为'z'(平局)
    int numTurns = 0;
    
    //get the names of the players
    getUserNames(nameX, nameO);
    
    //the game is played for 8 turns maximum
    while(numTurns < 8)
    {
        //print a board that has the postions numbered
        printTheBoard(gameBoard);
        //ask player x where they want to put an 'x'
        printUserPrompt(nameX, 'x');
        //check that the input is a valid position and that
        //it has not already been taken
        checkResponse(gameBoard, 'x');	//注意检测：有效地输入
        //check to see if player 'x' has four in a row somewhere on the board
        gameWinner = gameBoard.determineWinner();	//每一次输入后检测：是否成功
        
        //if player 'x' has won, end the game
        if(gameWinner != 'z')
        {
            printTheBoard(gameBoard);
            writeTheBoard(gameBoard);
            printGameWinner(gameBoard, nameX, nameO);
            break;
        }
        
        //Now do the same for player 'o'
        //print a board that has the postions numbered
        printTheBoard(gameBoard);
        writeTheBoard(gameBoard);
        //ask player x where they want to put an 'o'
        printUserPrompt(nameO, 'o');
        //check that the input is a valid position and that
        //it has not already been taken
        checkResponse(gameBoard, 'o');

        printTheBoard(gameBoard);
        writeTheBoard(gameBoard);
        //check to see if player 'o' has four in a row somewhere on the board
        gameWinner = gameBoard.determineWinner();
        //if player 'o' has won, end the game
        if(gameWinner != 'z')
        {
            printTheBoard(gameBoard);
            writeTheBoard(gameBoard);
            printGameWinner(gameBoard, nameX, nameO);
            break;
        }
        numTurns++;	//限定回合数：可估计
    }
    //if there is no winner at this point, the game is a tie
    if(numTurns >= 8)
        cout<<"Tie game.\n\n";
    return 0;
}
```



## TicTacToeClasses.cpp

实现类的另一种方法：

- 方法声明的同时完成定义(在类的内部) => 所以无法清晰地看到类中methods的列表



me|感受：

- 参考答案是一个类的对象：分两种情况(两位选手)考虑
- 而我是两个类的实例对象：也就是两位选手 => 他们的操作都是相同的，所以调用类的data、methods也是一致的





亮点：

- 注意类定义末尾的分号
- `char* getPositions(void)`	//返回字符数组(用指针表示)

```c++
/*The Class header file for main.cpp*/
#include <stdio.h>
#include <iostream>
#include <cstring>
#include <string>
#include <fstream>

using namespace std;

class Board
{//the class tracks the game and the winner
    char positionsSelected[16];
    char winner;
    
public:
    /*This is another way of doing methods in classes. 
    Instead of listing the methods in the class, 
    Then defining them afterwards, the methods are 
    defined within the class definition. 
    The downside of this method is: it is not immediately
    clear what methods are in the class. 
    */
    Board()
    {//sets the board to blanks and the winner to 'z'
        //std::cout<<"Creating a board\n";
        winner = 'z'; //not determined or tie
        
        for(int i = 0; i < 16; i++)
        {
            positionsSelected[i] = '_';
        }
    }
    
    
    char* getPositions(void)	//返回字符数组(用指针表示)
    {//return all the positions on the board
        return positionsSelected;
    }
    
    
    int setPosition(int gridNumber, char user)
    {//set a given position to x or o
        {
            if(positionsSelected[gridNumber] == '_')
            {
                positionsSelected[gridNumber] = user;
                return 0;
            }
            else
            {
                //cout<<"That position is taken\n";
                return -1;
            }
        }
        return 0;
    }
    
    char checkRows()
    {//check the rows for a winner
        int rows = 0;
        int columns = 0;
        int fourInRowX = 0;
        int fourInRowO = 0;
        
        //Check rows for a winner
        for(int rows = 0; rows<16; rows=rows+4)
        {
            for(int i = 0; i < 4; i++)
            {
                if(positionsSelected[i + rows] == 'x')
                    fourInRowX++;
                if(positionsSelected[i + rows] == 'o')
                    fourInRowO++;
            }
            if(fourInRowX == 4)
            {
                //cout<<"ROWS X won\n";
                winner = 'x';
                return 'x';
            }
            if(fourInRowO == 4)
            {
                //cout<<"ROWS O won\n";
                winner = 'o';
                return 'o';
            }
            fourInRowX = 0;
            fourInRowO = 0;
        }
        return 'z';
    }
    
    char checkColumns()
    {//check the columns for a winner
        int rows = 0;
        int columns = 0;
        int fourInRowX = 0;
        int fourInRowO = 0;
        
        //Check columns for a winner
        for(int columns = 0; columns<4; columns++)
        {
            for(int i = 0; i < 16; i= i + 4)
            {
                if(positionsSelected[i + columns] == 'x')
                    fourInRowX++;
                if(positionsSelected[i + columns] == 'o')
                    fourInRowO++;
            }
            if(fourInRowX == 4)
            {
                //cout<<"COLUMNS X won\n";
                winner = 'x';
                return 'x';
            }
            if(fourInRowO == 4)
            {
                //cout<<"COLUMNs O won\n";
                winner = 'o';
                return 'o';
            }
            fourInRowX = 0;
            fourInRowO = 0;
        }
        return 'z';
    }
    
    char checkDiagonals()
    {//check the diagonals for a winner
        char winner = 'z';
        int fourInRowX = 0;
        int fourInRowO = 0;
        
        //check left to right diagonal
        for(int i = 0; i < 16; i=i+5)
        {
            if(positionsSelected[i] == 'x')
                fourInRowX++;
            if(positionsSelected[i] == 'o')
                fourInRowO++;
        }
        
        //check right to left diagonal
        if(fourInRowO != 4 and fourInRowX != 4)
        {
            fourInRowX = 0;
            fourInRowO = 0;
            for(int i = 3; i < 15; i= i+3)
            {
                if(positionsSelected[i] == 'x')
                    fourInRowX++;
                if(positionsSelected[i] == 'o')
                    fourInRowO++;
            }
        }

        if(fourInRowX == 4)
        {
            //cout<<"DIAGONALS X won\n";
            winner = 'x';
            return winner;
        }
        if(fourInRowO == 4)
        {
            //cout<<"DIAGONALS O won\n";
            winner = 'o';
            return winner;
        }
        fourInRowX = 0;
        fourInRowO = 0;
        return winner;
    }

    char determineWinner()
    {//if 4 in a row, then there is a winner
        char winner = 'z';
        winner =  checkRows();
        if(winner == 'z')
            winner = checkColumns();
        if(winner =='z')
            winner = checkDiagonals();
        return winner;
    }  
};	//注意类的定义之后的分号
```





## TicTacToeFunctions.cpp

思路：类尽量简化

- 只跟类的数据有关的操作，传递类对象Board





@亮点：

- 声明的时候不用形参，只需要数据类型即可
- 传递变量引用/指针  `void getUserNames(string &, string &);`
- 传递类对象的方式
- text输入数据：
  - 只需要简单的cin即可
- +==检查用户输入的合法性==

```c++
/*Goal: Practice creating classes and functions. 
**Create a program that allows two users to 
**play a 4x4 tic-tac-toe game. 
*/

#include <fstream>

void getUserNames(string &, string &);
void printBlankBoard(string);
void printTheBoard(Board, string);
void printUserPrompt(string, char);
void printGameWinner(Board, string, string);
int  getUserResponse();
void checkResponse(Board&, char);
void writeTheBoard(Board);

using namespace std;

void checkResponse(Board &boardIn, char input)
{//check that the position selected is not already
    //selected
    int position;
    int userInput;
    do
    {
        position = getUserResponse();
        userInput = boardIn.setPosition(position, input);
        if(userInput == -1)
        {
            cout<<"That postition is taken.";
        }
    }while(userInput == -1);
}

void getUserNames(string &userX, string &userY)
{//get the user names
    std::cout<< "Name of user to be 'x' :";
    std::cin >>userX;
    std::cout<< "Name of user to be 'o' :";
    std::cin >>userY;
}

void printBlankBoard()
{//print a blank board, with numbered squares
    for(int i = 0; i <16; i++)
    {
        std::cout<<"|"<<i<<":";
        if(i <= 9)
            cout<<" ";
        if(i%4 == 3)
        {
            std::cout<<"|\n";
        }
    }
    cout<<"\n\n\n";
}

void printTheBoard(Board boardIn)
{//print the board with player moves
    printBlankBoard();

    for(int i = 0; i <16; i++)
    {
        std::cout<<"|"<<boardIn.getPositions()[i];
        if(i%4 == 3)
        {
            std::cout<<"|\n";
        }
    }
    cout<<"\n\n\n";
}

void writeTheBoard(Board boardIn)
{//print the board with player moves
    cout<<"\n\n";
    for(int i = 0; i <16; i++)
    {
        cout<<"|"<<boardIn.getPositions()[i];
        if(i%4 == 3)
        {
            cout<<"|\n";
        }
    }
    cout<<"\n\n\n";
}

void printUserPrompt(string nameIn, char letter)
{//prompt for user input
    std::cout<<nameIn<<" where would you like to place an "<<letter<<"?: ";
    cout<<"\n\n";
    cout<<" where would you like to place an "<<letter<<"?: ";
}

void printGameWinner(Board boardIn, string nameX, string nameO)
{//print the winner statement
    char winner;
    winner = boardIn.determineWinner();
    if(winner == 'x')
    {
        cout<<"Congrats "<<nameX<<" you won!\n\n";
    }
    if(winner == 'o')
    {
        cout<<"Congrats "<<nameO<<" you won!\n\n";
    }
}

int getUserResponse()
{//get the chosen user position, check that it is an integer
    //check that it is between 0 and 15 inclusive
    int position = -1;
    cout<<"Enter an integer between 0 and 15: ";
    std::cin>>position;
    
    while(position > 15 or position < 0 or !cin)
    {
        cin.clear();
        //cin.ignore(numeric_limits<streamsize>::max(), '\n');
        std::cout<<"That is not a valid position\n";
        cout<<"Enter an integer between 0 and 15: ";
        cin.clear();
        cin>>position;
    }
    return position;
}
```

