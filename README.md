# AI-powered-Personalized-News-Summarization
利用自然语言处理技术为用户生成个性化的新闻摘要，节省时间并提高信息获取效率。
from transformers import pipeline
import random

# Simulated news database
news_db = [
    {
        "title": "Global Market Trends",
        "content": "Today, global markets showed a mixed response amid varying economic signals..."
    },
    {
        "title": "Tech Innovation Conference 2024",
        "content": "This year's Tech Innovation Conference showcased groundbreaking technologies..."
    },
    {
        "title": "Environmental Policies Update",
        "content": "Several countries have announced new environmental policies aimed at reducing carbon emissions..."
    }
]

# User profiles for personalization (example)
user_profiles = {
    "user1": {"interests": ["technology", "innovation"]},
    "user2": {"interests": ["environment", "policy"]}
}

# News summarization pipeline
summarizer = pipeline("summarization")

def fetch_news(user_interests):
    """Simulate fetching news based on user interests."""
    relevant_news = [news for news in news_db if any(interest in news["title"].lower() for interest in user_interests)]
    if not relevant_news:  # If no specific interests match, return a random article
        relevant_news = [random.choice(news_db)]
    return relevant_news

def summarize_news(news_article):
    """Summarize a news article."""
    summary = summarizer(news_article["content"], max_length=50, min_length=25, do_sample=False)
    return summary[0]['summary_text']

def personalized_news_summary(user_id):
    """Generate a personalized news summary for a given user."""
    user_interests = user_profiles.get(user_id, {}).get("interests", [])
    fetched_news = fetch_news(user_interests)
    
    summaries = []
    for article in fetched_news:
        summaries.append({
            "title": article["title"],
            "summary": summarize_news(article)
        })
    
    return summaries

# Example usage
user_id = "user1"
summaries = personalized_news_summary(user_id)
for summary in summaries:
    print(f"Title: {summary['title']}\nSummary: {summary['summary']}\n")
