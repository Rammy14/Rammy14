import requests
import random
import time

API_URL = "https://api.deriv.com"
API_KEY = "your_api_key_here"
ACCOUNT_ID = "your_account_id"

# Step 1: Authenticate and get account details
def authenticate():
    payload = {
        "authorize": {
            "api_key": API_KEY
        }
    }
    response = requests.post(API_URL, json=payload)
    return response.json()

# Step 2: Place a Digit Match Trade
def place_trade(last_digit):
    # Generate a random stake amount
    stake = 1  # You can adjust this value
    market = "synthetic_index"  # Example: synthetic index or a market you want to trade
    
    # Make sure you adapt this URL and payload to match Deriv's actual trade placement process
    payload = {
        "trade": {
            "account_id": ACCOUNT_ID,
            "market": market,
            "stake": stake,
            "digit": last_digit,  # The last digit you're predicting
            "action": "match",  # Action based on Digit Match rules
            "duration": 1,  # 1-second expiry
        }
    }
    
    response = requests.post(f"{API_URL}/place_trade", json=payload)
    return response.json()

# Step 3: Start the bot and make trades
def start_trading():
    while True:
        # Randomly choose a last digit (0-9) as an example
        predicted_digit = random.randint(0, 9)
        
        print(f"Predicting last digit: {predicted_digit}")
        
        # Place the trade with the predicted digit
        trade_response = place_trade(predicted_digit)
        print(f"Trade response: {trade_response}")
        
        # Wait for a short period before making the next trade
        time.sleep(1)

if __name__ == "__main__":
    authenticate()
    start_trading()
