# Stone-Paper-Scissor-Game
A console-based Stone Paper Scissors game built with C++.
// Project 1 Stone - Paper - Scessor
#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>
using namespace std;
enum enChoice
{
    Stone = 1,
    Paper = 2,
    Scessor = 3
};
enum enWinner
{
    Player = 1,
    Computer = 2,
    Draw = 3
};
struct strRoundInfo
{
    short RoundNum;
    enChoice PlayerChoice;
    enChoice ComputerChoice;
    enWinner RoundWinner;
    string WinnerName;
};
struct strGameResult
{
    short Rounds;
    short PlayerWinTimes;
    short ComputerWinTimes;
    short DrawTimes;
    enWinner GameWinner;
    string WinnerName;
};
int Random(int From, int To)
{
    int Num = rand() % (To - From + 1) + From;
    return Num;
}
short HowManyRounds()
{
    int Round;
    do
    {
        cout << "Please Enter How Many Rounds Between 1 And 10 , You Want To Play." << endl;
        cin >> Round;
    } while (Round < 1 || Round >10);
    return Round;
}
enChoice PlayerChoice()
{
    short Choice;
   do
   {
        cout << "Your Choice : [1]Stone , [2]Paper , [3]Scessor ??" << endl;
        cin >> Choice;
   } while (Choice < 1 || Choice >3);
   return (enChoice)Choice;
}
enChoice ComputerChoice()
{
    return (enChoice)Random(1, 3);
}
enWinner RoundWinner(enChoice PlayerChoice , enChoice ComputerChoice)
{
    if (PlayerChoice == ComputerChoice)
    {
        return enWinner::Draw;
    }
    switch (PlayerChoice)
    {
    case enChoice::Stone:
        if (ComputerChoice == enChoice::Paper)
        {
            return enWinner::Computer;
        }
        break;
    case enChoice::Scessor:
        if (ComputerChoice == enChoice::Stone)
        {
            return enWinner::Computer;
        }
        break;
    case enChoice ::Paper:
        if (ComputerChoice == enChoice::Scessor)
        {
            return enWinner::Computer;
        }
        break;
    }
    return enWinner::Player;
}
string WinnerName(enWinner RoundWinner)
{
    string WinnerName[3] = { "Player","Computer","Draw" };
    return WinnerName[RoundWinner - 1];
}
string ChoiceName(enChoice Choice)
{
    string ChoiceName[3] = { "Stone","Paper","Scessor" };
    return  ChoiceName[Choice - 1];
}
void ScreenColor(strRoundInfo Winner)
{
    if (Winner.RoundWinner == enWinner::Player)
        system("color 2F");
    else if (Winner.RoundWinner == enWinner::Computer)
        system("color 4F");
    else
        system("color E0");
}
void PrintRoundResult(short RoundNum, strRoundInfo RoundInfo)
{
    cout << endl << endl;
    cout << "___________________Round[" << RoundNum << "]___________________" << endl;
    cout << "Player Choice   : " << ChoiceName(RoundInfo.PlayerChoice) << endl;
    cout << "Computer Choice : " << ChoiceName(RoundInfo.ComputerChoice) << endl;
    cout << "Round Winner    : " << RoundInfo.WinnerName << endl;
    cout << "____________________________________________________" << endl;
    ScreenColor(RoundInfo);
}
enWinner GameWinnerII(short PlayerWinTimes, short ComputerWinTimes)
{
    if (PlayerWinTimes > ComputerWinTimes)
        return enWinner::Player;
    else if (ComputerWinTimes > PlayerWinTimes)
        return enWinner::Computer;
    else
        return enWinner::Draw;
}
strGameResult FillGameResult(short TotalRounds,short PlayerWinTimes,short ComputerWinTimes,short DrawTimes)
{
    strGameResult GameResult;
    GameResult.Rounds = TotalRounds;
    GameResult.PlayerWinTimes = PlayerWinTimes;
    GameResult.ComputerWinTimes = ComputerWinTimes;
    GameResult.DrawTimes = DrawTimes;
    GameResult.GameWinner = GameWinnerII(GameResult.PlayerWinTimes, GameResult.ComputerWinTimes);
    GameResult.WinnerName = WinnerName(GameResult.GameWinner);
    return GameResult;
}
strGameResult RoundInfo(short Round)
{
    strRoundInfo RoundInfo;
    short PlayerWinTimes = 0, ComputerWinTimes = 0, DrawTimes = 0;

    for (short RoundCount = 1; RoundCount <= Round;RoundCount++)
    {
        cout << "Round[" << RoundCount << "] Begins" << endl;
        RoundInfo.RoundNum = RoundCount;
        RoundInfo.PlayerChoice = PlayerChoice();
        RoundInfo.ComputerChoice = ComputerChoice();
        RoundInfo.RoundWinner = RoundWinner(RoundInfo.PlayerChoice,RoundInfo.ComputerChoice);
        RoundInfo.WinnerName = WinnerName(RoundInfo.RoundWinner);
        if (RoundInfo.RoundWinner == enWinner::Player)
            PlayerWinTimes++;
        else if (RoundInfo.RoundWinner == enWinner::Computer)
            ComputerWinTimes++;
        else
            DrawTimes++;
        PrintRoundResult(RoundCount, RoundInfo);
    }
    return FillGameResult(Round, PlayerWinTimes, ComputerWinTimes, DrawTimes);
}
string Tabs(short Tab)
{
    string T = "";
    for (short t = 1;t <= Tab;t++)
    {
        T = T + "\t";
    }
    return T;
}
void PrintGameResult(strGameResult GameResult)
{
    cout << Tabs(2) << "______________________[Game Result]____________________" << endl;
    cout << Tabs(2) << "Game Rounds        : " << GameResult.Rounds << endl;
    cout << Tabs(2) << "Player Won Times   : " << GameResult.PlayerWinTimes << endl;
    cout << Tabs(2) << "Computer Won Times : " << GameResult.ComputerWinTimes << endl;
    cout << Tabs(2) << "Draw Times         : " << GameResult.DrawTimes << endl;
    cout << Tabs(2) << "Final Winner       : " << GameResult.WinnerName << endl;
    cout << Tabs(2) << "______________________________________________________" << endl;
}
void PrintGameOverScreen()
{
    cout << Tabs(2) << "______________________________________________________" << endl;
    cout << Tabs(2) << "                       +++Game Over+++                " << endl;
    cout << Tabs(2) << "______________________________________________________" << endl;
}
void ResetTheScreen()
{
    system("cls");
    system("color 0F");
}
void StartGame()
{
    string PlayAgain = "Y";
    do
    {
        ResetTheScreen();
        strGameResult Game = RoundInfo(HowManyRounds());
        PrintGameOverScreen();
        PrintGameResult(Game);
        cout << Tabs(2)<<"Do You Want To Play Again ??" << endl;
        cin >> PlayAgain;
    } while (PlayAgain == "Y" || PlayAgain == "y");
}
int main()
{
    srand((unsigned)time(NULL));
    StartGame();
    return 0;
}
