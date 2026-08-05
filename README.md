# SnapClass

Attendance marking in most classrooms is still roll-call by name, or a sheet that gets passed around (and abused for proxy attendance). SnapClass fixes that — students join a class with a QR code / join link, and attendance gets marked using face recognition instead of someone shouting names for 10 minutes.

Built this with Streamlit because I wanted something I could get working end-to-end fast, without building a separate frontend.

## What it does

- Teacher creates a class and gets a join code (as a QR code too, so students can just scan it).
- Student opens the link/scans the QR → gets auto-enrolled into that class.
- When it's time for attendance, student's face is scanned and matched against their registered photo. If it matches, they're marked present.
- Added voice verification on top (using resemblyzer) as an extra check, so someone can't just hold up a photo to the camera and get marked present for a friend.
- Everything (users, classes, attendance records) is stored in Supabase.

## Tech used

- Streamlit for the whole UI
- `face_recognition` + `dlib` for the face matching
- `resemblyzer` + `librosa` for voice-based verification
- `segno` for generating the QR codes
- `bcrypt` for password hashing
- Supabase as the backend/database
- pandas / numpy / scikit-learn for the general data handling

## Folder structure

```
SnapClass/
├── .streamlit/               
├── src/
│   ├── screens/
│   │   ├── home_screen.py     -> login / choose role
│   │   ├── teacher_screen.py  -> teacher dashboard
│   │   └── student_screen.py  -> student dashboard
│   └── components/
│       └── dialog_auto_enroll.py   -> handles the join-code auto-enroll flow
├── app.py
└── requirement.txt
```

## Running it locally

Clone the repo:
```bash
git clone https://github.com/rajchakrawarti999/Snap_Class-Making_attendance_faster_using_AI.git
cd Snap_Class-Making_attendance_faster_using_AI
```

Set up a venv (not required but recommended, `dlib` install can get messy if it clashes with other projects):
```bash
python -m venv venv
source venv/bin/activate      # venv\Scripts\activate on Windows
```

Install the dependencies:
```bash
pip install -r requirement.txt
```

Heads up — `dlib-bin` and `face_recognition` sometimes need `cmake` installed on your system before pip will build them properly. If the install fails, that's usually why.

Add your Supabase keys in `.streamlit/secrets.toml`:
```toml
SUPABASE_URL = "your-supabase-url"
SUPABASE_KEY = "your-supabase-anon-key"
```

Then just run:
```bash
streamlit run app.py
```

It'll open at `localhost:8501`.

## Still to do

- Attendance reports (CSV export at least)
- Better mobile support for the camera capture step
- Support for a teacher running multiple classes/sections
- Low-attendance alerts for students

## Contributing

If you want to add something or fix a bug, feel free to fork it and send a PR. Nothing formal — just open an issue first if it's a bigger change so we're on the same page.

## Author

Raj Chakrawarti — [@rajchakrawarti999](https://github.com/rajchakrawarti999)
