# 🎬 MultiEmbed Stream Resolver

A lightweight API and web interface for resolving embed pages into final stream URLs. This tool is dependency-light and uses standard Python libraries for HTTP operations.

## ✨ Features

- 🚀 Simple HTTP API server
- 🌐 Beautiful web interface (deployable on GitHub Pages)
- 🔄 Live HTTP resolution and capture file fallback
- 📦 Zero external dependencies for basic functionality
- 🎯 Support for multiple streaming servers
- 🔍 Automatic stream URL extraction

## 🚀 Quick Start

### Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/multiembed.git
   cd multiembed
   ```

2. **Run the API server:**
   ```bash
   python maneby.py --serve --port 8787
   ```

3. **Open the web interface:**
   - Open `index.html` in your browser
   - Or visit `http://localhost:8787/` if you add static file serving

4. **Test the API:**
   ```bash
   curl "http://127.0.0.1:8787/resolve?url=https%3A%2F%2Fmultiembed.mov%2F%3Fvideo_id%3D280%26tmdb%3D1"
   ```

## 🌐 Deploy to Cloud Platforms

### Deploy API Server

#### Option 1: Render

1. Fork this repository
2. Go to [Render Dashboard](https://dashboard.render.com/)
3. Click "New +" → "Web Service"
4. Connect your GitHub repository
5. Configure:
   - **Name:** multiembed-api
   - **Environment:** Python 3
   - **Build Command:** `pip install -r requirements.txt` (if needed)
   - **Start Command:** `python maneby.py --serve --host 0.0.0.0 --port $PORT`
6. Click "Create Web Service"
7. Copy your API URL (e.g., `https://multiembed-api.onrender.com`)

#### Option 2: Railway

1. Fork this repository
2. Go to [Railway](https://railway.app/)
3. Click "New Project" → "Deploy from GitHub repo"
4. Select your forked repository
5. Railway will auto-detect the Procfile
6. Copy your API URL

#### Option 3: Heroku

```bash
heroku create multiembed-api
git push heroku main
```

### Deploy Web Interface (GitHub Pages)

1. **Enable GitHub Pages:**
   - Go to your repository settings
   - Navigate to "Pages" section
   - Source: "GitHub Actions"

2. **Push to GitHub:**
   ```bash
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/multiembed.git
   git push -u origin main
   ```

3. **Access your web interface:**
   - Your site will be available at: `https://YOUR_USERNAME.github.io/multiembed/`
   - Enter your API server URL in the interface

4. **Configure CORS (Important!):**
   
   The API server needs to allow requests from your GitHub Pages domain. The code already includes:
   ```python
   self.send_header("Access-Control-Allow-Origin", "*")
   ```
   
   For production, you should restrict this to your domain:
   ```python
   self.send_header("Access-Control-Allow-Origin", "https://YOUR_USERNAME.github.io")
   ```

## 📖 API Documentation

### Endpoints

#### `GET /health`
Health check endpoint.

**Response:**
```json
{
  "ok": true,
  "service": "web_dev_learing_testing.py"
}
```

#### `GET /resolve`
Resolve an embed URL to stream URLs.

**Parameters:**
- `url` (required): The embed URL to resolve (URL-encoded)
- `live` (optional): Use live HTTP resolution (default: `1`)
- `capture` (optional): Use capture file fallback (default: `1`)
- `server` (optional): Preferred server ID (e.g., `89`, `88`, `90`)

**Example:**
```bash
curl "http://localhost:8787/resolve?url=https%3A%2F%2Fmultiembed.mov%2F%3Fvideo_id%3D280%26tmdb%3D1&live=1&capture=1&server=89"
```

**Response:**
```json
{
  "input_url": "https://multiembed.mov/?video_id=280&tmdb=1",
  "ok": true,
  "status": "ok",
  "play_url": "https://streamingnow.mov/...",
  "play_token": "...",
  "sources": [...],
  "embed_urls": [...],
  "stream_urls": [
    "https://example.com/stream.m3u8"
  ],
  "steps": [...],
  "errors": [],
  "used_capture": false,
  "used_live_http": true
}
```

## 🛠️ CLI Usage

### Basic resolution:
```bash
python maneby.py "https://multiembed.mov/?video_id=280&tmdb=1"
```

### Serve API:
```bash
python maneby.py --serve --host 0.0.0.0 --port 8787
```

### Options:
- `--serve`: Start the JSON API server
- `--host`: Host to bind to (default: `127.0.0.1`)
- `--port`: Port to bind to (default: `8787`)
- `--capture-dir`: Folder containing capture files
- `--no-live`: Skip live HTTP resolution
- `--no-capture`: Skip capture file fallback
- `--server-id`: Preferred server ID
- `--browser`: Use optional Playwright browser collector (requires `playwright` installed)

## 🧪 Optional: Browser Mode

For advanced collection using a real browser:

```bash
pip install playwright
python -m playwright install chromium
python maneby.py --browser "https://multiembed.mov/?video_id=280&tmdb=1"
```

## 📁 Project Structure

```
multiembed/
├── maneby.py           # Main Python API server
├── index.html          # Web interface
├── requirements.txt    # Python dependencies
├── Procfile           # For Heroku/Railway deployment
├── runtime.txt        # Python version
├── README.md          # This file
└── .github/
    └── workflows/
        └── deploy.yml # GitHub Actions workflow
```

## 🔧 Environment Variables

- `PORT`: Server port (default: `8787`)

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is for educational and testing purposes only.

## ⚠️ Disclaimer

This tool is intentionally dependency-light and does not attempt to bypass CAPTCHA, DRM, paywalls, or access controls. It is designed for legitimate testing and development purposes only.

## 🌟 Features Coming Soon

- [ ] Rate limiting
- [ ] Authentication
- [ ] Caching layer
- [ ] More streaming platform support
- [ ] Docker support

## 📧 Support

If you have any questions or issues, please open an issue on GitHub.

---

Made with ❤️ by the community
