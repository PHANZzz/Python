import requests
import time
import asyncio
from telegram import Bot

# 🛠️ Telegram Bot Settings
TELEGRAM_TOKEN = "7309989178:AAGiSmLMqJNJkid7w9XnjZGBr_zEGZuyuWQ"  # Your Telegram Bot Token
CHAT_ID = "1686678295"  # Your Telegram Chat ID

# Initialize Telegram Bot
bot = Bot(token=TELEGRAM_TOKEN)

# 🌐 API Endpoints for Exchanges
EXCHANGES = {
    "Binance": "https://fapi.binance.com/fapi/v1/premiumIndex",
    "Bybit": "https://api.bybit.com/v2/public/tickers",
    "Gate.io": "https://api.gateio.ws/api/v4/futures/usdt/contracts",
    "Bitget": "https://api.bitget.com/api/mix/v1/market/funding-rate",
    "OKX": "https://www.okx.com/api/v5/public/funding-rate"
}

# 📌 Function to Fetch Funding Rates
def get_funding_rates():
    alerts = []

    # ✅ Binance
    try:
        response = requests.get(EXCHANGES["Binance"])
        if response.status_code == 200:
            data = response.json()
            print("Binance Data:", data)  # Debugging log
            for coin in data:
                symbol = coin['symbol']
                funding_rate = float(coin['lastFundingRate']) * 100
                if funding_rate < -1.0:
                    alerts.append(f"⚠️ Binance: {symbol} funding rate is {funding_rate:.2f}%")
    except Exception as e:
        print(f"Error fetching Binance data: {e}")

    # ✅ Bybit
    try:
        response = requests.get(EXCHANGES["Bybit"])
        if response.status_code == 200:
            data = response.json()["result"]
            print("Bybit Data:", data)  # Debugging log
            for coin in data:
                if "funding_rate" in coin:
                    symbol = coin['symbol']
                    funding_rate = float(coin['funding_rate']) * 100
                    if funding_rate < -1.0:
                        alerts.append(f"⚠️ Bybit: {symbol} funding rate is {funding_rate:.2f}%")
                else:
                    print(f"⚠️ No funding rate in Bybit data for coin: {coin}")
    except Exception as e:
        print(f"Error fetching Bybit data: {e}")

    # ✅ Gate.io
    try:
        response = requests.get(EXCHANGES["Gate.io"])
        if response.status_code == 200:
            data = response.json()
            print("Gate.io Data:", data)  # Debugging log
            for coin in data:
                symbol = coin['name']
                funding_rate = float(coin['funding_rate']) * 100
                if funding_rate < -1.0:
                    alerts.append(f"⚠️ Gate.io: {symbol} funding rate is {funding_rate:.2f}%")
    except Exception as e:
        print(f"Error fetching Gate.io data: {e}")

    # ✅ Bitget
    try:
        response = requests.get(EXCHANGES["Bitget"])
        if response.status_code == 200:
            data = response.json().get("data", [])
            print("Bitget Data:", data)  # Debugging log
            for coin in data:
                if 'fundingRate' in coin:
                    symbol = coin['symbol']
                    funding_rate = float(coin['fundingRate']) * 100
                    if funding_rate < -1.0:
                        alerts.append(f"⚠️ Bitget: {symbol} funding rate is {funding_rate:.2f}%")
                else:
                    print(f"⚠️ No funding rate in Bitget data for coin: {coin}")
    except Exception as e:
        print(f"Error fetching Bitget data: {e}")

    # ✅ OKX
    try:
        response = requests.get(EXCHANGES["OKX"])
        if response.status_code == 200:
            data = response.json().get("data", [])
            print("OKX Data:", data)  # Debugging log
            for coin in data:
                if 'fundingRate' in coin:
                    symbol = coin['instId']
                    funding_rate = float(coin['fundingRate']) * 100
                    if funding_rate < -1.0:
                        alerts.append(f"⚠️ OKX: {symbol} funding rate is {funding_rate:.2f}%")
                else:
                    print(f"⚠️ No funding rate in OKX data for coin: {coin}")
    except Exception as e:
        print(f"Error fetching OKX data: {e}")

    return alerts

# ✉️ Function to Send Telegram Alert (Asynchronous)
async def send_telegram_alert(message):
    try:
        await bot.send_message(chat_id=CHAT_ID, text=message)
        print("✅ Telegram alert sent successfully!")
    except Exception as e:
        print(f"❌ Error sending Telegram alert: {e}")

# 🔄 Run Script Every 5 Minutes (Asynchronous)
async def main():
    while True:
        alerts = get_funding_rates()
        if alerts:
            alert_message = "\n".join(alerts)
            await send_telegram_alert(alert_message)
        else:
            print("✅ No alerts. Waiting for the next check...")

        await asyncio.sleep(300)  # Wait for 5 minutes before checking again

# Run the script with asyncio
if __name__ == "__main__":
    asyncio.run(main())
