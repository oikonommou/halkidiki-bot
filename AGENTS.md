# Halkidiki Experience AI Chatbot Instructions

This repository contains the WordPress plugin for the Halkidiki Experience AI chatbot.

Main file:
halkidiki-ai-planner.php

Main REST endpoint:
/wp-json/halkidiki-ai/v1/chat

Main shortcode:
[halkidiki_ai_planner]

Main local dataset:
data/halkidiki-places.json

Core rule:
The chatbot must never invent partner businesses. Business recommendations must come only from real WordPress listings returned by deterministic filtering.

Business recommendation flow:
1. Detect the user's requested village/region.
2. Detect the user's intent/category.
3. Query exact-region partner listings first.
4. Filter by category/features.
5. If exact matches exist, use only exact matches.
6. If exact matches are zero, use only nearby villages from halkidiki_ai_get_region_clusters().
7. Return maximum 6 businesses.
8. If more than 6 matches exist, rotate or shuffle results so the same first 6 are not always shown.
9. Clearly label nearby fallback businesses with their real village.
10. Never imply that nearby fallback businesses are inside the requested village.

Important categories:
- food: restaurants, taverns, fast food, pizza, snack bar
- brunch: Brunch first, Cafe-Snacks only as fallback if needed
- drink/nightlife: Bars, Clubs, Beach Bars, Cafe & Cocktail / Cafe & Coctail
- coffee: Cafe-Snacks, Cafe & Cocktail / Cafe & Coctail, coffee-related categories
- dessert/ice cream: dessert, bakery, ice cream categories
- stay: hotels, apartments, villas, studios
- activities: cruises, rentals, water sports, kids club, gyms, massage etc.

Do not mix unrelated categories.
If the user asks for drink, do not return taverns or restaurants.
If the user asks for brunch, do not return generic food unless no brunch exists and the answer clearly explains the fallback.

Day plan rules:
For full-day itinerary requests, ask clarification questions first if important details are missing:
- starting village/location,
- whether the user has a car,
- preferred style: relaxed, beach, sightseeing, family, couple, nightlife etc.

Use partner listings for food, brunch, coffee and drink.
Use data/halkidiki-places.json for beaches, attractions and places.
Do not blindly use the first beach or attraction in the JSON.
Select places based on requested region and nearby villages where possible.

Tone:
Answer in Greek unless the user writes in English.
Use polite plural form in Greek.
Sound warm, natural and helpful, like a local tourist guide.
Avoid markdown symbols, robotic bullet lists and raw URLs.
Write every business name exactly as it exists in the data.

Testing:
Always add or update tests before changing behavior.
The core test queries are:
1. Θέλω να φάω στο Πευκοχώρι
2. Θέλω brunch στο Πευκοχώρι
3. Θέλω να πιω ένα ποτό στην Άφυτο
4. Θέλω να φάω brunch στη Φούρκα
5. Θέλω να μου φτιάξεις πρόγραμμα για όλη τη μέρα στο Πευκοχώρι

The tests must check:
- maximum 6 businesses,
- correct region,
- correct category,
- no unrelated business types,
- nearby fallback only when exact matches are zero,
- nearby fallback is clearly labeled,
- day plan asks clarification questions first when information is missing.

Rate limit:
The production chatbot has rate limiting.
Tests must either wait long enough or safely bypass rate limiting only in local/test mode.
Never disable production rate limiting.
