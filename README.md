float calculateProfitPercent(float entryPrice,
                             float currentPrice,
                             int leverage,
                             string tradeType)
{
    float percentMove;

    if(tradeType == "Long")
    {
        percentMove =
        ((currentPrice - entryPrice)
        /
        entryPrice) * 100;
    }
    else
    {
        percentMove =
        ((entryPrice - currentPrice)
        /
        entryPrice) * 100;
    }

    return percentMove * leverage;
}

float calculatePnL(float quantity,
                   float entryPrice,
                   float currentPrice,
                   string tradeType)
{
    if(tradeType == "Long")
    {
        return
        (currentPrice - entryPrice)
        * quantity;
    }
    else
    {
        return
        (entryPrice - currentPrice)
        * quantity;
    }
}

float liquidationPrice(float entryPrice,
                       float quantity,
                       float walletBalance,
                       int leverage,
                       string leverageType)
{
    float liqPrice;

    if(leverageType == "Cross")
    {
        liqPrice =
        entryPrice -
        (walletBalance / quantity);
    }
    else
    {
        liqPrice =
        entryPrice -
        (entryPrice / leverage);
    }

    if(liqPrice < 0)
    {
        liqPrice = 0;
    }

    return liqPrice;
}
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

struct Crypto
{
    string name;
    float walletBalance;
    float margin;
    float quantity;
    float entryPrice;
    float currentPrice;
    int leverage;
    string leverageType;
    string tradeType;
};

float calculatePnL(float quantity,
                   float entryPrice,
                   float currentPrice,
                   string tradeType)
{
    if(tradeType == "Long")
    {
        return (currentPrice - entryPrice)
               * quantity;
    }
    else
    {
        return (entryPrice - currentPrice)
               * quantity;
    }
}

float calculatePnLPercent(float pnl,
                          float margin)
{
    return (pnl / margin) * 100;
}

void saveToFile(Crypto coin,
                float pnl,
                float pnlPercent)
{
    ofstream file("portfolio.txt",
                  ios::app);

    file << "Coin: " << coin.name << endl;
    file << "Wallet Balance: $" << coin.walletBalance << endl;
    file << "Margin Used: $" << coin.margin << endl;
    file << "Entry Price: $" << coin.entryPrice << endl;
    file << "Current Price: $" << coin.currentPrice << endl;
    file << "Leverage: " << coin.leverage << "x" << endl;
    file << "Leverage Type: " << coin.leverageType << endl;
    file << "Trade Type: " << coin.tradeType << endl;
    file << "Position Size: " << coin.quantity << endl;
    file << "PnL: $" << pnl << endl;
    file << "PnL Percentage: " << pnlPercent << "%" << endl;
    file << "----------------------" << endl;

    file.close();
}

void viewFile()
{
    ifstream file("portfolio.txt");

    if(!file)
    {
        cout << "Error Opening File!"
             << endl;
        return;
    }

    string line;

    cout << endl;
    cout << "FILE CONTENTS"
         << endl;

    while(getline(file, line))
    {
        cout << line
             << endl;
    }

    file.close();
}

void addCoin(Crypto coins[],
             int &count)
{
    if(count >= 100)
    {
        cout << "Portfolio Full!"
             << endl;
        return;
    }

    Crypto coin;

    cout << "Coin Name: ";
    cin >> coin.name;

    cout << "Wallet Balance: ";
    cin >> coin.walletBalance;

    cout << "Margin Used: ";
    cin >> coin.margin;

    cout << "Entry Price: ";
    cin >> coin.entryPrice;

    cout << "Current Price: ";
    cin >> coin.currentPrice;

    cout << "Leverage (1-125): ";
    cin >> coin.leverage;

    cout << "Leverage Type (Cross/Isolated): ";
    cin >> coin.leverageType;

    cout << "Trade Type (Long/Short): ";
    cin >> coin.tradeType;

    if(coin.leverage < 1 ||
       coin.leverage > 125)
    {
        cout << "ERROR: Invalid Leverage!"
             << endl;
        return;
    }

    if(coin.margin >
       coin.walletBalance)
    {
        cout << "ERROR: Margin exceeds Wallet Balance!"
             << endl;
        return;
    }

    if(coin.entryPrice <= 0)
    {
        cout << "ERROR: Invalid Entry Price!"
             << endl;
        return;
    }

    if(coin.tradeType != "Long" &&
       coin.tradeType != "Short")
    {
        cout << "ERROR: Invalid Trade Type!"
             << endl;
        return;
    }

    coin.quantity =
    (coin.margin *
     coin.leverage)
     /
     coin.entryPrice;

    if(coin.quantity <= 0)
    {
        cout << "ERROR: Invalid Position Size!"
             << endl;
        return;
    }

    float pnl =
    calculatePnL(
    coin.quantity,
    coin.entryPrice,
    coin.currentPrice,
    coin.tradeType);

    float pnlPercent =
    calculatePnLPercent(
    pnl,
    coin.margin);

    coins[count] = coin;

    count++;

    saveToFile(
    coin,
    pnl,
    pnlPercent);

    cout << "Coin Added Successfully!"
         << endl;
}

