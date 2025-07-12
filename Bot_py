from flask import Flask, request
import requests
import json

app = Flask(__name__)

TELEGRAM_TOKEN = "8150485685:AAHCZIqxYVHU3nUzhTaaVzOViiF_Z9zzEM4"
TELEGRAM_API_URL = f"https://api.telegram.org/bot{TELEGRAM_TOKEN}/sendMessage"

# تحليل مبسط للخبر
def analyze_news(news_text):
    news_lower = news_text.lower()
    if "raise interest rate" in news_lower or "hawkish" in news_lower:
        return "📈 التوصية: بيع EUR/USD - بيع الذهب"
    elif "cut interest rate" in news_lower or "dovish" in news_lower:
        return "📉 التوصية: شراء EUR/USD - شراء الذهب"
    elif "inflation rises" in news_lower:
        return "📈 التوصية: بيع EUR/USD - بيع الذهب"
    elif "inflation falls" in news_lower:
        return "📉 التوصية: شراء EUR/USD - شراء الذهب"
    else:
        return "⚠️ لم يتم تحديد توصية تلقائية - راجع التحليل يدوياً"

# إرسال رسالة إلى التلغرام
def send_message(chat_id, text):
    payload = {
        "chat_id": chat_id,
        "text": text
    }
    requests.post(TELEGRAM_API_URL, json=payload)

# Webhook لاستقبال التحديثات
@app.route("/webhook", methods=["POST"])
def webhook():
    data = request.json
    if "message" in data and "text" in data["message"]:
        chat_id = data["message"]["chat"].get("id")
        user_msg = data["message"]["text"]

        if "news" in user_msg.lower():
            example_news = "The Federal Reserve decided to raise interest rate by 0.25%."
            analysis = analyze_news(example_news)
            msg = f"📰 خبر عاجل:\n{example_news}\n\n{analysis}"
            send_message(chat_id, msg)
        else:
            send_message(chat_id, "أرسل كلمة 'news' لتجربة الخبر والتحليل")
    return "ok"

@app.route("/")
def index():
    return "Forex & Gold News Bot Active!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
