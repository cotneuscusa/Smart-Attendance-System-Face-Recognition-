# Smart Attendance System

A privacy-focused Android application designed to simplify classroom attendance using on-device face recognition. The application helps teachers identify enrolled students, record attendance, and generate reports while keeping biometric processing and student data on the device.

## Features

- **Face Detection:** Detects faces using Google ML Kit.
- **On-Device Face Recognition:** Uses TensorFlow Lite / LiteRT for facial feature extraction and face matching.
- **Student Enrollment:** Registers students using multiple face samples to improve recognition reliability.
- **Automated Attendance:** Recognizes enrolled students during classroom scanning and records attendance.
- **Attendance Management:** Maintains student records and attendance history.
- **Report Generation:** Generates attendance reports in CSV and PDF formats.
- **Encrypted Local Database:** Uses SQLCipher to protect locally stored student and attendance data.
- **Privacy-Focused Design:** Processes facial data on-device without relying on a cloud-based recognition service.
- **Android Application:** Designed for use on Android devices.

## Technology Stack

- **Language:** Kotlin
- **UI:** Jetpack Compose
- **Camera:** CameraX
- **Face Detection:** Google ML Kit Face Detection
- **Face Recognition:** TensorFlow Lite / LiteRT
- **Database:** Room with SQLCipher encryption
- **Reporting:** CSV and PDF export
- **Platform:** Android

## How It Works

1. The teacher registers students and captures multiple facial samples.
2. The application detects faces using the device camera and ML Kit.
3. The face recognition model generates numerical facial representations.
4. The application compares detected faces with enrolled student representations.
5. Recognized students can have their attendance recorded automatically.
6. Teachers can review attendance records and generate CSV or PDF reports.

Recognition reliability depends on factors such as lighting, camera quality, facial angle, and the quality of enrollment samples. Attendance records should be reviewed when recognition is uncertain.

## Privacy and Security

Privacy is an important part of the application's design.

- Facial processing runs locally on the Android device.
- Student and attendance records are stored in a local database.
- SQLCipher provides database encryption.
- The application does not depend on a cloud-based face recognition service.

Biometric information is sensitive. Students should be informed about data collection, enrollment should follow applicable consent requirements, and stored records should be deleted when no longer needed.

## Installation

### Download the APK

A prebuilt APK is available in this repository:

[**Download SmartAttendance.apk**](https://github.com/cotneuscusa/Smart-Attendance-System-Face-Recognition-/blob/main/SmartAttendance.apk)

1. Open the APK link above.
2. Download the APK using GitHub's download option.
3. Transfer the APK to your Android device if necessary.
4. Open the file and follow Android's installation instructions.

Android may require permission to install applications from the source you used to download the APK. Only install applications you trust.

### Build from Source

**Requirements**

- Android Studio
- Android SDK
- A compatible JDK for the project's Gradle configuration
- An Android device or emulator

**Steps**

1. Clone the repository:

   ```bash
   git clone https://github.com/cotneuscusa/Smart-Attendance-System-Face-Recognition-.git
   ```

2. Open the project in Android Studio.
3. Allow Gradle to synchronize and download the required dependencies.
4. Connect an Android device with USB debugging enabled or start an emulator.
5. Run the application using Android Studio.

To build a debug APK on Windows, run the following command from the project root:

```powershell
.\gradlew.bat assembleDebug
```

The generated APK is normally located at:

```text
app\build\outputs\apk\debug\app-debug.apk
```

## Project Goals

This project explores the practical application of mobile computer vision, machine learning, encrypted local storage, and Android development to solve a real-world classroom attendance problem.

It demonstrates the integration of multiple technologies into a single application, with an emphasis on privacy, usability, and reducing the administrative workload associated with manual attendance.

## Limitations

- Recognition performance depends on environmental conditions and enrollment quality.
- Camera permission is required for face scanning.
- Data stored locally may be lost if the device is lost or the application is uninstalled without a suitable backup.
- Face recognition is not infallible, so teachers should be able to correct attendance records when necessary.

## License

No license has been specified for this project. All rights are reserved by default unless a license is added to the repository.
