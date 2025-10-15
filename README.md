# Lab Report Analysis API

A FastAPI-based web service for analyzing lab report images using AI. This service accepts lab report images and provides structured medical analysis with key findings, interpretations, and health insights.

## 🚀 Deployment Ready

This project is configured for deployment on Render with Docker containerization.

## Features

- 🖼️ Image upload support (JPG, PNG, BMP, TIFF, WEBP)
- 🔍 AI-powered lab report analysis
- 📊 Structured response with summary, key findings, and interpretation
- 🌐 RESTful API with automatic documentation
- 🧪 Built-in test client and web interface
- ⚡ Async processing for better performance

## Project Structure

```
Lab_analysis/
├── main.py                     # FastAPI application
├── lab_analyzer.py            # Core analysis logic
├── models.py                  # Pydantic models
├── test_client.py             # API test client
├── index.html                 # Web interface
├── requirements.txt           # Dependencies
├── Lab_report_analysis.py     # Original script
└── README.md                  # This file
```

## Installation

1. **Clone or navigate to the project directory:**

   ```bash
   cd "e:\E-JUST Assignments\Projects\HealthCare\Lab_analysis"
   ```

2. **Create a virtual environment (recommended):**

   ```bash
   python -m venv venv
   venv\Scripts\activate  # On Windows
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

## Running the API

### Method 1: Using Python directly

```bash
python main.py
```

### Method 2: Using Uvicorn

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

The API will be available at:

- **API Endpoints**: http://localhost:8000
- **Interactive Docs**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

## API Endpoints

### Health Check

- **GET** `/health`
- Returns service status

### Analyze Lab Report (File Upload)

- **POST** `/analyze`
- Upload an image file for analysis
- Accepts: `multipart/form-data` with `file` field

### Analyze Lab Report (Base64)

- **POST** `/analyze-base64`
- Send base64 encoded image for analysis
- Accepts: JSON with `image` field containing base64 string

## Usage Examples

### Using cURL

1. **Health check:**

   ```bash
   curl http://localhost:8000/health
   ```

2. **Analyze image file:**

   ```bash
   curl -X POST "http://localhost:8000/analyze" \
        -H "accept: application/json" \
        -H "Content-Type: multipart/form-data" \
        -F "file=@your_lab_report.jpg"
   ```

3. **Analyze base64 image:**
   ```bash
   curl -X POST "http://localhost:8000/analyze-base64" \
        -H "Content-Type: application/json" \
        -d '{"image": "your_base64_encoded_image_here"}'
   ```

### Using Python Test Client

```python
from test_client import LabReportAPIClient

client = LabReportAPIClient()

# Health check
health = client.health_check()
print(health)

# Analyze image
result = client.analyze_image_file("path/to/your/lab_report.jpg")
print(result)
```

### Using the Web Interface

1. Start the API server
2. Open `index.html` in your web browser
3. Drag and drop or select a lab report image
4. Click "Analyze Report" to get results

## Response Format

Successful analysis returns:

```json
{
  "success": true,
  "filename": "lab_report.jpg",
  "analysis": {
    "error": false,
    "summary": "Brief summary of the lab report",
    "key_findings": ["Finding 1", "Finding 2", "Finding 3"],
    "interpretation": "Medical interpretation",
    "note": "Disclaimer about medical advice",
    "raw_response": "Complete AI response"
  }
}
```

## Configuration

### Environment Variables

You can set these environment variables to customize the behavior:

- `API_HOST`: Host to bind to (default: "0.0.0.0")
- `API_PORT`: Port to bind to (default: 8000)
- `HF_API_KEY`: Hugging Face API key (currently hardcoded in `lab_analyzer.py`)

### Updating API Key

To use your own Hugging Face API key, modify the `lab_analyzer.py` file:

```python
self.client = InferenceClient(
    provider="nebius",
    api_key="your_api_key_here",  # Replace with your API key
)
```

## Development

### Running in Development Mode

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Testing

Run the test client:

```bash
python test_client.py
```

### Adding New Features

1. Add new endpoints to `main.py`
2. Update models in `models.py` if needed
3. Extend the analyzer in `lab_analyzer.py`
4. Update documentation

## Deployment

### 🐳 Docker Deployment

This project includes a production-ready Dockerfile:

```bash
# Build the Docker image
docker build -t lab-analysis-api .

# Run locally with Docker
docker run -p 8000:8000 -e HUGGINGFACE_API_KEY=your_api_key_here lab-analysis-api
```

### 🚀 Render Deployment

This project is configured for easy deployment on Render:

#### Method 1: Using render.yaml (Recommended)

1. Push your code to GitHub
2. Connect your GitHub repository to Render
3. Render will automatically detect the `render.yaml` file
4. Set the `HUGGINGFACE_API_KEY` environment variable in Render dashboard
5. Deploy!

#### Method 2: Manual Setup

1. Create a new Web Service on Render
2. Connect your GitHub repository
3. Set the following:
   - **Environment**: Docker
   - **Dockerfile Path**: `./Dockerfile`
   - **Docker Context**: `./`
4. Add environment variable:
   - `HUGGINGFACE_API_KEY`: Your Hugging Face API key
5. Deploy

#### Environment Variables for Production

Set these environment variables in your Render dashboard:

- `HUGGINGFACE_API_KEY`: Your Hugging Face API key (required)
- `ALLOWED_ORIGINS`: Comma-separated list of allowed origins (optional, defaults to "\*")
- `PORT`: Port number (automatically set by Render)

### 📋 Production Considerations

- ✅ Docker containerization with security best practices
- ✅ Non-root user execution
- ✅ Health checks configured
- ✅ Environment variable configuration
- ✅ Optimized Docker image with proper caching
- ✅ CORS configuration for production
- ✅ Proper error handling and logging

### 🔍 Health Check

The API includes a health check endpoint at `/health` for monitoring:

```bash
curl https://your-app.onrender.com/health
```

Response:

```json
{
  "status": "healthy",
  "service": "lab-report-analyzer"
}
```

## Troubleshooting

### Common Issues

1. **Import errors**: Make sure all dependencies are installed
2. **Port conflicts**: Change the port in the uvicorn command
3. **API key issues**: Verify your Hugging Face API key is valid
4. **Image format errors**: Ensure images are in supported formats

### Logs

The application logs important events. Check console output for debugging information.

## License

This project is for educational purposes. Please ensure you have proper licenses for any AI models used.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

---

**Note**: This analysis is for educational purposes only and should not replace professional medical advice.
