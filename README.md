# Sample articles data
articles = [
    {
        "title": "Article 1",
        "score": 0,
        "content": "This is an article about a major flood affecting the region.",
        "geographical_impact": 500,  # in square kilometers
        "people_impacted": 10000,
        "long_term_repercussions": 8,  # scale of 1-10
        "author": {
            "name": "Author A",
            "political_views": "neutral",
            "education": "PhD in Environmental Science",
            "aim": "Informative"
        },
        "related_articles": ["Article 2", "Article 3"]
    },
    {
        "title": "Article 2",
        "score": 0,
        "content": "This article discusses the long-term effects of climate change.",
        "geographical_impact": 2000,
        "people_impacted": 25000,
        "long_term_repercussions": 5,
        "author": {
            "name": "Author B",
            "political_views": "progressive",
            "education": "Master's in Climate Studies",
            "aim": "Advocacy"
        },
        "related_articles": ["Article 1"]
    },
    {
        "title": "Article 3",
        "score": 0,
        "content": "A major earthquake has caused devastation in the city, "
           "impacting thousands.",
        "geographical_impact": 1500,
        "people_impacted": 15000,
        "long_term_repercussions": 9,
        "author": {
            "name": "Author C",
            "political_views": "neutral",
            "education": "Journalism Degree",
            "aim": "Informative"
        },
        "related_articles": ["Article 1"]
    }
]

def simple_sentiment_analysis(content):
    positive_words = ["major", "good", "great", "impact", "benefit"]
    negative_words = ["devastation", "affecting", "problem", "concern"]

    score = 0
    for word in content.split():
        if word.lower() in positive_words:
            score += 1
        elif word.lower() in negative_words:
            score -= 1

    return score

def fetch_external_data(article_title):
    # Simulating an API call to fetch external data
    external_data = {
        "Article 1": {"additional_coverage": 10},
        "Article 2": {"additional_coverage": 20},
        "Article 3": {"additional_coverage": 30}
    }
    return external_data.get(article_title, {"additional_coverage": 0})

def evaluate_author_reliability(author):
    reliability_score = 0

    # Assess reliability based on political views
    if author['political_views'] == "neutral":
        reliability_score += 3
    elif author['political_views'] == "progressive":
        reliability_score += 2
    elif author['political_views'] == "conservative":
        reliability_score += 1

    # Assess based on education
    if "PhD" in author['education']:
        reliability_score += 3
    elif "Master's" in author['education']:
        reliability_score += 2
    elif "Journalism" in author['education']:
        reliability_score += 1

    # Assess based on aim
    if author['aim'] == "Informative":
        reliability_score += 3
    elif author['aim'] == "Advocacy":
        reliability_score += 1

    return reliability_score

def cross_reference_articles(article_title, articles):
    related_scores = []

    for article in articles:
        if article['title'] == article_title:
            continue
        if article_title in article['related_articles']:
            related_scores.append(article['score'])

    return sum(related_scores) / len(related_scores) if related_scores else 0

def evaluate_articles(articles):
    for article in articles:
        # Analyze sentiment
        sentiment_score = simple_sentiment_analysis(article['content'])
        article['sentiment_score'] = sentiment_score

        # Fetch external data
        external_data = fetch_external_data(article['title'])
        article['external_coverage'] = external_data['additional_coverage']

        # Evaluate author reliability
        article['author_reliability'] = evaluate_author_reliability(article['author'])

        # Cross-reference scores
        article['cross_reference_score'] = \
cross_reference_articles(article['title'], articles)

        # Calculate a relevance score based on the criteria
        article['score'] = (article['geographical_impact'] * 0.2 +
                            article['people_impacted'] * 0.2 +
                            article['long_term_repercussions'] * 0.2 +
                            sentiment_score * 0.1 +
                            article['external_coverage'] * 0.1 +
                            article['author_reliability'] * 0.1 +
                            article['cross_reference_score'] * 0.1)

    # Sort articles by score in descending order
    sorted_articles = sorted(articles, key=lambda x: x['score'], reverse=True)

    return sorted_articles

# Get all evaluated articles
evaluated_articles = evaluate_articles(articles)

# Most relevant article
most_relevant_article = evaluated_articles[0]
print(f"The most relevant article is: {most_relevant_article['title']}")

# Most reliable article based on author reliability
most_reliable_article = sorted(
    evaluated_articles, key=lambda x: x['author_reliability'], reverse=True
)[0]
print(f"The most reliable article is: {most_reliable_article['title']}")

