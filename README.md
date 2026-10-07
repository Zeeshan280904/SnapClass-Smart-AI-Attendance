# SnapClass — AI Smart Attendance System

SnapClass is an AI-powered attendance management system that automates classroom attendance using **Face Recognition** and **Voice Recognition**.

It provides separate **Student and Teacher dashboards**, allowing teachers to create subjects, manage students, conduct attendance, and monitor attendance records.

---

## Features

### Student

- Face-based login
- Voice-based authentication
- Join classes using QR codes or join codes
- Student dashboard

### Teacher

- Teacher registration and login
- Create and manage subjects
- Generate QR codes for classes
- Face and voice attendance
- View students and attendance records

### AI

- Face detection and recognition
- 128-dimensional face embeddings
- SVC-based student classification
- Euclidean-distance verification
- Voice embeddings and speaker recognition

---

## Tech Stack

| Technology              | Usage                                  |
| ----------------------- | -------------------------------------- |
| Python                  | Main programming language              |
| Streamlit               | Web application                        |
| Supabase                | PostgreSQL database & backend services |
| Dlib                    | Face detection and face embeddings     |
| face_recognition_models | Pre-trained Dlib models                |
| Scikit-learn            | SVC classifier                         |
| NumPy                   | Numerical operations                   |
| Pillow                  | Image processing                       |
| Segno                   | QR code generation                     |
| Librosa                 | Audio processing                       |
| Resemblyzer             | Voice recognition                      |
| Bcrypt                  | Password hashing                       |

---

# Face Recognition

SnapClass uses Dlib to generate a **128-dimensional face embedding** for each student.

```text
Image
  ↓
Dlib Face Detection
  ↓
Facial Landmark Detection
  ↓
128-D Face Embedding
  ↓
SVC Classifier
  ↓
Predicted Student
  ↓
Euclidean Distance Verification
  ↓
Recognized / Rejected
```

### Dlib + Pre-trained Models

**Dlib** provides the face-processing algorithms, while `face_recognition_models` provides the pre-trained model files used by Dlib.

- Face detector → finds the face
- Shape predictor → finds facial landmarks
- Face recognition model → generates the 128-D embedding

### SVC

SVC is used to classify the face embedding into a registered student's ID.

It was chosen because it works well with **high-dimensional features and relatively small datasets**.

An additional Euclidean-distance check with a threshold of **0.6** is used to verify the prediction.

---

# Voice Recognition

SnapClass uses **Librosa** and **Resemblyzer** for speaker recognition.

```text
Recorded Audio
      ↓
Librosa
(load & preprocess)
      ↓
Resemblyzer
      ↓
Voice Embedding
      ↓
Similarity Comparison
      ↓
Threshold Check
      ↓
Speaker Identified
```

- **Librosa** → loads and preprocesses audio.
- **Resemblyzer** → converts voice into a speaker embedding.
- **NumPy dot product** → calculates similarity between embeddings.
- **0.65 threshold** → determines whether the speaker is recognized.

---

# Database & Backend

SnapClass uses **Supabase's PostgreSQL database** to store:

- Student and teacher information
- Subjects
- Attendance records
- Face embeddings
- Voice embeddings
- Student-subject relationships

Supabase also provides APIs, allowing the Streamlit application to interact with the database without requiring a separate Flask/FastAPI backend.

---

# QR Code Class Joining

Teachers can generate a unique URL for each subject:

```python
join_url = f"{app_domain}/?join-code={subject_code}"
```

Segno converts this URL into a QR code:

```python
qr = segno.make(join_url)
```

`io.BytesIO()` is used to store the generated QR image **in memory** without creating a permanent file.

---

# Streamlit Session State

SnapClass uses `st.session_state` to maintain information between Streamlit reruns.

It is used for things such as:

- Login status
- Current user
- User role
- Student information
- Selected subject

`@st.cache_resource` is used to cache expensive resources such as the Dlib and Resemblyzer models.

---

# Security

- Passwords are hashed using **bcrypt**.
- Database credentials should be stored using environment variables/secrets.
- Biometric embeddings should be protected.
- Supabase Row Level Security should be configured appropriately for production.

---

# Project Workflow

### Face Attendance

```text
Capture Image
     ↓
Generate Face Embedding
     ↓
SVC Prediction
     ↓
Distance Verification
     ↓
Mark Attendance
```

### Voice Attendance

```text
Record Voice
     ↓
Generate Voice Embedding
     ↓
Compare with Stored Embeddings
     ↓
Threshold Check
     ↓
Mark Attendance
```

---

# Installation

```bash
git clone <YOUR_REPOSITORY_URL>
cd SnapClass
pip install -r requirements.txt
streamlit run app.py
```

Configure your Supabase credentials using environment variables/secrets before running the application.

---

# Future Improvements

- Dedicated attendance session IDs instead of relying on timestamps
- Face liveness/anti-spoofing
- Better recognition under poor lighting and noisy environments
- Improved database security
- Scalability for larger classrooms

---

## Skills Demonstrated

**Python • Streamlit • Machine Learning • Computer Vision • Face Recognition • Voice Recognition • SVC • NumPy • PostgreSQL • Supabase • Authentication • QR Codes • Audio Processing • Git/GitHub**
