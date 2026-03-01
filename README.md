# 🔥 Tahir GPT – Ultimate Edition

A production-ready AI SaaS platform that pushes free-tier infrastructure to its absolute maximum.

## 🚀 Features

- **AI Chat Engine**: Powered by HuggingFace free inference models (Mistral-7B).
- **Live Data Integration**: Injects DuckDuckGo/Wikipedia search results into prompts.
- **File Upload & Analysis**: Extracts text from PDF, DOCX, and TXT files for context.
- **Document Generation**: Generates professional PDF, Word, and PowerPoint files instantly.
- **Image Generation**: Creates images using Pollinations AI (no API key required).
- **Website Generator**: Generates full HTML/CSS/JS websites.
- **Authentication**: Secure JWT-based auth with bcrypt password hashing.
- **Database**: Stores users, chats, and metadata in MongoDB Atlas.

## 🏗 Architecture

This project uses a Full-Stack architecture optimized for the AI Studio environment:
- **Frontend**: React 18+, Vite, TailwindCSS, Zustand, React Router.
- **Backend**: Express.js, Mongoose, Multer, JWT.
- **Note**: While the prompt requested Next.js App Router, the AI Studio environment enforces a Vite-based setup. We have implemented the exact same serverless-compatible modular architecture using Express + Vite middleware to ensure full compatibility and stability within this sandbox.

## 🛠 Setup Instructions

### 1. Environment Variables

Create a `.env` file in the root directory and add the following variables:

```env
# MongoDB Connection String (Free Tier Atlas Cluster)
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/tahirgpt?retryWrites=true&w=majority

# JWT Secret for Authentication
JWT_SECRET=your_super_secret_jwt_key_here

# HuggingFace API Key (Free Inference)
# Get yours at: https://huggingface.co/settings/tokens
HF_API_KEY=hf_your_api_key_here

# Optional: Admin Mode
ADMIN_MODE=false
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Run the Application

```bash
npm run dev
```

This will start the Express server on port 3000, which also serves the Vite frontend via middleware.

## 📦 MongoDB Setup (Free Tier)

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register).
2. Create a free shared cluster (M0).
3. Create a database user and password.
4. Allow access from anywhere (`0.0.0.0/0`) in Network Access.
5. Get your connection string and add it to `MONGO_URI` in `.env`.

## 🤗 HuggingFace Setup

1. Create a free account at [HuggingFace](https://huggingface.co/).
2. Go to Settings > Access Tokens.
3. Create a new token (Read role is sufficient for inference).
4. Add the token to `HF_API_KEY` in `.env`.

## 🚀 Vercel Deployment Guide

To deploy this full-stack application to Vercel:

1. Create a `vercel.json` file in the root directory:
```json
{
  "version": 2,
  "builds": [
    { "src": "server.ts", "use": "@vercel/node" },
    { "src": "package.json", "use": "@vercel/static-build" }
  ],
  "routes": [
    { "src": "/api/(.*)", "dest": "/server.ts" },
    { "src": "/(.*)", "dest": "/dist/$1" }
  ]
}
```
2. Update the `build` script in `package.json` to compile the server if necessary, or let Vercel handle it via `@vercel/node`.
3. Push your code to GitHub.
4. Import the repository in Vercel.
5. Add your Environment Variables (`MONGO_URI`, `JWT_SECRET`, `HF_API_KEY`) in the Vercel dashboard.
6. Deploy!

## 🔄 Self-Updating Architecture

The intelligence of Tahir GPT is kept fresh without expensive model retraining:
1. **Live Search Injection**: When a user asks a question, the backend intercepts it and queries Wikipedia/DuckDuckGo for live data.
2. **Context Assembly**: The live data is injected into the system prompt before sending it to the LLM.
3. **Result**: The model answers using its base knowledge *plus* the real-time data provided in the prompt context.

## 📂 File Handling

- Files are uploaded securely using `multer` with memory storage to avoid disk writes on serverless environments.
- File size is strictly limited to 5MB.
- Text is extracted in-memory using `pdf-parse` (PDF) and `mammoth` (DOCX).
- The extracted text is appended to the chat context.

## ⚡ Performance Optimization

- **In-Memory Processing**: Avoids disk I/O bottlenecks during file uploads.
- **Streaming Simulation**: UI updates optimistically.
- **Modular Services**: AI, Search, and Document generation are decoupled.
- **Lazy Loading**: React Router handles code splitting for pages.

## ⚠️ Limitations

- **Free Tier Limits**: HuggingFace free inference API has rate limits. If exceeded, the API will return errors.
- **MongoDB Storage**: Free tier is limited to 512MB.
- **Stateless Serverless**: In a true serverless deployment (like Vercel), memory storage for files is ephemeral and limited by the function's memory allocation (usually 1024MB).
