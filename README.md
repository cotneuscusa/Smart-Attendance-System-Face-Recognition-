Smart Attendance

Smart Attendance is an Android application for managing classroom attendance using on-device face detection and face recognition. Teachers can register students, create class sessions, record attendance with the camera, review or correct records, and export attendance reports to CSV or PDF.

The application uses Kotlin, Jetpack Compose, CameraX, Google ML Kit Face Detection, a bundled MobileFaceNet model, and a locally stored Room database configured with SQLCipher. Face processing is designed to run on the device without sending camera frames to a remote recognition service.

Features

Student registration
- Create student profiles using a name and student ID.
- Capture multiple face samples for each student (the registration workflow is designed around three samples).
- Use ML Kit Face Detection to locate a face before embedding extraction.
- Generate numerical face embeddings for later matching rather than relying on names or IDs to identify a face during live attendance.

Live attendance
- Create an attendance session for a class or course.
- Analyze camera frames with CameraX and detect faces in the live preview.
- Generate face embeddings and compare them with registered embeddings using cosine similarity.
- Record a recognized student as present in the active session and prevent duplicate records within that session.
- Treat a face that does not meet the configured matching threshold as unknown.
- 
Attendance records and reports
- Browse registered students and created sessions.
- Review attendance by session or date.
- Correct attendance records manually when needed.
- Export attendance data to CSV for spreadsheet applications.
- Generate PDF reports and share exported files through Android's share interface.
- 
Local storage and privacy
- Store student, session, embedding, and attendance data locally.
- Encrypt the Room database using SQLCipher.
- Use Android Keystore-backed key management as implemented in the project.
- Process face images on-device; temporary registration images are intended to be removed after feature extraction.
- 
Face embeddings are sensitive biometric data, even when raw photographs are not retained. Use test data or obtain explicit authorization and consent before recording anyone. Do not publish the app database, student details, face data, or private attendance reports.
How face recognition works
1. A teacher registers a student and captures face samples.
2. ML Kit detects the face in a sample.
3. The preprocessing component prepares the detected face for the bundled MobileFaceNet model.
4. The embedding component produces a 192-value feature vector and applies L2 normalization as implemented by the project.
5. During a session, the camera pipeline repeats face detection and embedding extraction for faces in the preview.
6. FaceMatcher compares the new embedding with stored embeddings using cosine similarity. The current configured threshold is 0.45f.
7. A match that satisfies the configured comparison can be recorded as present for the active session. Faces that do not satisfy it should be treated as unknown.

Recognition quality can vary with lighting, camera placement, pose, occlusion, camera quality, preprocessing, and threshold configuration. The threshold is not a guarantee of accuracy. Test the app in the intended environment and review records before relying on them for official attendance.
Technology stack
Area	Technology
Language and UI	Kotlin, Jetpack Compose, Material 3
Camera	CameraX (camera-camera2, camera-lifecycle, camera-view)
Face detection	Google ML Kit Face Detection
Face embeddings	MobileFaceNet with LiteRT / TensorFlow Lite
Local persistence	Room, AndroidX SQLite, SQLCipher
Key management	Android Keystore
Report generation	Android PdfDocument API and a CSV exporter
File sharing	Android FileProvider and the system share interface
Build system	Gradle wrapper; dependency versions are managed in the project's version catalog where configured


Requirements
- Android Studio compatible with the Android Gradle Plugin and Kotlin versions configured in this repository.
- A compatible JDK selected in Android Studio's Gradle JDK setting.
- Android SDK and the SDK components requested by Gradle sync.
- An Android phone running Android 6.0 (API 23) or later, or a compatible emulator.
- Camera permission for registration and live attendance.
- For command-line installation, Android SDK Platform-Tools (which provides adb).

A physical phone is recommended for evaluating the camera flow and face recognition under realistic conditions. Emulator camera support and recognition behavior may differ from a physical device.

Get the source code
1. Clone or download the repository.
2. Open the project root—the folder containing settings.gradle.kts and gradlew.bat—in Android Studio.
3. Allow Gradle sync to finish. Resolve any SDK or JDK prompt shown by Android Studio.
4. Do not copy another developer's local.properties file into the project. Android Studio generates this file for the local SDK installation.

If you already have the project on your computer, open the existing smart_attendance project folder instead of creating a new Android project.
Build and run from Android Studio
1. Connect an Android phone with USB debugging enabled, or start an emulator.
2. Unlock the phone and accept the USB debugging authorization prompt if it appears.
3. In Android Studio, select the app run configuration and the intended device.
4. Click Run. Accept the camera and other runtime permissions requested by the app.
5. Test student registration, live attendance, record review, and report export before preparing a submission.
Build a debug APK from the terminal
Run these commands from the project root.

Windows PowerShell:
.\gradlew.bat clean assembleDebug

macOS or Linux:
./gradlew clean assembleDebug

