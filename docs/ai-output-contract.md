# AI Prompt and Output Contract

## 1. Role of the AI integration

The trip creation flow asks a Gemini model to generate hotel suggestions and a day-by-day itinerary based on a destination, duration, traveler group, and budget category. The current integration is implemented in `src/service/AIModal.jsx` and called from `src/create-trip/index.jsx`.

Generated content is advisory. It is not a verified source of current prices, operating hours, travel times, hotel availability, safety information, ticket costs, or accessibility.

## 2. Current model configuration

The service:
- creates a `GoogleGenerativeAI` client with `VITE_GOOGLE_GEMINI_AI_API_KEY`;
- selects `gemini-1.5-flash`;
- sets temperature, topP, topK, maximum output tokens, and JSON response MIME type;
- creates a chat session with a large embedded Las Vegas sample exchange.

The model identifier and SDK behavior should be checked against current provider documentation before upgrading. Do not assume a model name remains available indefinitely.

## 3. User input fields

The prompt template in `src/constants/Options.jsx` interpolates:
- `location`: destination text;
- `totalDays`: requested trip duration;
- `traveler`: traveler category;
- `budget`: budget category.

The current source uses inconsistent capitalization for the day count key: some code references `noOfdays` and other code references `nOofdays`. The number conversion result is calculated but the state update currently stores the original value. These are implementation defects to address separately from documentation.

Inputs should be validated before contacting the model. At minimum, validate destination length, integer day count, upper/lower duration limits, recognized budget values, and recognized traveler categories.

## 4. Expected response shape

The rendering components expect a structure similar to:

```json
{
  "hotel_options": [
    {
      "HotelName": "Example hotel",
      "HotelAddress": "Example address",
      "Price": "Sample price range",
      "HotelImageUrl": "https://example.invalid/image.jpg",
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

This example documents the fields expected by current components. It is not a formal JSON Schema or guaranteed model response.

## 5. Current parser behavior

The form reads `result.response.text()` and passes the text to `JSON.parse`. It catches a parse exception and displays a toast, but there is no complete schema validation, Markdown-fence normalization, retry policy, or provider timeout handling.

A valid JSON string can still have the wrong shape. For example, `itinerary` could be an array, a day object could lack `places`, or a hotel field could be null. Components currently assume several nested properties exist.

## 6. Required validation before persistence

A safer implementation should:
1. parse the response in a controlled function;
2. reject empty or excessively large responses;
3. validate that the root is an object;
4. validate that `hotel_options` is an array;
5. validate every hotel field used by the UI;
6. validate that `itinerary` is a plain object;
7. validate each day record and its `places` array;
8. constrain string lengths and array counts;
9. normalize absent optional values;
10. reject unsupported URL protocols;
11. store only validated and size-limited data;
12. render all model text as text, never trusted HTML.

Use a schema validator such as Zod only if the project deliberately adds it as a dependency and documents its use.

## 7. Safety and factual reliability

Model-generated content may hallucinate:
- hotels that do not exist;
- incorrect addresses or coordinates;
- stale prices and ticket fees;
- closed attractions;
- impossible daily schedules;
- incorrect travel duration;
- image URLs that fail or point to unrelated content.

The UI must label suggestions as AI-generated and instruct users to verify critical details with official providers. Do not represent generated hotel cards as booking inventory.

## 8. Prompt injection and user content

Destination input is user-controlled. Do not treat arbitrary user text as trusted system instructions. Future prompt design should separate instruction hierarchy from user-supplied fields, constrain output format, and avoid passing private user data unnecessarily.

Prompt constraints do not replace output validation or server-side authorization.

## 9. Cost and abuse controls

A browser-exposed Gemini API key can be extracted and reused outside the application. Provider-side quotas and referrer restrictions can limit some abuse but are not a complete authentication boundary. Production generation should pass through a server-side function with authenticated identity, request limits, input bounds, usage logging that avoids sensitive content, and budget monitoring.

## 10. Change management

When changing the prompt or expected response:
- update the prompt template;
- update parser and validation tests;
- update hotel and itinerary renderers;
- update this document and Firestore data documentation;
- test missing fields and malformed JSON;
- confirm older saved trips still render or provide a migration/fallback.

Do not silently change key names without considering already stored Firestore documents.
