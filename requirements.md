# Project Requirements: SIGNET
**Theme:** AI for Bharat Hackathon
**Project Name:** SIGNET (Real-Time Sign Language Interpretation System)

## 1. Problem Statement
Communication is a fundamental right, yet for the **Deaf and Speech-Impaired (Non-Verbal) community** in India, significant barriers exist. 
- Traditional sign language is not understood by the general public.
- Existing assistive technologies often require expensive hardware (sensory gloves, depth cameras).
- Most solutions are one-way (Sign-to-Text), lacking a feedback loop for a true conversation.

## 2. Proposed Solution
SIGNET is a web-based, AI-powered two-way communication bridge. It uses computer vision to translate Indian Sign Language (ISL) gestures into text and speech in real-time and converts spoken/written replies back to the signer.

## 3. Functional Requirements
The system must perform the following core functions:

### A. Signer Interface (Deaf User)
1.  **Video Capture:** Access the device webcam to capture hand gestures.
2.  **Gesture Recognition:** Detect and classify hand signs into letters (Spelling Mode) or words (Speaking Mode).
3.  **Real-Time Display:** Show the interpreted text on the screen instantly.
4.  **Receive Feedback:** Display text replies sent by the non-signer.

### B. Receiver Interface (Non-Signer)
1.  **Live Transcript:** View the translated text stream from the signer.
2.  **Text-to-Speech (TTS):** Audibly speak the interpreted text.
3.  **Translation:** Convert English output into regional Indian languages (e.g., Hindi, Tamil) using API integration.
4.  **Reply System:** Type messages that are instantly pushed to the Signer’s screen.

### C. Connection & Security
1.  **Handshake Protocol:** Users must be able to create or join a private room using a unique Access Code.
2.  **Session Management:** Data should only sync between users in the same room.

## 4. Non-Functional Requirements
1.  **Low Latency:** Inference and data sync must occur in under 100ms to ensure natural conversation flow.
2.  **Hardware Independence:** Must run on standard CPUs (laptops/smartphones) without requiring dedicated GPUs.
3.  **Scalability:** The architecture must support multiple concurrent chat rooms.
4.  **Privacy:** No video data should be stored on the server; only vector landmarks are processed.

## 5. Scope & Constraints
-   **Current Scope:** Recognition of alphabets (A-Z) and a predefined vocabulary of common words.
-   **Constraint:** Requires stable internet connectivity for the Supabase Realtime socket connection.
-   **Lighting:** Requires moderate lighting for MediaPipe detection accuracy.

## 6. Future Scope
-   **3D Avatar Response:** Integrating a visual avatar to sign back to the deaf user.
-   **Mobile App:** Porting the LSTM model to TensorFlow Lite for offline mobile usage.
-   **Dialect Expansion:** Training on regional ISL variations across different Indian states.