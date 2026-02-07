# Legal Aid System - Testing Guide

## ✅ System Status

### Backend Server
- **URL**: http://localhost:8000
- **API Docs**: http://localhost:8000/docs (FastAPI Swagger UI)
- **Status**: ✅ Running
- **AI Support**: OpenAI GPT-4 → Google Gemini Pro (fallback)

### Frontend Application
- **URL**: http://localhost:3000
- **Status**: ✅ Running
- **Framework**: React with React Router

### Database
- **MongoDB**: ✅ Running on localhost:27017
- **Database Name**: test_database

---

## 🧪 Testing Steps

### 1. Navigation Testing
**Test**: Verify all navigation links work correctly

1. Open http://localhost:3000
2. Click on each navigation link:
   - ✅ **Home** - Should load homepage
   - ✅ **Legal Query** - Should navigate to /query
   - ✅ **Student Portal** - Should navigate to /students
   - ✅ **Documents** - Should navigate to /documents

**Expected Result**: All pages should load without errors

---

### 2. Legal Query Submission (Text Mode)
**Test**: Submit a legal query via text input

1. Go to http://localhost:3000/query
2. Select language (English/Hindi/Telugu)
3. Type a query: "How do I file an FIR for theft?"
4. Click "Get Legal Guidance" button

**Expected Result**: 
- Query should be submitted to backend
- You should be redirected to response page
- AI-generated legal guidance should be displayed

**Backend API Call**: `POST /api/queries`

---

### 3. Voice Input Testing
**Test**: Submit a query using voice input

1. Go to http://localhost:3000/query
2. Click "Voice Input" button
3. Allow microphone access
4. Speak your query clearly
5. Wait for transcription

**Expected Result**:
- Speech should be transcribed using Whisper
- Query text should appear in the text area
- Can submit the query normally

**Backend API Call**: `POST /api/voice-to-text`

---

### 4. Feature Cards on Homepage
**Test**: Click on feature cards and legal categories

1. Go to http://localhost:3000
2. Scroll to "Legal Categories Supported" section
3. Click on any category card (FIR, RTI, Consumer Rights, etc.)

**Expected Result**: Should navigate to /query page

---

### 5. Sample Queries
**Test**: Use sample queries for quick testing

1. Go to http://localhost:3000/query
2. Scroll to "Sample Queries" section
3. Click on any sample query

**Expected Result**: 
- Query text should be populated in the text area
- Can submit immediately

---

### 6. Language Selection
**Test**: Query in different languages

**English Query**:
- Query: "How do I file an FIR for theft?"
- Expected: Response in English

**Hindi Query**:
- Query: "चोरी के लिए एफआईआर कैसे दर्ज करें?"
- Expected: Response in Hindi

**Telugu Query**:
- Query: "దొంగతనం కోసం ఎఫ్‌ఐఆర్ ఎలా దాఖలు చేయాలి?"
- Expected: Response in Telugu

---

### 7. Student Portal
**Test**: Student registration and case management

1. Go to http://localhost:3000/students
2. Click "Register Student"
3. Fill in details (Name, Email, College, Skills)
4. Submit

**Expected Result**:
- Student should be registered
- Should appear in students list
- Can view assigned cases

**Backend API Calls**: 
- `GET /api/students` - List students
- `POST /api/students` - Register student
- `GET /api/cases` - List cases

---

### 8. Document Generation
**Test**: Generate FIR or RTI documents

1. Go to http://localhost:3000/documents
2. Select document type (FIR or RTI)
3. Select language
4. Fill in required details
5. Generate document

**Expected Result**:
- Document should be generated in selected language
- Should display formatted legal document
- Can download or copy

**Backend API Call**: `POST /api/documents`

---

## 🐛 Troubleshooting

### Issue: "Submit query is not working"
**Causes**:
1. Backend not connected
2. CORS error
3. Missing navigate function

**Solutions**:
- ✅ Verify backend is running on port 8000
- ✅ Check browser console for errors (F12)
- ✅ Ensure REACT_APP_BACKEND_URL is set in .env
- ✅ Added `useNavigate()` hook in QueryPage.jsx

---

### Issue: "Navigation links not working"
**Causes**:
1. BrowserRouter not configured
2. Link components not imported

**Solutions**:
- ✅ BrowserRouter wraps the app in App.js
- ✅ All Links use react-router-dom
- ✅ Routes are properly configured

---

### Issue: "Features/Cards not clickable"
**Causes**:
1. Missing Link wrapper
2. onClick handler not defined

