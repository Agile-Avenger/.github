# AI-Powered Early Disease Detection

An application that utilizes machine learning to analyze medical imaging (e.g., X-rays) and detect early signs of diseases (such as tuberculosis or cancer) for remote or underdeveloped areas with limited healthcare resources.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Architecture](#architecture)
- [Frontend Workflow](#frontend-workflow)
- [Backend Workflow](#backend-workflow)
- [Model Training](#model-training)
- [MLOps Pipeline](#mlops-pipeline)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Firebase Authentication](#firebase-authentication)
- [License](#license)

---

## Overview

This application is designed to assist healthcare providers in underdeveloped areas by analyzing medical images for signs of diseases. The machine learning model processes images and provides predictions, enabling early disease detection, which can be crucial for patient outcomes. By bridging the gap in healthcare accessibility, this application empowers users to make informed decisions about their health.

## Features

- **User Authentication**: Google OAuth and Email/Password-based authentication for secure access.
- **Image Upload**: Allows users to upload X-ray or ultrasound images in various formats (e.g., JPG, PNG).
- **Disease Detection**: Utilizes advanced ML algorithms to predict potential diseases based on uploaded images.
- **Confidence Scores**: Provides confidence levels for predictions to assist in clinical decision-making.
- **Report Generation**: Generates a downloadable PDF report of the predictions, including detailed analyses.
- **Accessibility**: Optimized for low-bandwidth environments and designed for easy navigation by users with limited technical skills.

---

## Installation

### Prerequisites

- **Flutter SDK**: Required for the frontend.
- **Python 3.8+**: Needed for the backend services.
- **MongoDB**: Database for storing user data and prediction results.
- **Docker**: For creating containerized applications and managing the MLOps pipeline.
- **Google Cloud SDK**: Optional, for deployment and managing cloud resources.
- **Firebase Project** (for authentication)

### Setup

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/early-disease-detection.git
   cd early-disease-detection
   ```

2. **Frontend (Flutter Setup)**:
   - **Install Flutter SDK**: Download and install from [flutter.dev](https://flutter.dev/docs/get-started/install).
   - **Set up IDE**: Install the Flutter and Dart plugins in your preferred IDE (e.g., Visual Studio Code or Android Studio).
   - **Configure Device**: Set up an emulator or connect a physical device for testing.
   - **Run Flutter Packages**:
     ```bash
     cd frontend
     flutter pub get
     ```

3. **Backend**:
   ```bash
   cd backend
   pip install -r requirements.txt
   ```

4. **Environment Variables**:
   - Create a `.env` file in both `frontend` and `backend` directories with necessary environment variables:
     - **Frontend**:
       ```plaintext
       GOOGLE_CLIENT_ID=your_google_client_id
       ```
     - **Backend**:
       ```plaintext
       MONGO_URI=your_mongodb_uri
       GOOGLE_CLIENT_ID=your_google_client_id
       SECRET_KEY=your_secret_key
       ```

5. **Run the Application**:
   - **Backend**:
     ```bash
     python app.py
     ```
   - **Frontend**:
     ```bash
     flutter run
     ```

6. **MLOps Pipeline** :
   - For Docker and MLOps setup, refer to the **MLOps Pipeline** section below.

---


## Firebase Authentication

To set up Firebase Authentication for your application, follow these steps:

### Step 1: Create a Firebase Project

1. Go to the [Firebase Console](https://console.firebase.google.com/).
2. Click on **Add project**.
3. Enter your project name and follow the prompts to create the project.

### Step 2: Enable Authentication Methods

1. In your Firebase project, navigate to **Build** > **Authentication** > **Sign-in method**.
2. Enable **Email/Password** and **Google** authentication:
   - For **Email/Password**:
     - Click on the **Email/Password** option and toggle the switch to enable it.
   - For **Google**:
     - Click on the **Google** option and enable it.
     - Make sure to configure the required OAuth consent screen and provide the necessary details.

### Step 3: Obtain Firebase Configuration

1. In the Firebase Console, navigate to **Project settings** (gear icon).
2. Scroll down to **Your apps** and select **Web**.
3. Register your app and copy the Firebase configuration object:
   ```javascript
   const firebaseConfig = {
       apiKey: "your_api_key",
       authDomain: "your_auth_domain",
       projectId: "your_project_id",
       storageBucket: "your_storage_bucket",
       messagingSenderId: "your_messaging_sender_id",
       appId: "your_app_id",
   };
   ```

### Step 4: Add Firebase to Your Flutter Project

1. Add the `firebase_auth` and `firebase_core` packages to your `pubspec.yaml`:
   ```yaml
   dependencies:
     firebase_auth: ^4.0.0
     firebase_core: ^2.0.0
   ```
2. Initialize Firebase in your `lib/main.dart`:
   ```dart
   import 'package:firebase_core/firebase_core.dart';

   void main() async {
       WidgetsFlutterBinding.ensureInitialized();
       await Firebase.initializeApp();
       runApp(MyApp());
   }
   ```

### Step 5: Implement Authentication

1. **Email/Password Sign-In**:
   - Use the `FirebaseAuth` instance to register and sign in users:
   ```dart
   FirebaseAuth.instance.createUserWithEmailAndPassword(
       email: 'user@example.com',
       password: 'password123',
   );
   ```
   
   ```dart
   FirebaseAuth.instance.signInWithEmailAndPassword(
       email: 'user@example.com',
       password: 'password123',
   );
   ```

2. **Google Sign-In**:
   - Use the `google_sign_in` package along with Firebase Auth:
   ```dart
   final GoogleSignIn googleSignIn = GoogleSignIn();
   final GoogleSignInAccount? googleUser = await googleSignIn.signIn();
   final GoogleSignInAuthentication googleAuth = await googleUser!.authentication;

   final AuthCredential credential = GoogleAuthProvider.credential(
       accessToken: googleAuth.accessToken,
       idToken: googleAuth.idToken,
   );

   await FirebaseAuth.instance.signInWithCredential(credential);
   ```

This setup allows your application to authenticate users via Firebase using either Email/Password or Google login.

---
## Usage

1. **Login**: Access the app through Google OAuth or email/password.
2. **Upload Image**: Go to the dashboard and upload a medical image in supported formats.
3. **View Predictions**: Wait for the model to analyze the image and display results in real time.
4. **Generate Report**: Download a PDF report with the prediction details, including images and confidence scores.

---

## Architecture

### Frontend
- **Framework**: Flutter
- **Components**:
  - **Landing Page**: Introduction to the app with options to log in or explore features.
  - **Authentication System**: Google OAuth and email/password login.
  - **Dashboard**: Main interface to upload images, view predictions, and download reports.
  - **Feedback Form**: Collects user feedback and suggestions for app improvement.
  
### Backend
- **Framework**: Flask
- **Components**:
  - **Authentication**: Manages user sessions and permissions securely.
  - **Model Prediction API**: Receives images, processes them, and returns predictions in a structured format.
  - **Database**: MongoDB for storing user data, predictions, and feedback.
  - **Report Generation API**: Generates downloadable PDF reports using libraries like `reportlab` or `fpdf`.

---

## Frontend Workflow

### Landing Page

- **Overview**: Introduction to the app's purpose and benefits.
- **Login Options**: Users can choose Google OAuth or email/password for secure access.
- **User Guidance**: Step-by-step instructions on using the app for first-time users.

### Authentication System

1. **Google OAuth**: Securely redirects users to Google for login, utilizing JWT for session management.
2. **Email/Password**: Verifies user credentials against MongoDB, ensuring secure data handling.

### Dashboard

1. **Upload Image**: Users select an X-ray or ultrasound image for analysis.
2. **Prediction Results**:
   - Displays the detected disease condition.
   - Shows confidence level in a user-friendly format.
   - Provides actionable recommendations based on the analysis.
3. **Report Generation**:
   - Allows users to generate a PDF report containing prediction details, including uploaded images and insights.
   - Offers options to download or email the report directly from the app.

---

## Backend Workflow

1. **Receive Image**: The backend accepts an uploaded image from the frontend.
2. **Image Processing**: The backend preprocesses the image (e.g., resizing, normalization) before sending it to the model.
3. **Model Inference**: The trained model runs predictions using the processed image data.
4. **Return Results**: The backend sends back prediction details to the frontend, formatted in JSON.
5. **Report Generation**: Creates a PDF report summarizing the prediction results, which can be sent back to the user.

---

## Model Training

### Tools
- **TensorFlow : Frameworks for model training, depending on the architecture chosen.
- **Medical Datasets**: Utilizes publicly available and medically validated datasets (e.g., NIH Chest X-ray dataset) for training and evaluation.

### Steps

1. **Data Preprocessing**:
   - Data cleaning, augmentation, and normalization.
   - Splitting the dataset into training, validation, and test sets for unbiased evaluation.
2. **Model Training**:
   - Train using convolutional neural networks (CNNs) or other advanced models suitable for image classification.
   - Optimize using techniques like transfer learning for better accuracy with less data.
3. **Model Evaluation**:
   - Conduct accuracy checks and validation on unseen data to ensure generalization.
   - Use metrics like precision, recall, and F1-score for comprehensive performance evaluation.
4. **Model Deployment**:
   - Deploying the model to Google Cloud using a containerized Docker environment for scalability.

---

## MLOps Pipeline

### Tools
- **Docker**: Containerizes the application for consistent deployment across environments.
- **Google Cloud**: Hosts and scales the application as needed.
- **CI/CD Tools**: Automates deployment and updates, ensuring seamless integration and delivery.

### Process

1. **Build Docker Image**:
   - Create a Docker image of the model and backend using Dockerfiles for easy deployment.
   ```bash
   docker build -t disease-detection-backend .
   ```

2. **Push to Registry**:
   - Push the Docker image to Google Container Registry or Docker Hub for cloud-based deployment.
   ```bash
   docker push gcr.io/project-id/disease-detection-backend
   ```

3. **Deploy to Cloud**:
   - Utilize Google Cloud Run to deploy and manage instances, enabling auto-scaling based on demand.

4. **Monitor and Scale**:
   - Use Google Cloud Monitoring or Prometheus to observe model performance and scale resources as needed for optimal operation.

---

## Troubleshooting

1. **Google Authentication Issues**:
   - Ensure Google OAuth credentials are correctly configured in the `.env` file and Google Cloud console.
2. **Model Predictions Not Displaying**:
   - Check if the model service is running correctly and if the Docker images are up-to-date.
3. **PDF Report Generation Issues**:
   - Verify that the PDF generation library dependencies are correctly installed in the backend environment.
4. **Database Connection Issues**:
   - Check MongoDB connection strings and ensure the database server is accessible.

---

## Contributing

Contributions are welcome! Please fork the repository, create a new branch for your feature or bug fix, and submit a pull request. Make sure to follow the coding standards and add appropriate tests.

---

## License

This project is licensed under the MIT License.

---

