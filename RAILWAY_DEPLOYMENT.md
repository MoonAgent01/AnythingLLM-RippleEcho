# Deploy AnythingLLM to Railway

This guide will help you deploy AnythingLLM to Railway.

## Prerequisites
- A Railway account (sign up at https://railway.app)
- Railway CLI installed (optional, for CLI deployment)

## Method 1: Deploy via Railway Dashboard (Recommended)

### Step 1: Create a New Project
1. Go to https://railway.app/new
2. Click "Deploy from GitHub repo"
3. Connect your GitHub account if not already connected
4. Select the `AnythingLLM-RippleEcho` repository

### Step 2: Configure Environment Variables
Add the following environment variables in Railway dashboard:

**Required Variables:**
```env
SERVER_PORT=3001
STORAGE_DIR=/app/server/storage
UID=1000
GID=1000
```

**Security Variables (IMPORTANT - Generate random strings):**
```env
SIG_KEY=<generate-random-32-char-string>
SIG_SALT=<generate-random-32-char-string>
JWT_SECRET=<generate-random-12-char-string>
```

**Optional - LLM Provider (Choose one):**

For OpenAI:
```env
LLM_PROVIDER=openai
OPEN_AI_KEY=your-openai-api-key
OPEN_MODEL_PREF=gpt-4o
```

For Anthropic Claude:
```env
LLM_PROVIDER=anthropic
ANTHROPIC_API_KEY=your-anthropic-key
ANTHROPIC_MODEL_PREF=claude-3-5-sonnet-20241022
```

For Ollama (self-hosted):
```env
LLM_PROVIDER=ollama
OLLAMA_BASE_PATH=http://your-ollama-server:11434
OLLAMA_MODEL_PREF=llama2
```

### Step 3: Configure Volume (Persistent Storage)
1. Go to your Railway project settings
2. Click on "Volumes" tab
3. Add a new volume:
   - Mount Path: `/app/server/storage`
   - Size: 10GB (or more based on your needs)

### Step 4: Deploy
1. Click "Deploy"
2. Wait for the build to complete (this may take 5-10 minutes)
3. Once deployed, Railway will provide you with a URL

### Step 5: Access Your Application
- Your app will be available at: `https://your-project-name.up.railway.app`
- The first time you access it, you'll be prompted to set up an admin account

---

## Method 2: Deploy via Railway CLI

### Step 1: Install Railway CLI
```bash
# Windows (PowerShell)
npm install -g @railway/cli

# Or using Scoop
scoop install railway
```

### Step 2: Login to Railway
```bash
railway login
```

### Step 3: Initialize Project
```bash
cd "H:\Desktop work\Anything LLM\anything-llm"
railway init
```

### Step 4: Link to Your Repository
```bash
railway link
```

### Step 5: Set Environment Variables
```bash
railway variables set SERVER_PORT=3001
railway variables set STORAGE_DIR=/app/server/storage
railway variables set UID=1000
railway variables set GID=1000
railway variables set SIG_KEY=your-random-32-char-string
railway variables set SIG_SALT=your-random-32-char-string
railway variables set JWT_SECRET=your-random-12-char-string

# Add your LLM provider
railway variables set LLM_PROVIDER=openai
railway variables set OPEN_AI_KEY=your-openai-key
```

### Step 6: Deploy
```bash
railway up
```

---

## Important Configuration Notes

### 1. Generate Secure Keys
Generate random strings for security variables:
```bash
# In PowerShell
-join ((48..57) + (65..90) + (97..122) | Get-Random -Count 32 | ForEach-Object {[char]$_})
```

### 2. Embedding Provider
By default, AnythingLLM uses a local embedding model. For Railway, you might want to use an external provider:

```env
EMBEDDING_ENGINE=openai
EMBEDDING_BASE_PATH=https://api.openai.com/v1
EMBEDDING_MODEL_PREF=text-embedding-3-small
```

### 3. Vector Database
Default is LanceDB (file-based). For production, consider:

**Pinecone:**
```env
VECTOR_DB=pinecone
PINECONE_API_KEY=your-pinecone-key
PINECONE_INDEX=anythingllm
```

**Chroma (hosted):**
```env
VECTOR_DB=chroma
CHROMA_ENDPOINT=http://your-chroma-server:8000
```

### 4. Authentication
Enable password protection:
```env
AUTH_TOKEN=true
```

### 5. Storage Considerations
- Railway provides ephemeral storage by default
- **IMPORTANT:** Configure a volume for persistent storage
- Path: `/app/server/storage`
- Recommended size: 10GB minimum

---

## Post-Deployment Steps

1. **Access the application** at your Railway URL
2. **Create admin account** on first login
3. **Configure your LLM** in the settings if not done via environment variables
4. **Set up workspaces** and start using AnythingLLM

---

## Troubleshooting

### Build Fails
- Check Railway build logs
- Ensure Dockerfile path is correct: `docker/Dockerfile`
- Verify all required environment variables are set

### Application Won't Start
- Check deployment logs in Railway dashboard
- Verify `SERVER_PORT=3001` is set
- Ensure volume is properly mounted

### Can't Access After Deployment
- Make sure Railway has generated a public URL
- Check if the service is running in Railway dashboard
- Verify health check is passing

### Data Loss After Restart
- Ensure volume is mounted at `/app/server/storage`
- Check volume is properly configured in Railway settings

---

## Cost Optimization

Railway charges based on:
- **Compute time** (when your app is running)
- **Memory usage**
- **Storage** (volumes)

Tips:
1. Use Railway's free tier for testing
2. Configure auto-scaling limits
3. Monitor resource usage in Railway dashboard
4. Consider using external vector databases to reduce storage costs

---

## Updating Your Deployment

To update your Railway deployment with new changes:

```bash
# Push changes to GitHub
git add .
git commit -m "Update AnythingLLM"
git push origin master

# Railway will automatically redeploy
```

Or manually trigger redeploy in Railway dashboard.

---

## Support & Resources

- Railway Documentation: https://docs.railway.app
- AnythingLLM Documentation: https://docs.anythingllm.com
- Railway Discord: https://discord.gg/railway
- AnythingLLM Discord: https://discord.gg/6UyHPeGZAC

---

## Security Best Practices

1. ✅ Always use strong, random keys for `SIG_KEY`, `SIG_SALT`, and `JWT_SECRET`
2. ✅ Enable `AUTH_TOKEN=true` for password protection
3. ✅ Keep API keys secure and never commit them to Git
4. ✅ Use Railway's secrets management for sensitive data
5. ✅ Regularly update to the latest version
6. ✅ Monitor access logs and usage

---

Happy deploying! 🚀
