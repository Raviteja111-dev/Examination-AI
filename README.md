#  Examination AI

> A Smart, Step-by-Step Learning Assistant for Exam Preparation

Examination AI is an intelligent web-based study tool that helps students understand complex concepts quickly and prepare confidently for exams. Powered by Google's Gemini AI, it delivers clear definitions, structured step-by-step solutions, and customizable difficulty levels.

---

## Key Features

###  Smart Question Answering
- **AI-Powered Responses**: Leverages Google Gemini 2.5 Pro for accurate, intelligent answers
- **Step-by-Step Breakdown**: Every answer includes:
  -  **Key Idea** - Concise 1-2 line summary
  -  **Step-by-Step Process** - Detailed numbered steps with examples
  -  **Final Summary** - Quick recap and practical tips

###  Difficulty Levels
Choose how detailed your explanation should be:
- **🟢 Easy** - Simple, beginner-friendly language with analogies
- **🟡 Moderate** - Balanced, student-focused explanations (default)
- **🔴 Advanced** - In-depth technical details with real-world applications

###  Question History
- Automatically saves all previously asked questions
- Quick access to past explanations for rapid revision
- Delete individual history items with one click
- Browse history in the side panel

###  Dark Mode Support
- Toggle between light and dark themes for comfortable reading
- Smooth transitions and eye-friendly design

###  User Authentication
- Secure account creation with email and password
- Password hashing with Werkzeug security
- Session management for personalized history
- Protected routes requiring login

###  Clean, Intuitive UI
- ChatGPT-like interface for familiarity
- Responsive design that works on all modern browsers
- Real-time response streaming with loading indicators
- Mobile-friendly layout

---

##  Tech Stack

### Backend
- **Framework**: Flask 3.1.1 - Lightweight Python web framework
- **Database**: MySQL - Persistent storage for users and chat history
- **AI Integration**: Google Generative AI SDK (Gemini 2.5 Pro)
- **Authentication**: Werkzeug - Password hashing and security
- **Connectors**: 
  - `mysql-connector-python` - MySQL database connection
  - `PyMySQL` - MySQL driver

### Frontend
- **HTML5** - Semantic markup
- **CSS3** - Modern styling with dark mode support
- **JavaScript (Vanilla)** - Dynamic interactions without frameworks
- **API Communication**: Fetch API for real-time backend requests

### Dependencies
```
Flask==3.1.1
Flask-SQLAlchemy==3.1.1
mysql-connector-python==9.4.0
PyMySQL==1.1.1
Werkzeug==3.1.3
google-generativeai==0.8.5
PyMuPDF==1.26.3
```

---

## 📁 Project Structure

```
Examination-AI/
├── app.py                 # Main Flask application with routes
├── requirements.txt       # Python dependencies
├── .env                   # Environment variables (API keys, DB config)
├── config.py             # Configuration file (optional)
├── models.py             # Database models (optional)
├── check_models.py       # Model validation script
├── history.json          # Sample chat history data
│
├── templates/            # HTML templates
│   ├── index.html        # Main chat interface
│   ├── login.html        # Login page
│   └── register.html     # Registration page
│
└── static/               # Frontend assets
    ├── style.css         # CSS styling
    └── script.js         # JavaScript (inline in HTML for now)
```

---

##  Getting Started

### Prerequisites
- Python 3.8+
- MySQL Server running locally
- Google API key for Gemini AI

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Raviteja111-dev/Examination-AI.git
   cd Examination-AI
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up the database**
   - Create a MySQL database named `exam_ai`
   - Create the required tables:
   ```sql
   CREATE DATABASE exam_ai;
   USE exam_ai;
   
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       username VARCHAR(255) UNIQUE NOT NULL,
       email VARCHAR(255) UNIQUE NOT NULL,
       password VARCHAR(255) NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   
   CREATE TABLE history (
       id INT AUTO_INCREMENT PRIMARY KEY,
       user_id INT NOT NULL,
       question TEXT NOT NULL,
       answer LONGTEXT NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
       FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
   );
   ```

5. **Configure environment variables**
   
   Create a `.env` file in the root directory:
   ```
   DATABASE_URL=mysql+pymysql://exam_user:Strong_Password_123@localhost:3306/exam_ai
   GOOGLE_API_KEY=your_google_gemini_api_key_here
   UPLOAD_FOLDER=uploads
   FLASK_SECRET_KEY=your_secret_key_here
   ```

   Or update the hardcoded values in `app.py`:
   - Line 10-15: Update MySQL connection details
   - Line 19: Add your Google Gemini API key

