# 3-day-delay
found a premade code for backtesting the delay of a 3 day grace period
"""
Simulating a T+3 trading settlement date for Robinhood algorithms
"""


def initialize(context):
    # Necessary contest variable for simulating a T+3 settlement date
    context.last_sale = None

def handle_data(context, data):
    # `cash_settlement_date` simulates a T+3 settlement date. This
    # should be used at the beginning of any handle_data or method
    # used for schedule_function that places orders. At the end of
    # of that method should be `check_last_sale`. Only for
    # backtesting purposes!
    if cash_settlement_date(context):
        # Before running final backtest to begin live trading,
        # please comment this section out.
        log.info("Unsettled Cash Simulated, not trading")
        return
    else:
        order(sid(24), -1)
    
    # Only for backtesting purposes to simulate a T+3 Settlement Date
    # and to be used directly after a sale is made.
    # Please comment out or remove before Live Trading
    check_last_sale(context)
    
    
"""
Simulating T+3 in Backtesting
"""
    
def check_last_sale(context):
    """
    To be used at the end of each bar. This checks if there were
    any sales made and sets that to `context.last_sale`.
    `context.last_sale` is then used in `cash_settlement_date` to
    simulate a T+3 Cash Settlement date

    To only be used for backtesting!
    """
    open_orders = get_open_orders()
    most_recent_trade = []
    # If there are open orders check for the most recent sale
    if open_orders:
        for sec, order in open_orders.iteritems():
            for oo in order:
                if oo.amount < 0:
                    most_recent_trade.append(oo.created)
    if len(most_recent_trade) > 0:
        context.last_sale = max(most_recent_trade)
        
def cash_settlement_date(context):
    """
    This will simulate Robinhood's T+3 cash settlement. If the 
    most recent sale is less than 3 trading days from the current
    day, assume we have unsettled funds and exit

    To only be used for backtesting!
    """
    if context.last_sale and abs(get_datetime().weekday() - context.last_sale.weekday()) < 3:
        return True


The link is https://www.quantopian.com/algorithms/56a7917f2bd945e3aa000689
it was made bySeong Lee
he shares alot of helpful codes that could help us get started. 
visit his page if you have a moment
https://www.quantopian.com/users/51cbb24ab03fe0bc5500000e
