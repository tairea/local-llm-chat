# Local LLM Chat

A terminal-style web interface for chatting with multiple LLM models simultaneously via FlexLLama.

## Features

- **Dual Model Responses**: Query two LLM models side-by-side and compare their responses
- **Terminal Aesthetic**: Retro black terminal with green prompt and blinking caret
- **Real-time Streaming**: Watch responses stream in character-by-character
- **Color-Coded Responses**:
  - GPT-OSS-20B: Cyan/Aqua
  - Qwen3-4B: Magenta
- **Mobile Responsive**: Works on desktop and mobile devices

## Architecture

This web app connects to a FlexLLama instance via Tailscale:

```
User Browser → Hetzner Server (ai.sunriselabs.io)
                     ↓
              Tailscale VPN
                     ↓
         Local Machine (100.118.106.2:8090)
                     ↓
              FlexLLama API
                     ↓
         LLM Models (GPT-OSS-20B, Qwen3-4B)
```

## Configuration

The app is configured to connect to FlexLLama at:
- **Tailscale IP**: `100.118.106.2:8090`
- **Endpoint**: `/v1/chat/completions`

To change the API endpoint, edit the `API_URL` constant in `index.html`:

```javascript
const API_URL = 'http://100.118.106.2:8090/v1/chat/completions';
```

## Deployment

### Prerequisites

1. **FlexLLama** running on local machine with Tailscale
2. **Hetzner server** with:
   - Nginx installed
   - Tailscale configured
   - SSL certificate (Let's Encrypt)
3. **DNS**: A record for `ai.sunriselabs.io` pointing to Hetzner server IP

### Deployment Steps

1. **Clone repository on Hetzner**:
   ```bash
   cd /var/www
   git clone https://github.com/yourusername/local-llm-chat.git
   ```

2. **Configure Nginx**:
   ```nginx
   server {
       listen 80;
       server_name ai.sunriselabs.io;

       root /var/www/local-llm-chat;
       index index.html;

       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```

3. **Set up SSL with Certbot**:
   ```bash
   certbot --nginx -d ai.sunriselabs.io
   ```

4. **Ensure Tailscale is running**:
   ```bash
   tailscale status
   ```

## Local Development

To run locally:

1. Serve the HTML file with any web server:
   ```bash
   python -m http.server 3000
   ```

2. Open browser to `http://localhost:3000`

3. Make sure FlexLLama is accessible at the configured Tailscale IP

## Models

Currently configured models:
- **gpt-oss-20b-mxfp4**: GPT-OSS 20B model
- **qwen3-4b-instruct-q4_k_m**: Qwen3 4B Instruct model

## License

MIT
