# 🧭 Smart Travel System — Filtering & Personalization

### Version: 2.0

**Updated:** October 2025  
**Authors:** SmartTravelSystem Development Team (Fanshawe College)

---

## 📚 Table of Contents

1. [Overview](#overview)
2. [Filter Overview](#filter-overview)
3. [Travel Type](#travel-type)
4. [Activity Theme](#activity-theme)
5. [Kid-Friendly](#kid-friendly)
6. [Budget Level](#budget-level)
7. [Query Combination Logic](#query-combination-logic)
8. [Gemini Integration](#gemini-integration)
9. [API Request Example](#api-request-example)
10. [Summary Table](#summary-table)

---

## 🧠 Overview

The **Smart Travel System** integrates the **Google Places API** and **Google Gemini 2.5 Flash** model to generate highly personalized travel itineraries.  
Users can specify multiple filters such as **Budget**, **Kid-Friendliness**, **Travel Type**, and **Activity Theme**.  
These filters directly influence:

- Which _places_ are retrieved from Google Places.
- How **Gemini** writes and tones the generated itinerary.

---

## 🎯 Filter Overview

Each filter affects both the **data selection** (Google Places) and the **AI narrative personalization** (Gemini).  
The system dynamically constructs queries like: 'top tourist attractions in Waterloo romantic places relaxing'

and sends them to the Google Places API, then passes filtered results to Gemini to write a narrative.

---

## 🧍‍♂️ Travel Type

Defines _who_ is traveling, influencing both the selected attractions and the tone of the itinerary.

| Type        | Purpose                                                            | Google Places Query Behavior                                      | Gemini Narrative Tone                                 |
| ----------- | ------------------------------------------------------------------ | ----------------------------------------------------------------- | ----------------------------------------------------- |
| **Solo**    | For independent travelers seeking safe and reflective experiences. | Adds `solo travel spots`.                                         | Calm, introspective tone. “Explore at your own pace.” |
| **Couple**  | Romantic experiences for two.                                      | Adds `romantic places`.                                           | Soft and cozy tone. “Enjoy a romantic dinner…”        |
| **Family**  | Kid-friendly and safe for all ages.                                | Adds `family friendly` keyword; also triggers kid-friendly logic. | Warm tone. “Fun for the whole family.”                |
| **Friends** | Group-oriented, social experiences.                                | Adds `fun group activities`.                                      | Energetic tone. “Perfect for group fun.”              |

**Example:**  
 top tourist attractions in Toronto romantic places

→ Gemini will describe cozy restaurants, scenic walks, and intimate spots.

---

## 🎨 Activity Theme

Represents _what kind of experience_ the user prefers — from adventure to relaxation.

| Theme         | Purpose                                | Google Places Keyword | Gemini Narrative Style                                  |
| ------------- | -------------------------------------- | --------------------- | ------------------------------------------------------- |
| **Adventure** | Outdoor or thrill-based travel.        | Adds `adventure`.     | Energetic, uses words like “explore”, “hike”, “embark”. |
| **Relaxing**  | Calm, peaceful itinerary.              | Adds `relaxing`.      | Soothing tone: “Unwind by the lakeside spa…”            |
| **Cultural**  | Focused on history, art, and heritage. | Adds `cultural`.      | Educational and reflective tone.                        |
| **Shopping**  | Retail and lifestyle focus.            | Adds `shopping`.      | Leisurely tone: “Browse trendy boutiques…”              |

**Example Query:**  
 top tourist attractions in Banff adventure

→ Emphasizes hiking trails, mountain viewpoints, and outdoor excitement.

---

## 👨‍👧 Kid-Friendly

Ensures that only safe, family-appropriate attractions are included.

**Logic**

- When `kid_friendly = true`, the system keeps only places whose types include `"park"` or `"museum"`.
- Filters out bars, clubs, or adult-only activities.
- Gemini switches to a warm, educational tone.

**Example Output**

> “Spend the afternoon at the Cambridge Butterfly Conservatory — a delightful place where kids can explore safely.”

---

## 💰 Budget Level

Controls the affordability and tone of activities and dining recommendations.

| Value | Label       | Description                                 |
| ----- | ----------- | ------------------------------------------- |
| **0** | Free        | Parks, landmarks, or zero-cost activities.  |
| **1** | Inexpensive | Budget-friendly casual experiences.         |
| **2** | Moderate    | Mid-range restaurants and tours.            |
| **3** | Expensive   | Premium restaurants and private activities. |
| **4** | Luxury      | Exclusive, high-end experiences.            |

**Implementation**

- Filters by the Google Places `price_level` (0–4).
- Gemini adjusts tone:
  - _Budget 1:_ “Affordable local café.”
  - _Budget 4:_ “Fine dining with panoramic city views.”

---

## ⚙️ Query Combination Logic

```python
query_parts = [f"top tourist attractions in {destination}"]

if activity_theme:
    query_parts.append(activity_theme)

if travel_type:
    if travel_type == "couple":
        query_parts.append("romantic places")
    elif travel_type == "family":
        query_parts.append("family friendly")
    elif travel_type == "friends":
        query_parts.append("fun group activities")
    elif travel_type == "solo":
        query_parts.append("solo travel spots")

att_query = " ".join(query_parts)
```

**Example Combined Query**
top tourist attractions in Waterloo romantic places relaxing
→ Romantic + Relaxing tone in itinerary.

---

## 📸 POI Images Feature (Google Places Photo API)

The **Smart Travel System** now supports high-quality images for every point of interest (POI) — including both **attractions** and **restaurants** — using the **Google Places API**.

### 🧠 How It Works

1. **Text Search API**

   - Used to find places based on destination and filters.
   - Returns `place_id`, `name`, and one sample photo reference.

2. **Place Details API**

   - Uses each `place_id` to fetch up to **five photos per place**.
   - Field used: `fields=photos` (lightweight call).
   - Each photo reference is converted into a direct image URL with:
     ```
     https://maps.googleapis.com/maps/api/place/photo?maxwidth=800&photo_reference=...&key=YOUR_API_KEY
     ```

3. **Backend Combination Flow**
   ```text
   Text Search → Place ID → Place Details → Photo References → Photo URLs
   ```
