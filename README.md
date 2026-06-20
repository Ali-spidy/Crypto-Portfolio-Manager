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
