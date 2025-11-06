# 🌍 Smart Travel System — Backend (Developed by Ei Ei Khaing)

This repository contains my backend contributions to the Fanshawe College Capstone project **Smart Travel System**, an AI-powered travel planning assistant that integrates real-time Google data with generative AI for personalized travel experiences.

I developed two key backend modules — **Smart Itinerary** and **Sentiment Insights** — using **FastAPI**, **Google Places API**, and **Gemini Flash 2.5**.  
These features enable the system to generate personalized travel itineraries and analyze user reviews dynamically.

---

## 🚀 Project Overview

### 🧭 Smart Itinerary
Generates personalized, day-by-day travel itineraries using **Google Places API** and **Gemini LLM**.

#### 🔹 Core Workflow
1. Receives user input — destination city, trip duration, and preferences  
2. Builds filtered Google Places queries for attractions and restaurants  
3. Retrieves photo references and metadata (name, rating, price, types)  
4. Sends the structured data + user filters to **Gemini Flash 2.5**  
5. Gemini generates a **natural-language itinerary** (Morning / Afternoon / Evening) with descriptions tailored to preferences  

#### 🔹 Personalization Filters
| Filter | Description | Example |
|--------|--------------|----------|
| **Travel Type** | Solo / Couple / Family / Friends | “Perfect for couples who enjoy peaceful escapes.” |
| **Activity Theme** | Adventure / Cultural / Relaxing / Shopping | “Start your morning with an exciting hike...” |
| **Kid-Friendly** | Boolean filter | Excludes bars, nightlife venues |
| **Budget Level** | 0–4 (Free → Luxury) | Controls both price level and descriptive tone |
| **Days** | Number of days to plan | Adjusts day count dynamically |

#### 🔹 Output Example
```json
{
  "city": "Toronto",
  "days": 2,
  "itinerary": [
    {
      "day": 1,
      "morning": "Explore the CN Tower and nearby cafes.",
      "afternoon": "Visit Ripley's Aquarium.",
      "evening": "Enjoy dinner at a lakeside restaurant."
    }
  ]
}
```

---

### 💬 Sentiment Insights
Analyzes Google Maps reviews for selected places to produce concise AI-generated summaries.

#### 🔹 Process Flow
1. Retrieve reviews from **Google Places Details API** via Place ID  
2. Aggregate multiple review texts  
3. Use **Gemini Flash 2.5** or a pretrained sentiment model (e.g. BERT/DistilBERT)  
4. Generate a structured sentiment summary  

#### 🔹 Output Example
```json
{
  "place_id": "ChIJmzrzi9Y0K4gR3Y1XxJ8zN7M",
  "overall_sentiment": "Positive",
  "summary": "Visitors love the atmosphere and friendly staff, though parking is limited."
}
```

#### 🔹 Highlights
- Real-time, **zero-dataset** review analysis using Google Places data  
- Supports multiple languages and summarization styles via Gemini  
- Scalable for integration with itinerary recommendations  

---

### 🗣️ Language Buddy (Planned 🚧)
Upcoming module for multilingual translation and conversational travel assistance using **Gemini Flash 2.5** multilingual capabilities.

---

## 🧠 Architecture

```
SmartTravelSystem_Backend_EiEiKhaing/
│
├── app.py
├── routers/
│   ├── itinerary.py          # Smart Itinerary endpoint
│   ├── sentiment.py          # Sentiment Insights endpoint
│
├── services/
│   ├── gemini_service.py     # Handles Gemini LLM API interactions
│   ├── places_service.py     # Handles Google Places API requests
│   └── sentiment_service.py  # Performs review sentiment analysis
│
├── models/
│   ├── schemas.py            # Pydantic request/response models
│
├── docs/
│   ├── Smart_Itinerary_filters_guide.md
│
└── requirements.txt
```

**Framework:** FastAPI  
**LLM Integration:** Gemini Flash 2.5  
**External APIs:** Google Places API, Google Photos API, Google Maps Reviews API  

---

## ⚙️ API Endpoints

| Endpoint | Method | Description |
|-----------|--------|-------------|
| `/itinerary/generate` | POST | Generate itinerary using city + filters |
| `/sentiment/{place_id}` | GET | Analyze sentiment for a Google Place |
| `/chatbot/query` | POST | (Coming soon) Multilingual chatbot |

---

## 🧰 Tech Stack
- **FastAPI** for API framework  
- **Python 3.10+**  
- **Gemini Flash 2.5** for AI-based text generation  
- **Google Places / Photos / Maps APIs**  
- **SQLAlchemy** & **SQLite** for local persistence  
- **Requests**, **Pydantic**, **Uvicorn**

---

## 🔐 Environment Variables

Copy `.env.example` and fill in your own API keys:

```
GEMINI_API_KEY=your_gemini_api_key_here
GOOGLE_API_KEY=your_google_places_key_here
```

---

## ⚙️ How to Run
```bash
# 1️⃣ Install dependencies
pip install -r requirements.txt

# 2️⃣ Run FastAPI server
uvicorn app:app --reload

# 3️⃣ Test endpoints
http://127.0.0.1:8000/docs
```

---

## 📄 Status Summary
| Module | Status | Description |
|---------|---------|-------------|
| Smart Itinerary | ✅ Completed | Integrated Google Places & Gemini |
| Sentiment Insights | ✅ Completed | Real-time review summarization |
| Language Buddy | 🚧 Planned | Multilingual conversational support |

---

## 📈 Future Enhancements
- Integrate user authentication for personalized itinerary history  
- Add caching layer to reduce Google API cost  
- Expand Gemini prompts for richer itinerary storytelling  
- Deploy on **Render** or **AWS Lambda** for public demo  

---

## 👩‍💻 Author
**Ei Ei Khaing**  
Graduate Certificate in Artificial Intelligence & Machine Learning  
Fanshawe College | London, Ontario, Canada  

📧 [LinkedIn](https://www.linkedin.com/in/ei-khaing-42595028)  
🔗 [GitHub Portfolio](https://github.com/9eek9)

---

## 🏷️ Keywords
`FastAPI` `Gemini API` `Google Places` `AI Backend` `Travel Itinerary` `Sentiment Analysis` `Generative AI` `Python`
