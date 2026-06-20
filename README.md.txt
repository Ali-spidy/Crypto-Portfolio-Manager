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
