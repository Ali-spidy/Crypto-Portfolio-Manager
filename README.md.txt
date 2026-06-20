//STARTING OF PROJECT CREATION
structure  of code
struct Crypto
{
    string name;
    float walletBalance;
    float cost;
    float quantity;
    float entryPrice;
    float currentPrice;
    int leverage;
    string leverageType;
    string tradeType;
};

void addCoin(Crypto *coins, int &x)
{
    if(x >= 100)
    {
        cout << endl;
        cout << "Portfolio Full!" << endl;
        return;
    }

    cout << endl;
    cout << "Enter Coin Name: ";
    cin >> coins[x].name;

    cout << "Enter Wallet Balance: ";
    cin >> coins[x].walletBalance;

    cout << "Enter Position Cost: ";
    cin >> coins[x].cost;

    cout << "Enter Entry Price: ";
    cin >> coins[x].entryPrice;

    cout << "Enter Current Price: ";
    cin >> coins[x].currentPrice;

    cout << "Enter Leverage (10,20,50): ";
    cin >> coins[x].leverage;

    cout << "Enter Leverage Type (Cross/Isolated): ";
    cin >> coins[x].leverageType;

    cout << "Enter Trade Type (Long/Short): ";
    cin >> coins[x].tradeType;

    if(coins[x].entryPrice <= 0)
    {
        cout << endl;
        cout << "Invalid Entry Price!"
             << endl;
        return;
    }

    coins[x].quantity =
    (coins[x].cost * coins[x].leverage)
    /
    coins[x].entryPrice;

    x++;

    cout << endl;
    cout << "Coin Added Successfully!"
         << endl;
}


//function to search and delete coin
void searchCoin(Crypto *coins, int x)
{
    string searchName;

    bool found = false;

    cout << endl;
    cout << "Enter Coin Name to Search: ";

    cin >> searchName;

    for(int i = 0; i < x; i++)
    {
        if(coins[i].name == searchName)
        {
            float pnl;
            float pnlPercent;
            float liqPrice;
            float remainingWallet;

            pnl =
            calculatePnL(
            coins[i].quantity,
            coins[i].entryPrice,
            coins[i].currentPrice,
            coins[i].tradeType);

            pnlPercent =
            calculateProfitPercent(
            coins[i].entryPrice,
            coins[i].currentPrice,
            coins[i].leverage,
            coins[i].tradeType);

            liqPrice =
            liquidationPrice(
            coins[i].entryPrice,
            coins[i].quantity,
            coins[i].walletBalance,
            coins[i].leverage,
            coins[i].leverageType);

            remainingWallet =
            coins[i].walletBalance + pnl;

            if(remainingWallet < 0)
            {
                remainingWallet = 0;
            }

            cout << endl;
            cout << "Coin Found!"
                 << endl;

            cout << "Coin Name: "
                 << coins[i].name
                 << endl;

            cout << "Trade Type: "
                 << coins[i].tradeType
                 << endl;

            cout << "Wallet Balance: $"
                 << coins[i].walletBalance
                 << endl;

            cout << "Remaining Wallet Balance: $"
                 << remainingWallet
                 << endl;

            cout << "Position Cost: $"
                 << coins[i].cost
                 << endl;

            cout << "Quantity: "
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
                 << "x" << endl;

            cout << "Leverage Type: "
                 << coins[i].leverageType
                 << endl;

            cout << "Liquidation Price: $"
                 << liqPrice
                 << endl;

            cout << "PnL: $"
                 << pnl
                 << endl;

            cout << "PnL Percentage: "
                 << pnlPercent
                 << "%"
                 << endl;

            found = true;
        }
    }

    if(found == false)
    {
        cout << endl;
        cout << "Coin Not Found!"
             << endl;
    }
}
