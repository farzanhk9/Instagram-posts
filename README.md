#!/usr/bin/env python3
# File: caption_generator.py
# A simple Python script to generate creative Instagram captions based on a topic, tone, and audience.

import argparse
import random

# Define emoji sets for different tones
EMOJIS = {
    "friendly": ["✨", "🔥", "💥", "💡", "😊", "🎉", "🙌", "💫", "🌟"],
    "luxury": ["💎", "🌙", "🎩", "🖤", "✨", "🌟", "👑"],
    "tech": ["💻", "⚡", "🚀", "🔧", "📈", "💡"],
    "sport": ["🏅", "⚡", "🔥", "💪", "🏃‍♂️", "🎯"],
}

# Define a set of call-to-actions (CTAs)
CTAS = [
    "Shop now", "Tap to see more", "DM us for details", 
    "Save this for later", "Comment your favorite", "Tag a friend", "Link in bio"
]

def pick_emojis(tone: str, k=2) -> str:
    """
    Pick k random emojis based on the tone.
    """
    pool = EMOJIS.get(tone, EMOJIS["friendly"])
    return " ".join(random.sample(pool, k=min(k, len(pool))))

def generate_hashtags(keywords: list, count=20) -> str:
    """
    Generate a list of hashtags based on the provided keywords.
    """
    hashtags = []
    # Create big, medium, and niche tags
    big_tags = ["fashion", "style", "trending", "reels", "explore", "shopnow"]
    medium_tags = [f"{k}daily" for k in keywords] + [f"{k}style" for k in keywords]
    niche_tags = [f"{k}community" for k in keywords] + [f"{k}review" for k in keywords]

    # Combine all tags
    pool = random.sample(big_tags, min(6, len(big_tags))) + \
           random.sample(medium_tags, min(7, len(medium_tags))) + \
           random.sample(niche_tags, min(12, len(niche_tags)))

    # Remove duplicates and limit to 'count' number of hashtags
    seen = set()
    for tag in pool:
        if tag not in seen and len(hashtags) < count:
            hashtags.append("#" + tag)
            seen.add(tag)
    return " ".join(hashtags)

def generate_caption(topic: str, tone: str, audience: str, hashtags: str) -> str:
    """
    Generate a caption based on the topic, tone, audience, and hashtags.
    """
    emojis = pick_emojis(tone)
    hook = f"{emojis} {topic.title()} you'll actually love."
    
    # Bullet points
    features = [
        "• High-quality materials",
        "• Durable and comfortable",
        "• Perfect for all occasions",
    ]
    body = "\n".join(features)

    cta = random.choice(CTAS)
    
    caption = f"""
{hook}

{body}

{cta} {emojis}
{hashtags}
"""
    return caption.strip()

def main():
    # Set up argument parser
    parser = argparse.ArgumentParser(description="Generate Instagram captions with hashtags.")
    parser.add_argument("--topic", required=True, help="Topic of the post (e.g., 'leather boots')")
    parser.add_argument("--tone", default="friendly", choices=["friendly", "luxury", "tech", "sport"], help="Tone of the caption")
    parser.add_argument("--audience", default="everyone", help="Audience for the caption (e.g., 'adults', 'fitness enthusiasts')")
    parser.add_argument("--keywords", default="", help="Comma-separated keywords related to the product")
    
    args = parser.parse_args()

    # Split keywords
    keywords = [k.strip() for k in args.keywords.split(",") if k.strip()]
    if not keywords:
        keywords = [args.topic]  # Use the topic as a fallback keyword
    
    # Generate hashtags
    hashtags = generate_hashtags(keywords, count=20)
    
    # Generate caption
    caption = generate_caption(args.topic, args.tone, args.audience, hashtags)
    
    print("\nGenerated Caption:\n")
    print(caption)
    print("\n---\nHashtags:\n", hashtags)

if __name__ == "__main__":
    main()
