from flask import Flask, render_template, request, jsonify
from textblob import TextBlob

app = Flask(__name__)

def analyze_sentiment(message):
    analysis = TextBlob(message)
    polarity = analysis.sentiment.polarity
    if polarity > 0.1:
        sentiment = "Positive"
        reply = "😊 I'm glad to hear that!"
    elif polarity < -0.1:
        sentiment = "Negative"
        reply = "😟 I'm sorry you feel that way."
    else:
        sentiment = "Neutral"
        reply = "😐 Okay, I understand."
    return sentiment, reply

@app.route("/")
def index():
    return render_template("index.html")

@app.route("/get_sentiment", methods=["POST"])
def get_sentiment():
    user_message = request.form["message"]
    sentiment, reply = analyze_sentiment(user_message)
    return jsonify({"sentiment": sentiment, "reply": reply})

if __name__ == "__main__":
    app.run(debug=True)
