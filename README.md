# PartyPlaneGenerator (2025)

Generate personalized birthday invitation cards, custom t-shirt designs, and AI-powered party plans using FastAPI, Google Generative AI, and image processing.

## Features

- **🎂 Birthday Card Generation**: Create personalized invitation cards with custom themes, messages, and AI-generated images
- **👕 T-Shirt Design**: Generate custom t-shirt designs and mockups based on user preferences
- **🎉 Party Planning**: AI-powered party planning with gift suggestions and product recommendations
- **☁️ Cloud Integration**: Automatic image upload to Cloudinary for storage and delivery
- **🤖 AI-Powered**: Uses Google Generative AI (Gemini 2.5) for text and image generation

## Tech Stack

- **Backend**: FastAPI, Uvicorn
- **AI/ML**: Google Generative AI (Gemini 2.5), ONNX Runtime
- **Image Processing**: Pillow, rembg (background removal)
- **Cloud Storage**: Cloudinary
- **Utilities**: Tenacity (retry logic), python-dotenv
- **Validation**: Pydantic

## Installation & Setup

### Prerequisites

- Python 3.8+
- Google Generative AI API key
- Cloudinary account credentials

### Installation Steps

1. **Clone the repository**:

   ```bash
   git clone <repo-url>
   cd PartyPlaneGenerator
   ```

2. **Install dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

3. **Environment Configuration**:
   Create a `.env` file in the root directory:

   ```env
   GEMINI_API_KEY=your_gemini_api_key
   CLOUDINARY_CLOUD_NAME=your_cloud_name
   CLOUDINARY_API_KEY=your_api_key
   CLOUDINARY_API_SECRET=your_api_secret
   ```

4. **Run the application**:

   ```bash
   uvicorn main:app --reload
   ```

5. **Access API Documentation**:
   - Swagger UI: http://127.0.0.1:8000/docs
   - ReDoc: http://127.0.0.1:8000/redoc

## API Endpoints

### 1. Birthday Card Generation

**Endpoint**: `POST /api/v1/generate-card`

**Description**: Generates personalized birthday invitation cards with AI-generated text and images.

**Request Body**:

```json
{
  "theme": "Football lover",
  "description": "Playing a boy football with cake",
  "age": 10,
  "gender": "Male",
  "birthday_person_name": "Prinom",
  "venue": "Party Hall",
  "date": "12 Oct 2025",
  "time": "4:00 PM",
  "contact_info": "01610982021"
}
```

**Response**:

```json
{
  "invitation_text": "Generated invitation text...",
  "images": [
    {
      "url": "https://cloudinary.com/image-url",
      "public_id": "image_public_id"
    }
  ]
}
```

### 2. T-Shirt Design Generation

**Endpoint**: `POST /t_shirt_generate`

**Description**: Creates custom t-shirt designs and mockups based on user specifications.

**Request**: Form Data

- `t_shirt_type` (required): Type of t-shirt (Adult or child)
- `t_shirt_size` (required): Size (S, M, L, XL)
- `gender` (required): Gender fit (male, female)
- `t_shirt_color` (required): Base color (black, white, red, etc.)
- `age` (required): Age of target wearer
- `t_shirt_theme` (required): Theme/style (birthday, sports, cartoon)
- `optional_description` (optional): Additional design description
- `img_file` (optional): Image file for design reference

**Response**:

```json
{
  "generated_design_url": "https://cloudinary.com/design-url",
  "generated_mockup_url": "https://cloudinary.com/mockup-url"
}
```

**Supported File Types**: JPEG, PNG, BMP

### 3. Party Planning

**Endpoint**: `POST /party_generate`

**Description**: Generates comprehensive party plans with gift suggestions and product recommendations.

**Request Body**:

```json
{
  "person_name": "John",
  "person_age": 8,
  "budget": 1500.0,
  "num_guests": 15,
  "party_date": "2025-12-15",
  "location": "Home",
  "party_details": {
    "theme": "Superhero",
    "favorite_activities": ["games", "dancing", "crafts"]
  },
  "num_product": 5
}
```

**Response**:

```json
{
  "party_plan": {
    "🎨 Theme & Decorations": ["decoration suggestions"],
    "🎉 Fun Activities": ["activity list"],
    "🍔 Food & Treats": ["food suggestions"],
    "🛍️ Party Supplies": ["supply list"],
    "⏰ Party Timeline": ["timeline with emojis"],
    "🎁 Suggested Gifts": ["gift suggestions"],
    "🌟 New Adventure Ideas": ["adventure ideas"]
  },
  "suggest_gifts": ["product recommendations"],
  "all_product": ["available products"]
}
```

## Project Structure

```
PartyPlaneGenerator/
├── app/
│   ├── api/v1/endpoints/          # API route handlers
│   │   ├── generate_card.py       # Birthday card endpoints
│   │   ├── t_shirt_endpoint.py    # T-shirt design endpoints
│   │   └── generate_party.py      # Party planning endpoints
│   ├── schemas/                   # Pydantic models
│   │   ├── invite.py             # Birthday card schemas
│   │   └── schema.py             # T-shirt and party schemas
│   ├── services/                  # Business logic
│   │   ├── generator.py          # Card generation service
│   │   ├── t_shirt/shirt.py      # T-shirt generation service
│   │   └── party/party.py        # Party planning service
│   ├── utils/                     # Utilities
│   │   ├── helper.py             # Helper functions
│   │   └── logger.py             # Logging configuration
│   └── config.py                 # Application configuration
├── config/                        # Configuration files
├── data/                         # Sample/reference images
├── generated_cards/              # Generated card outputs
├── logs/                         # Application logs
├── main.py                       # FastAPI application entry point
└── requirements.txt              # Python dependencies
```

## Configuration

### Environment Variables

| Variable                | Description                  | Required |
| ----------------------- | ---------------------------- | -------- |
| `GEMINI_API_KEY`        | Google Generative AI API key | Yes      |
| `CLOUDINARY_CLOUD_NAME` | Cloudinary cloud name        | Yes      |
| `CLOUDINARY_API_KEY`    | Cloudinary API key           | Yes      |
| `CLOUDINARY_API_SECRET` | Cloudinary API secret        | Yes      |

### Application Settings

Key configuration constants in `app/config.py`:

- `MODEL_NAME`: "gemini-2.5-flash-image-preview"
- `PRODUCT_MODEL`: "gemini-2.5-flash"
- `TEMPERATURE`: 1.0
- `PRODUCT_API`: External product API endpoint

## Error Handling

The API implements comprehensive error handling:

- **400**: Bad Request (invalid file types, missing files)
- **404**: Not Found (file not found)
- **500**: Internal Server Error (AI generation failures, processing errors)

Retry logic is implemented for AI API calls with exponential backoff.

## Logging

Application logs are stored in the `logs/` directory with timestamps. Logs include:

- Request processing information
- AI generation status
- Error details and stack traces
- Retry attempts

## Development

### Running in Development Mode

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Testing

The application includes test files and sample data in the `data/` directory for development and testing purposes.

## Deployment

The application is configured for production deployment with:

- CORS middleware for cross-origin requests
- Async request handling
- Background task processing
- Automatic cleanup of temporary files

## License

MIT

## Author

prinom2000
---

For more detailed API testing, visit the interactive documentation at `/docs` when running the application.
