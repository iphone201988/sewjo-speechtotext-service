# Speech to Text Microservice

A FastAPI-based microservice that converts audio files from URLs into text transcripts in multiple subtitle formats (SRT, VTT, JSON, RAW).

## Features

- 🎵 **Audio URL Support**: Fetches audio files from any publicly accessible URL
- 📝 **Multiple Formats**: Supports SRT, VTT, JSON, and RAW subtitle formats
- 🚀 **FastAPI**: Built with FastAPI for high performance and automatic API documentation
- 🔄 **Automatic Cleanup**: Temporarily stores files and cleans them up after processing
- 📚 **OpenAPI Documentation**: Interactive API documentation available at `/docs`

## Prerequisites

- Python 3.9+
- `autosub3` command-line tool installed and available in PATH
  - Install via: `pip install autosub3`

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd sewjo-speechtotext-service
```

2. Install Python dependencies:
```bash
pip install fastapi uvicorn requests
```

3. Ensure `autosub` is installed:
```bash
pip install autosub3
```

4. Create the temporary directory (if it doesn't exist):
```bash
mkdir -p tmp
```

## Running the Service

Start the FastAPI server using uvicorn:

```bash
uvicorn src.app:app --reload --host 0.0.0.0 --port 8000
```

The service will be available at:
- API: `http://localhost:8000`
- Interactive Docs: `http://localhost:8000/docs`
- OpenAPI Schema: `http://localhost:8000/openapi.json`

## API Documentation

### POST `/transcribe`

Transcribes audio from a URL into text in the specified format.

#### Query Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `url` | string | Yes | - | Publicly accessible URL of the audio file to transcribe |
| `subtitle_format` | string | No | `vtt` | Output format: `srt`, `vtt`, `json`, or `raw` |

#### Example Requests

**Using cURL:**
```bash
# VTT format (default)
curl -X POST "http://localhost:8000/transcribe?url=https://example.com/audio.mp3"

# SRT format
curl -X POST "http://localhost:8000/transcribe?url=https://example.com/audio.mp3&subtitle_format=srt"

# JSON format
curl -X POST "http://localhost:8000/transcribe?url=https://example.com/audio.mp3&subtitle_format=json"

# RAW format
curl -X POST "http://localhost:8000/transcribe?url=https://example.com/audio.mp3&subtitle_format=raw"
```

**Using Python:**
```python
import requests

response = requests.post(
  "http://localhost:8000/transcribe",
  params={
    "url": "https://example.com/audio.mp3",
    "subtitle_format": "vtt"
  }
)
print(response.text)
```

**Using JavaScript/Fetch:**
```javascript
const response = await fetch(
  'http://localhost:8000/transcribe?url=https://example.com/audio.mp3&subtitle_format=vtt',
  { method: 'POST' }
);
const transcript = await response.text();
console.log(transcript);
```

#### Response

- **Content-Type**: 
  - `text/plain` for SRT, VTT, and RAW formats
  - `application/json` for JSON format
- **Status Codes**:
  - `200 OK`: Successful transcription
  - `500 Internal Server Error`: Failed to fetch audio or transcribe

#### Response Examples

**VTT Format:**
```
WEBVTT

00:00:00.000 --> 00:00:05.000
Hello, this is a sample transcript.

00:00:05.000 --> 00:00:10.000
This is the second line of the transcript.
```

**SRT Format:**
```
1
00:00:00,000 --> 00:00:05,000
Hello, this is a sample transcript.

2
00:00:05,000 --> 00:00:10,000
This is the second line of the transcript.
```

## Supported Audio Formats

The service supports any audio format that `autosub` can process, including:
- MP3
- WAV
- M4A
- OGG
- FLAC
- And other formats supported by autosub

## Project Structure

```
sewjo-speechtotext-service/
├── src/
│   └── app.py          # Main FastAPI application
├── tmp/                # Temporary directory for audio files (created automatically)
├── README.md           # This file
├── LICENSE             # License file
└── .gitignore          # Git ignore rules
```

## Code Review Notes

### Current Implementation

The service uses:
- FastAPI for the web framework
- `requests` library for fetching audio files
- `autosub` command-line tool for transcription
- Temporary file storage in `tmp/` directory

## License

See the [LICENSE](LICENSE) file for details.