A successful build should report BUILD SUCCESSFUL. The debug APK is written to:
app/build/outputs/apk/debug/app-debug.apk

Install the APK on an Android phone
Option A: install with Android Debug Bridge (ADB)
First, check that ADB can see the connected device:
adb devices
The phone should appear with the status device. If the list is empty, check the USB cable, select a USB data connection mode if required, enable Developer options → USB debugging, and reconnect the phone. If its status is unauthorized, unlock the phone and accept the authorization prompt.

Install or update the APK from the project root:
adb install -r .\app\build\outputs\apk\debug\app-debug.apk
If PowerShell reports that adb is not recognized and Android SDK Platform-Tools is installed in the default SDK location, run it using the full path:
$adb = "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe"
& $adb devices
& $adb install -r ".\app\build\outputs\apk\debug\app-debug.apk"
If your Android SDK is installed in a different location, replace the path with the actual location of adb.exe. You can find the SDK location under Android Studio → Settings → Languages & Frameworks → Android SDK.

Option B: install directly on the phone
1. Copy app/build/outputs/apk/debug/app-debug.apk to the phone using USB, cloud storage, or another trusted transfer method.
2. Open the APK on the phone and approve installation when Android prompts you. You may need to allow your file manager to install unknown apps.
3. Open Smart Attendance and grant the permissions required for its camera workflow.
Only install APKs from a source you trust. The debug APK is for development and evaluation; if the course or release process specifically requires a signed release APK, create and verify a release build separately.

Troubleshooting
Gradle sync or build fails
- Read the first relevant error in Android Studio's Build output; later errors may be consequences of the first one.
- Confirm that Android Studio is using a JDK compatible with the project's Gradle and Android Gradle Plugin versions.
- Open Tools → SDK Manager and install the Android SDK components requested by the build.
- Check your internet connection if Gradle cannot download dependencies, then sync and try the build again.

PowerShell says adb is not recognized
This usually means Android SDK Platform-Tools is installed but its folder is not in Windows' PATH. If you use the default SDK location, run ADB through its full path from the project root:
$adb = "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe"
& $adb devices
& $adb install -r ".\app\build\outputs\apk\debug\app-debug.apk"

If this path does not exist, find the Android SDK location in Android Studio → Settings → Languages & Frameworks → Android SDK, then update the command to point to that SDK's platform-tools\adb.exe. To make adb available as a normal command, add the platform-tools folder to your user PATH and open a new terminal.

No device appears in adb devices
- Confirm the USB cable supports data, not just charging, and try another USB port if necessary.
- Unlock the phone and select a USB data-transfer mode if the phone offers one.
- Enable Developer options → USB debugging and reconnect the device.
- If using an emulator, start it in Android Studio's Device Manager.

The device status is unauthorized
Unlock the phone and accept the USB debugging authorization prompt. If the prompt does not appear, disconnect and reconnect the phone. You can also revoke USB debugging authorizations in the phone's Developer options and reconnect to trigger a new prompt.

Installation reports a signature conflict
The installed version may have been signed with a different key from the APK you are installing. Uninstalling the existing app can resolve the conflict, but may permanently delete local student profiles, embeddings, sessions, and attendance records. Do not uninstall until you have confirmed that any data you need can be safely retained.

The camera preview does not appear
- Open Android Settings, find Smart Attendance, and confirm that camera permission is allowed.
- Close other apps that might be using the camera, then reopen the registration or live-attendance screen.
- Try restarting the app or testing on a physical device if the emulator camera is unavailable or misconfigured.

A face is not recognized
- Use clear, even lighting and position the face fully within the camera preview.
- Check that the camera is focused and that the student was registered successfully with usable face samples.
- Try again with the face looking toward the camera and without objects covering key facial features.
- The app may reject a match when similarity is below its configured threshold. A rejected match is not proof that the person is absent; review the session and correct the record when appropriate.

CSV or PDF export fails
- Check the destination selected in the Android share or save interface and respond to any system prompts.
- Try another available destination to determine whether the issue is specific to a file manager or sharing app.
- If the problem continues, reproduce it and inspect the app's messages in Android Studio Logcat to identify the underlying error.


Project structure
The main application package is com.example.smartattendance.
com.example.smartattendance/
├── MainActivity.kt
├── data/
│   ├── AttendanceDatabase
│   ├── StudentEntity
│   ├── FaceEmbeddingEntity
│   ├── AttendanceSessionEntity
│   ├── AttendanceRecordEntity
│   ├── repositories
│   └── EmbeddingCodec
├── export/
│   ├── CsvExporter
│   └── PdfExporter
├── ml/
│   ├── FaceEmbedder
│   ├── FacePreprocessor
│   └── FaceMatcher
└── security/
    └── DatabaseKeyManager
The UI also includes screens for registration, students, session creation, live attendance, session details, and attendance reports. This overview describes the main responsibilities; confirm exact filenames in the current source tree because implementation details can change.