void viewPortfolio(Crypto coins[],
                   int count)
{
    if(count == 0)
    {
        cout << "Portfolio Empty!"
             << endl;
        return;
    }

    for(int i = 0;
        i < count;
        i++)
    {
        float pnl =
        calculatePnL(
        coins[i].quantity,
        coins[i].entryPrice,
        coins[i].currentPrice,
        coins[i].tradeType);

        float pnlPercent =
        calculatePnLPercent(
        pnl,
        coins[i].margin);

        cout << endl;

        cout << "Coin Name: "
             << coins[i].name
             << endl;

        cout << "Wallet Balance: $"
             << coins[i].walletBalance
             << endl;

        cout << "Margin Used: $"
             << coins[i].margin
             << endl;

        cout << "Position Size: "
             << coins[i].quantity
             << endl;

        cout << "Entry Price: $"
             << coins[i].entryPrice
             << endl;

        cout << "Current Price: $"
             << coins[i].currentPrice
             << endl;

        cout << "Leverage: "
             << coins[i].leverage
             << "x"
             << endl;

        cout << "Leverage Type: "
             << coins[i].leverageType
             << endl;

        cout << "Trade Type: "
             << coins[i].tradeType
             << endl;

        cout << "PnL: $"
             << pnl
             << endl;

        cout << "PnL Percentage: "
             << pnlPercent
             << "%"
             << endl;
    }
}

void searchCoin(Crypto coins[],
                int count)
{
    string name;

    cout << "Enter Coin Name: ";

    cin >> name;

    for(int i = 0;
        i < count;
        i++)
    {
        if(coins[i].name == name)
        {
            cout << endl;

            cout << "Coin Found!"
                 << endl;

            cout << "Name: "
                 << coins[i].name
                 << endl;

            cout << "Position Size: "
                 << coins[i].quantity
                 << endl;

            cout << "Trade Type: "
                 << coins[i].tradeType
                 << endl;

            return;
        }
    }

    cout << "Coin Not Found!"
         << endl;
}

void deleteCoin(Crypto coins[],
                int &count)
{
    string name;

    cout << "Enter Coin Name: ";

    cin >> name;

    for(int i = 0;
        i < count;
        i++)
    {
        if(coins[i].name == name)
        {
            for(int j = i;
                j < count - 1;
                j++)
            {
                coins[j] =
                coins[j + 1];
            }

            count--;

            cout << "Coin Deleted!"
                 << endl;

            return;
        }
    }

    cout << "Coin Not Found!"
         << endl;
}

void totalPortfolioValue(Crypto coins[],
                         int count)
{
    float total = 0;

    for(int i = 0;
        i < count;
        i++)
    {
        total +=
        coins[i].quantity *
        coins[i].currentPrice;
    }

    cout << "Total Portfolio Value: $"
         << total
         << endl;
int main()
{
    Crypto coins[100];

    int x = 0;

    int choice;

    do
    {
        cout << endl;

        cout << " CRYPTO FUTURES"
             << endl;

        cout << "1. Add Coin"
             << endl;

        cout << "2. View Portfolio"
             << endl;

        cout << "3. Search Coin"
             << endl;

        cout << "4. Delete Coin"
             << endl;

        cout << "5. Total Portfolio Value"
             << endl;

        cout << "6. Exit"
             << endl;

        cout << endl;

        cout << "Enter Your Choice: ";

        cin >> choice;

        if(choice == 1)
        {
            addCoin(coins, x);
        }
        else if(choice == 2)
        {
            viewPortfolio(coins, x);
        }
        else if(choice == 3)
        {
            searchCoin(coins, x);
        }
        else if(choice == 4)
        {
            deleteCoin(coins, x);
        }
        else if(choice == 5)
        {
            totalPortfolioValue(coins, x);
        }
        else if(choice == 6)
        {
            cout << endl;
            cout << "Exiting Program..."
                 << endl;
        }
        else
        {
            cout << endl;
            cout << "Invalid Choice!"
                 << endl;
        }

    }
    while(choice != 6);

    return 0;
}
