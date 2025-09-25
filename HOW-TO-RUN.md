# How to Run CSrankings Locally and Deploy

This guide explains how to set up and run the CSrankings website locally on your computer, as well as deployment options.

## Prerequisites

Before you begin, make sure you have the following installed on your system:

- **Node.js** (version 14 or higher) - [Download here](https://nodejs.org/)
- **TypeScript** - Install globally with `npm install -g typescript`
- **Google Closure Compiler** - Install with `npm install -g google-closure-compiler`
- **Python 3** (3.7 or higher) - For data processing scripts
- **Git** - For version control

### Additional Dependencies (Linux/Ubuntu)

For full data processing capabilities, you'll also need:

```bash
sudo apt-get install libxml2-utils python3-lxml basex
```

For macOS with Homebrew:
```bash
brew install libxml2 basex
pip3 install lxml
```

## Quick Start (Frontend Only)

If you just want to run the website with existing data:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/emeryberger/CSRankings.git
   cd CSRankings
   ```

2. **Build the JavaScript:**
   ```bash
   make csrankings.js
   ```
   Or manually:
   ```bash
   tsc --project tsconfig.json
   ```

3. **Start a local web server:**
   ```bash
   python3 -m http.server 8000
   ```
   Or with Node.js:
   ```bash
   npx http-server
   ```

4. **Open your browser:**
   Navigate to [http://localhost:8000](http://localhost:8000)

## Full Development Setup

For complete development including data processing:

1. **Install all dependencies:**
   ```bash
   npm install -g typescript google-closure-compiler
   pip3 install lxml
   ```

2. **Build everything:**
   ```bash
   make all
   ```

3. **Update DBLP data (optional, requires ~19GB RAM):**
   ```bash
   make update-dblp
   ```

4. **Clean and rebuild data:**
   ```bash
   make clean-csrankings
   ```

## File Structure

- `csrankings.ts` - Main TypeScript source code
- `csrankings.js` - Compiled JavaScript (auto-generated)
- `csrankings-*.csv` - Faculty data files
- `index.html` - Main website page
- `util/` - Data processing scripts
- `Makefile` - Build automation

## Development Workflow

1. **Make changes to TypeScript:**
   Edit `csrankings.ts` for frontend changes

2. **Recompile:**
   ```bash
   tsc --project tsconfig.json
   ```

3. **Test locally:**
   ```bash
   python3 -m http.server 8000
   ```

4. **View changes:**
   Open [http://localhost:8000](http://localhost:8000)

## Top Professors Feature

The Top Professors view shows faculty ranked by publication scores with these columns:
- **#** - Rank number
- **Faculty** - Professor name (linked to homepage when available)  
- **# Pubs** - Raw publication count
- **Adj. #** - Score-weighted publication count
- **Institution** - University affiliation

Toggle between "University View" and "Top Professors" using the buttons at the top.

## Deployment Options

### Option 1: GitHub Pages (Recommended)

CSrankings uses GitHub Pages for hosting:

1. **Fork the repository** on GitHub
2. **Enable GitHub Pages** in repository settings
3. **Push changes** to the `gh-pages` branch
4. **Access your site** at `https://yourusername.github.io/CSrankings`

### Option 2: Vercel Deployment

Vercel provides easy static site hosting:

1. **Install Vercel CLI:**
   ```bash
   npm install -g vercel
   ```

2. **Deploy from project directory:**
   ```bash
   vercel
   ```

3. **Follow the prompts** to link your GitHub account and deploy

4. **Configure build settings** (if needed):
   - Build Command: `make csrankings.js`
   - Output Directory: `./` (root directory)

### Option 3: Traditional Web Server

For Apache/Nginx deployment:

1. **Build the project:**
   ```bash
   make csrankings.js
   ```

2. **Copy files to web root:**
   ```bash
   cp -r * /var/www/html/
   ```

3. **Ensure proper permissions:**
   ```bash
   chmod -R 644 /var/www/html/
   ```

### Option 4: Docker Deployment

Create a `Dockerfile`:

```dockerfile
FROM node:16-alpine

WORKDIR /app
COPY . .

RUN npm install -g typescript google-closure-compiler
RUN make csrankings.js

FROM nginx:alpine
COPY --from=0 /app /usr/share/nginx/html
EXPOSE 80
```

Build and run:
```bash
docker build -t csrankings .
docker run -p 8080:80 csrankings
```

## Troubleshooting

### Common Issues

1. **TypeScript compilation errors:**
   - Ensure TypeScript is installed globally: `npm install -g typescript`
   - Check `tsconfig.json` for proper configuration

2. **Missing data files:**
   - Run `make all` to generate all required files
   - Ensure CSV files are present in the repository

3. **JavaScript not loading:**
   - Check browser console for errors
   - Verify `csrankings.js` was generated properly
   - Clear browser cache

4. **CORS issues in development:**
   - Always use a local HTTP server, never open HTML files directly
   - Use `python3 -m http.server` or similar

### Performance Notes

- The full DBLP update requires significant RAM (~19GB)
- Consider using the pre-built data files for development
- Large datasets may cause browser slowdowns

## Contributing

1. **Fork the repository** on GitHub
2. **Create a feature branch** for your changes
3. **Make your changes** and test locally
4. **Submit a pull request** with a clear description

## Support

- **GitHub Issues:** Report bugs and request features
- **Documentation:** Check the main README and FAQ
- **Community:** Follow [@CSrankings](https://twitter.com/CSrankings) on Twitter

---

For more detailed information, see the main [README.md](README.md) and [FAQ](faq.html).