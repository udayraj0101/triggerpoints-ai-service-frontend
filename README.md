# TriggerPoints AI (Frontend)

React + Vite frontend for the TriggerPoints AI chatbot application.

## Features

- 🤖 AI-powered chatbot interface
- 📱 Responsive design for mobile and desktop
- 🔄 Real-time streaming responses
- 🎨 Clean, modern UI

## Prerequisites

- Node.js 18+
- npm or yarn

## Local Development Setup

### 1. Clone and Install Dependencies

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/triggerpoints-ai-service-frontend.git
cd triggerpoints-ai-service-frontend

# Install dependencies
npm install
```

### 2. Environment Configuration

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Edit `.env` with your API configuration:

```env
# Local development (backend running on localhost:8000)
VITE_API_URL=http://localhost:8000
VITE_API_KEY=my-api-key
```

### 3. Run the Development Server

```bash
npm run dev
```

The app will be available at: `http://localhost:5173`

Make sure your backend is running at `http://localhost:8000`.

## Production Deployment

### Option 1: Vercel (Recommended)

1. Push code to GitHub
2. Go to [Vercel](https://vercel.com) and sign up
3. Click "Add New..." → "Project"
4. Import your GitHub repository
5. Configure:
   - Framework Preset: Vite
   - Build Command: `npm run build`
   - Output Directory: `dist`
6. Add Environment Variables:
   - `VITE_API_URL` = your production backend URL
   - `VITE_API_KEY` = your API key
7. Deploy!

### Option 2: Netlify

1. Push code to GitHub
2. Go to [Netlify](https://netlify.com)
3. "Add new site" → "Import an existing project"
4. Connect GitHub and select repository
5. Configure:
   - Base directory: `triggerpoints-ai-service-frontend`
   - Build command: `npm run build`
   - Publish directory: `dist`
6. Add Environment Variables:
   - `VITE_API_URL` = your production backend URL
   - `VITE_API_KEY` = your API key
7. Deploy!

### Option 3: GitHub Pages

1. Update `vite.config.js` with your GitHub repo name:

```javascript
export default defineConfig({
  base: '/triggerpoints-ai-service-frontend/',
  // ... rest of config
})
```

2. Add to `package.json`:

```json
{
  "scripts": {
    "deploy": "npm run build && gh-pages -d dist"
  }
}
```

3. Install gh-pages and deploy:

```bash
npm install -D gh-pages
npm run deploy
```

### Option 4: Static Hosting (Any Provider)

Build the static files:

```bash
npm run build
```

Upload the `dist` folder to any static hosting provider (AWS S3, Google Cloud Storage, etc.)

## API Configuration for Production

### Backend URL Setup

When deploying to production, you need to update the frontend to point to your deployed backend:

1. **Option A: Environment Variable**
   - Set `VITE_API_URL` to your production backend URL
   - Example: `VITE_API_URL=https://your-backend.onrender.com`

2. **Option B: Build-time Configuration**
   Edit `src/App.jsx` and change the API base URL:

```javascript
const API_BASE_URL = 'https://your-production-backend-url.com';
```

### CORS Configuration

If you encounter CORS issues in production, make sure your backend allows requests from your frontend domain:

```python
# In app/main.py, update CORS middleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://your-frontend.vercel.app"],  # Your production domain
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## Architecture

```
┌─────────────────┐      API Requests       ┌─────────────────┐
│                 │  ───────────────────→  │                 │
│   React + Vite │                         │   FastAPI       │
│   Frontend     │  ←───────────────────  │   Backend       │
│                 │     JSON Response      │                 │
└─────────────────┘                         └─────────────────┘
                                                       │
                                                       ↓
                                               ┌─────────────────┐
                                               │   Gemini AI     │
                                               │   + Redis       │
                                               │   + FAISS       │
                                               └─────────────────┘
```

## Troubleshooting

### CORS Errors

If you see CORS errors in the console:

1. Check that your backend CORS allows your frontend domain
2. Verify `VITE_API_URL` is set correctly in `.env`

### API Connection Failed

1. Verify backend is running
2. Check API key matches in both frontend and backend
3. Ensure backend URL doesn't have trailing slash

### Port Already in Use

```bash
# Find process using port 5173
netstat -ano | findstr :5173

# Or use a different port
npm run dev -- --port 3000
```

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build |
| `npm run lint` | Run ESLint |

## License

MIT