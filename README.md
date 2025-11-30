# BlockCert AI - Resume Matching System 🎓

An AI-powered resume screening and candidate matching system that uses NLP and machine learning to match candidate resumes with job descriptions. Built with Flask, sentence-transformers, and scikit-learn.

## 🌟 Features

- **AI-Powered Matching** - Uses sentence transformers to compute semantic similarity between resumes and job descriptions
- **PDF Resume Upload** - Automatic text extraction from PDF resumes
- **Manual Text Input** - Fallback option for image-based PDFs
- **Skill Extraction** - Automatically identifies technical and soft skills from resumes
- **Match Scoring** - Provides percentage-based match scores (0-100%)
- **Skill Gap Analysis** - Identifies missing skills candidates need to acquire
- **Candidate Dashboard** - View, sort, and manage all candidates
- **Resume Viewer** - View uploaded PDF resumes directly in browser
- **Delete Functionality** - Remove candidates with confirmation popup
- **Persistent Storage** - SQLite database for data persistence

## 🚀 Tech Stack

- **Backend**: Python 3.13, Flask 3.1.2
- **ML/NLP**: sentence-transformers 3.3.1, PyTorch 2.5.1, scikit-learn 1.5.2
- **Database**: SQLite with SQLAlchemy 2.0.36
- **Frontend**: Bootstrap 5.3, HTML5, CSS3, JavaScript
- **PDF Processing**: pypdf 5.1.0
- **Deployment**: Render (with Gunicorn 23.0.0)

## 📋 Prerequisites

- Python 3.10+ (Python 3.13 recommended)
- pip or conda package manager
- 2GB+ RAM (for ML models)

## 🛠️ Installation

### Local Development

1. **Clone the repository**
```bash
git clone https://github.com/Thiyanesh07/Blockcert-AI.git
cd Blockcert-AI
```

2. **Create virtual environment**
```bash
# Using conda
conda create -n blockcert python=3.13
conda activate blockcert

# OR using venv
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Run the application**
```bash
python app.py
```

5. **Access the app**
```
Open browser: http://localhost:5000
```

## 🌐 Deployment (Render)

The application is optimized for deployment on Render's free tier.

### Deploy Steps

1. **Push to GitHub**
```bash
git add .
git commit -m "Deploy to Render"
git push origin main
```

2. **Create Web Service on Render**
   - Go to [render.com](https://render.com)
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Render auto-detects `render.yaml` configuration

3. **Environment Variables** (Optional)
   - `SECRET_KEY`: Flask secret key (auto-generated if not set)
   - `DATABASE_URL`: PostgreSQL URL (optional, defaults to SQLite)

4. **Deploy**
   - Click "Create Web Service"
   - Wait 10-15 minutes for initial build (downloads ML models)

### Deployment Configuration

The `render.yaml` file includes optimizations for free tier:
- Single worker with threading (memory efficient)
- Worker recycling to prevent memory leaks
- Model caching
- No-cache pip install to reduce build size

## 📁 Project Structure

```
Blockcert-AI/
├── app.py                  # Main Flask application
├── db.py                   # Database configuration
├── models.py               # SQLAlchemy models
├── requirements.txt        # Python dependencies
├── render.yaml            # Render deployment config
├── Procfile               # Railway deployment config
├── railway.toml           # Railway configuration
├── reset_db.py            # Database reset utility
├── .slugignore            # Files to exclude from deployment
├── instance/              # Database storage (SQLite)
├── static/
│   └── style.css          # Custom CSS styles
├── templates/
│   └── index.html         # Main UI template
└── README.md              # This file
```

## 💾 Database Schema

### Candidates Table
- `id`: Primary key
- `name`: Candidate name
- `email`: Email address (unique identifier)
- `resume_text`: Extracted text from resume
- `skills`: Comma-separated skill list
- `resume_data`: Binary PDF data
- `created_at`: Timestamp

### Jobs Table
- `id`: Primary key
- `title`: Job title (hash-based for uniqueness)
- `description`: Job description text
- `skills_required`: Comma-separated required skills
- `created_at`: Timestamp

### Matches Table
- `id`: Primary key
- `candidate_id`: Foreign key to Candidates
- `job_id`: Foreign key to Jobs
- `match_score`: Percentage match (0-100)
- `missing_skills`: Comma-separated missing skills

## 🎯 Usage

### 1. Upload Resume & Job Description

- **Option A**: Upload PDF resume + paste job description
- **Option B**: Paste resume text + paste job description

### 2. View Results

The system shows:
- Match score percentage (color-coded)
- Candidate skills identified
- Job skills required
- Missing skills gap

### 3. Dashboard

View all candidates ranked by match score:
- Green badge: 70%+ match
- Yellow badge: 60-69% match
- Red badge: <60% match

### 4. Actions

- **View Resume**: Click "View PDF" to open resume in new tab
- **Delete Candidate**: Click "Delete" button (with confirmation)

## 🔧 Configuration

### Skill Vocabulary

Edit `SKILL_VOCAB` in `app.py` to customize skill detection:
```python
SKILL_VOCAB = [
    "python", "java", "javascript", "react", 
    "machine learning", "docker", "aws", ...
]
```

### ML Model

Change the sentence transformer model in `app.py`:
```python
_model = SentenceTransformer("all-MiniLM-L6-v2")  # Current
# Options: paraphrase-MiniLM-L3-v2, all-mpnet-base-v2, etc.
```

### Database

Switch to PostgreSQL for production:
```bash
# Set environment variable
export DATABASE_URL="postgresql://user:password@host:5432/dbname"
```

## 🐛 Troubleshooting

### PDF Extraction Fails
**Issue**: "Your PDF appears to be image-based"
**Solution**: Use manual text input or convert PDF to text-based format

### Memory Issues on Render
**Issue**: 502 errors, out of memory
**Solution**: 
- Already optimized for free tier
- Consider upgrading to paid tier ($7/month)
- Or switch to lighter model (TF-IDF instead of transformers)

### Database Resets on Deploy
**Issue**: Data lost after deployment
**Solution**: Use PostgreSQL instead of SQLite
```bash
# Add PostgreSQL database on Render
# Copy DATABASE_URL to environment variables
```

### Import Errors
**Issue**: `ModuleNotFoundError`
**Solution**: 
```bash
pip install -r requirements.txt
# OR
conda install --file requirements.txt
```

## 📊 Performance

- **First Load**: 5-10 seconds (model loading)
- **Subsequent Requests**: 1-3 seconds
- **PDF Processing**: 2-5 seconds
- **Match Calculation**: <1 second
- **Memory Usage**: ~400-450MB

## 🔐 Security Notes

- Change `SECRET_KEY` in production
- Validate file uploads (max size, type checking)
- Use HTTPS in production
- Sanitize user inputs
- Implement rate limiting for API endpoints

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📝 License

This project is developed by **Team Codezilla** for educational purposes.

## 👥 Team Codezilla

- Lead Developer: [Your Name]
- Contributors: [Team Members]

## 🙏 Acknowledgments

- sentence-transformers library by UKPLab
- Flask framework
- Render platform
- Bootstrap UI framework

## 📞 Support

For issues or questions:
- Open an issue on GitHub
- Check `/health` endpoint for service status
- Review deployment logs on Render dashboard

---

**Made with ❤️ by Team Codezilla**