**Solutions**:
- ✅ All category cards have Link wrapper to /query
- ✅ CTA button has Link to /query
- ✅ Homepage properly structured

---

### Issue: "Voice input not working"
**Causes**:
1. Microphone permission denied
2. Whisper model not loaded
3. torchaudio not installed

**Solutions**:
- ✅ torchaudio installed
- ✅ Whisper model loaded in backend
- ✅ Browser needs microphone permission

---

### Issue: "API returns 404"
**Check**:
```powershell
# Test backend health
curl http://localhost:8000/api/

# Expected response:
# {"message":"Legal Aid System API is running","status":"healthy"}
```

---

### Issue: "MongoDB connection error"
**Check if MongoDB is running**:
```powershell
Get-Process mongod -ErrorAction SilentlyContinue
```

**Start MongoDB if not running**:
```powershell
Start-Process mongod -WindowStyle Hidden
```

---

## 📊 API Endpoints Reference

### Query Endpoints
- `POST /api/queries` - Submit legal query
- `GET /api/queries` - List all queries
- `GET /api/queries/{query_id}` - Get specific query

### Voice AI Endpoints
- `POST /api/voice-to-text` - Speech-to-text transcription
- `POST /api/text-to-speech` - Text-to-speech synthesis
- `POST /api/voice-query` - End-to-end voice query

### Student Endpoints
- `GET /api/students` - List students
- `POST /api/students` - Register student
- `GET /api/students/{student_id}` - Get student details
- `GET /api/students/{student_id}/assigned-cases` - Get assigned cases

### Case Endpoints
- `GET /api/cases` - List cases
- `POST /api/cases` - Create case
- `PATCH /api/cases/{case_id}` - Update case

### Document Endpoints
- `POST /api/documents` - Generate FIR/RTI document
- `GET /api/documents` - List generated documents

### Utility Endpoints
- `GET /api/` - Health check
- `GET /api/stats` - System statistics
- `POST /api/seed` - Seed sample data

---

## 🔧 Quick Fixes Applied

### 1. Fixed Missing useNavigate Hook
**File**: `src/pages/QueryPage.jsx`
**Fix**: Added `const navigate = useNavigate();`

### 2. Added Missing Imports
**File**: `src/pages/QueryPage.jsx`
**Imports Added**:
- `Volume2` from lucide-react
- `Badge` from @/components/ui/badge

### 3. Added Missing State Variable
**File**: `src/pages/QueryPage.jsx`
**Added**: `const [useVoice, setUseVoice] = useState(false);`

### 4. Configured Environment Variables
**File**: `.env`
**Set**: `REACT_APP_BACKEND_URL=http://localhost:8000`

### 5. Added Gemini API Support
**File**: `server.py`
**Added**: Fallback to Google Gemini when OpenAI fails

---

## ✅ Current Status Summary

| Component | Status | Notes |
|-----------|--------|-------|
| Backend Server | ✅ Running | Port 8000 |
| Frontend App | ✅ Running | Port 3000 |
| MongoDB | ✅ Running | Port 27017 |
| OpenAI API | ✅ Configured | Primary AI |
| Gemini API | ✅ Configured | Fallback AI |
| Navigation | ✅ Fixed | All links working |
| Query Submission | ✅ Fixed | Navigate added |
| Voice Input | ✅ Ready | Whisper loaded |
| Document Generation | ✅ Working | FIR & RTI |

---

## 🎯 Test Scenarios

### Scenario 1: First-time User
1. Visit homepage
2. Read about features
3. Click "Start Speaking Your Query"
4. Submit a query
5. View response

### Scenario 2: Student Registration
1. Navigate to Student Portal
2. Register as a law student
3. View available cases
4. Accept a case assignment

### Scenario 3: Document Generation
1. Navigate to Documents page
2. Select FIR document type
3. Fill in incident details
4. Generate and download document

### Scenario 4: Multilingual Query
1. Submit query in Hindi
2. Get response in Hindi
3. Use TTS to hear response
4. Switch to English and compare

---

## 📝 Notes

- Frontend auto-compiles on file changes
- Backend auto-reloads with uvicorn --reload
- MongoDB stores all queries, students, and cases
- AI responses are cached in the database
- CORS is enabled for all origins (development only)

---

## 🚀 Next Steps

1. **Test each feature thoroughly**
2. **Check browser console for errors**
3. **Monitor backend logs for API calls**
4. **Verify MongoDB data persistence**
5. **Test voice input with microphone**
6. **Generate sample documents**

All systems are ready for testing! 🎉