6. **Run the application**
   ```bash
   python app.py
   ```
   The app will be available at `http://localhost:5000`

---

## 📖 Usage Guide

### Creating an Account
1. Click "Register" on the login page
2. Enter username, email, and password
3. Password is securely hashed before storage
4. Redirect to login after successful registration

### Asking Questions
1. Log in with your credentials
2. Type your exam question in the text area
3. Select difficulty level:
   - 🟢 **Easy** - Simple, jargon-free explanations
   - 🟡 **Moderate** - Balanced, student-focused
   - 🔴 **Advanced** - Technical depth and rigor
4. Get your answer instantly with structured breakdowns

### Using History
- **View**: Click any question in the sidebar to view/edit
- **Delete**: Click ❌ to remove from history
- **Search**: Use the search feature to find past questions
- **Revision**: Quickly review before exams

### Logout
- Click  **Logout** to end your session securely

---

## 🔄 API Routes

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/` | Main chat interface (requires login) |
| GET | `/login` | Login page |
| POST | `/login` | Handle login submission |
| GET | `/register` | Registration page |
| POST | `/register` | Handle user registration |
| GET | `/logout` | Clear session and logout |
| POST | `/ask` | Submit question and get AI response |
| GET | `/history` | Retrieve user's chat history |
| DELETE | `/delete/<id>` | Delete specific history item |

---

## 🎯 How It Works

### Question Processing Flow
1. User submits question with selected difficulty level
2. Backend builds a detailed prompt with formatting instructions
3. Prompt is sent to Google Gemini 2.5 Pro AI
4. AI generates HTML-formatted response with structured sections
5. Response is saved to MySQL database with user ID and timestamp
6. HTML is rendered in browser for clean presentation
7. History is updated in sidebar for quick access

### Response Structure
All AI responses follow this standardized format:
```html
<h2>How to Understand: [Question]</h2>
<h3>🔑 Key Idea (1–2 lines)</h3>
<p>Concise overview...</p>

<h3>Step-by-Step Process</h3>
<ol>
  <li><strong>Step 1</strong><br/>Description...</li>
  <li><strong>Step 2</strong><br/>Description...</li>
</ol>

<h3>🌟 Final Summary</h3>
<p>Quick recap...</p>
```

---

## 🔒 Security Features

- **Password Hashing**: Werkzeug's `generate_password_hash()` for secure storage
- **Session Management**: Flask sessions prevent unauthorized access
- **SQL Injection Prevention**: Parameterized queries in all database operations
- **Input Validation**: Question and user input sanitization
- **Protected Routes**: Authentication checks on all sensitive endpoints

---

##  Use Cases

✅ **School & College Exams**
- Science, Math, History, Literature concepts
- Quick concept clarification before tests

✅ **Competitive Exams**
- JEE, NEET, CAT preparation
- Complex topic breakdown and practice

✅ **Language Learning**
- Grammar rules explanation
- Concept understanding

✅ **Professional Certifications**
- AWS, Docker, coding interviews
- Technical depth when needed

✅ **Quick Revision**
- Browse past questions
- Last-minute concept review

---

##  Troubleshooting

### Database Connection Error
- Ensure MySQL server is running
- Check credentials in `app.py` (lines 10-15)
- Verify database `exam_ai` exists

### Google API Key Issues
- Obtain key from [Google AI Studio](https://aistudio.google.com/app/apikey)
- Add to `.env` or update line 19 in `app.py`
- Ensure API quota is available

### Login Issues
- Clear browser cookies
- Ensure user credentials are correct
- Check MySQL `users` table has entries

---

##  Roadmap

- [ ] PDF upload and question extraction
- [ ] Multi-language support
- [ ] Code snippet highlighting
- [ ] LaTeX math rendering
- [ ] User profile and settings
- [ ] Spaced repetition for revision
- [ ] Performance analytics
- [ ] Mobile app (React Native)

---

##  Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/YourFeature`)
3. Commit changes (`git commit -m 'Add YourFeature'`)
4. Push to branch (`git push origin feature/YourFeature`)
5. Open a Pull Request

---

## 📄 License

This project is open source and available under the MIT License.

---

##  Support & Contact

For questions, issues, or suggestions:
- **GitHub Issues**: [Open an issue](https://github.com/Raviteja111-dev/Examination-AI/issues)
- **Email**: Your email here
- **Twitter**: [@YourHandle](https://twitter.com/yourhandle)

---

##  Show Your Support

If this project helps you, please give it a star! ⭐

Made with ❤️ by [Raviteja111-dev](https://github.com/Raviteja111-dev)
