# System Design & Architecture: SIGNET

## 1. High-Level Architecture
SIGNET follows a **Client-Server-Database** architecture optimized for low-latency real-time communication.

-   **Frontend:** Handles video capture, visualization, and user interaction.
-   **AI Engine (Backend):** Processes video frames and performs inference.
-   **Real-Time Layer:** Synchronizes state between Signer and Receiver via WebSockets.

## 2. Technology Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Frontend** | HTML5, CSS3, JavaScript | Responsive UI for Signer/Receiver interfaces. |
| **Computer Vision** | Google MediaPipe | Extraction of 21 3D hand landmarks (x, y, z). |
| **AI Model** | PyTorch (LSTM + ANN) | Sequence detection and classification. |
| **Backend API** | Python (Flask) | Serves the AI model and handles HTTP requests. |
| **Database** | Supabase (PostgreSQL) | Stores access codes and chat logs. |
| **Real-Time Sync** | Supabase Realtime | WebSocket wrapper for instant data push. |
| **External APIs** | Web Speech API, MyMemory API | Text-to-Speech and Language Translation. |

## 3. Data Flow Pipeline

1.  **Input:** Webcam captures video frames at 30 FPS.
2.  **Preprocessing:** -   Frames are sent to **MediaPipe Hands**.
    -   21 skeletal landmarks are extracted.
    -   Coordinates are normalized relative to the wrist.
3.  **Inference:**
    -   **ANN (Artificial Neural Network):** Used for static signs (Alphabets).
    -   **LSTM (Long Short-Term Memory):** Used for dynamic gestures (Words/Sentences).
4.  **Output Generation:** The model returns the predicted class (Letter/Word).
5.  **Synchronization:**
    -   Prediction is sent to the Frontend.
    -   Frontend pushes the text to **Supabase**.
    -   Supabase broadcasts the update to the connected Receiver.

## 4. AI Model Optimization Strategy
*Why we chose Vector-based over 3D CNNs:*

* **Challenge:** Initial experiments using 3D CNNs on raw video pixels resulted in high latency (>1.5s) and required heavy GPU compute.
* **Optimization:** We switched to a **Landmark-based approach**.
    * **Data Reduction:** Instead of processing `224x224x3` pixels, we process a vector of `21x3` coordinates.
    * **Impact:** Reduced model size significantly and enabled real-time inference on standard CPUs.

## 5. Database Schema (Supabase)

### Table: `access_codes`
| Column | Type | Description |
| :--- | :--- | :--- |
| `id` | uuid | Primary Key |
| `code` | string | Unique room code (e.g., "1234") |
| `created_at` | timestamp | Session start time |

### Table: `conversation_logs`
| Column | Type | Description |
| :--- | :--- | :--- |
| `id` | uuid | Primary Key |
| `room_code` | string | Foreign Key to access_codes |
| `sender_type` | string | "Signer" or "Receiver" |
| `message` | text | The translated text |
| `timestamp` | timestamp | Time of message |

## 6. Security & Privacy
* **Video Processing:** All video processing happens in RAM; video feeds are never saved to the database.
* **Room Isolation:** Messages are filtered by `room_code`, ensuring conversations remain private between the two paired devices.