# AI Prompt and Output Contract

## Purpose
The trip creation flow asks Gemini to generate hotel suggestions and a day-by-day itinerary based on destination, duration, traveler group, and budget. The integration is implemented in `src/service/AIModal.jsx` and called from `src/create-trip/index.jsx`.

Generated content is advisory. It is not a verified source of current prices, hours, availability, safety, travel times, or accessibility.

## Current model configuration
The service reads `VITE_GOOGLE_GEMINI_AI_API_KEY`, creates a Google Generative AI client, selects `gemini-1.5-flash`, and configures temperature, topP, topK, maximum output tokens, and JSON response MIME type. It creates a chat session with a lengthy embedded Las Vegas sample exchange.

Check current provider documentation before changing model identifiers or SDK behavior. The embedded history is prototype context, not a system of factual grounding.

## Prompt inputs
The template in `src/constants/Options.jsx` substitutes destination, total days, traveler group, and budget. The current trip form has inconsistent day-count spelling between `noOfdays` and `nOofdays`; it calculates a numeric conversion but does not use that converted value in its state update. Fixing that belongs in source code, not in documentation alone.

Validate destination length, positive integer duration, supported duration range, recognized budget values, and recognized traveler values before a provider call.

## Expected response structure
The UI expects a root object with `hotel_options` array and `itinerary` object. Example shape:

```json
{
  "hotel_options": [
    {
      "HotelName": "Example hotel",
      "HotelAddress": "Example address",
      "Price": "Sample range",
      "HotelImageUrl": "https://example.invalid/hotel.jpg",
      "GeoCoordinates": "0.0000, 0.0000",
      "Rating": "Example rating",
      "Description": "Example description"
    }
  ],
  "itinerary": {
    "day1": {
      "best_time_to_visit": "Morning",
      "places": [
        {
          "placeName": "Example place",
          "PlaceDetails": "Example description",
          "PlaceImageUrl": "https://example.invalid/place.jpg",
          "GeoCoordinates": "0.0000, 0.0000",
          "TicketPricing": "Unknown",
          "TimeToTravel": "Estimate only"
        }
      ]
    }
  }
}
```

This is a description of fields consumed by the current UI, not a formal schema or guarantee of model output.

## Current parser behavior
The form reads `result.response.text()` and passes the raw text to `JSON.parse`. A parse error is caught and a toast is displayed. There is no full schema validation, Markdown fence normalization, provider timeout, or bounded retry strategy.

Valid JSON can still have the wrong shape. A day may omit `places`, a hotel field may be null, or the root may be an array. Components currently assume required nested values exist.

## Validation required before saving
A safer implementation should parse controlled text, reject empty or oversized output, validate the root object, validate hotel and itinerary collections, validate every field used by renderers, constrain string lengths and item counts, reject unsafe URL protocols, normalize optional values, and write only validated records. Render model text as text, never as trusted HTML.

## Factual reliability
Model output can invent hotels, addresses, coordinates, prices, ticket fees, travel times, or image URLs. It can suggest closed attractions or impossible schedules. The interface should label results as AI-generated and instruct users to verify critical details with official providers. The project does not book hotels.

## Prompt injection and user content
Destination is user-controlled. Do not treat user text as trusted instructions. Separate fixed instructions from user fields, constrain output structure, and avoid sending unnecessary personal data. Prompt constraints do not replace schema validation or server-side authorization.

## Cost and abuse controls
The Gemini key is bundled into the browser. Visitors can extract and reuse it outside the app. Quotas and referrer restrictions help but are not authentication. Production generation should use a server function with authentication, request limits, input bounds, budget monitoring, and minimal safe logging.

## Change management
When the prompt or response changes, update parser validation, hotel and itinerary renderers, this contract, Firestore documentation, and tests. Consider compatibility with already saved documents. Do not rename fields without handling historical records.
