# SIGNET: AI-Powered Sign Language Detector

Hey there, welcome to SIGNET!

I built this project because I wanted to find a tech-driven way to bridge the communication gap between the deaf and hearing communities. The concept is pretty straightforward: using just a standard webcam, SIGNET picks up on hand signs in real-time and translates them into readable text and spoken audio.

### How I Built It

I wanted this to be fast and accessible, which meant avoiding heavy, laggy image processing that requires a massive GPU. Instead, the application captures video frames and runs them through Google MediaPipe to map out 21 3D hand landmarks. 

Once those skeletal coordinates are extracted, they get passed into a custom PyTorch model I trained. For static letters and basic alphabets, an Artificial Neural Network does the heavy lifting. For dynamic words and full sentences that involve motion, I implemented an LSTM network to track the sequence of gestures. By working with coordinate vectors instead of raw pixels, the whole system runs incredibly smoothly even on a basic laptop CPU.

### The Real-Time Experience

The application is split into two main views to replicate a real conversation. The "Signer" interface captures the webcam feed and runs the AI inference. As soon as a sign is recognized, the translation is instantly broadcasted over to a connected "Receiver" interface using Supabase Realtime WebSockets. 

It essentially acts like a private, low-latency chat room. The signer gets to communicate naturally, and the receiver gets the translated text right on their screen. I also integrated the Web Speech API and MyMemory API to add text-to-speech and language translation, making the conversation even more seamless. And for privacy, video feeds are never saved anywhere, all processing happens locally in RAM, and messages are locked behind unique session access codes.

### Getting Started

If you want to run this on your own machine, make sure you have Python installed. You can install all the necessary dependencies from the requirements file. After that, you just need to start the Flask backend and open the frontend HTML files in your browser. Generate a room code, pair the signer and receiver interfaces, and you are good to go.

I had a lot of fun putting this together, especially the challenge of optimizing the neural networks for real-time performance. Feel free to dig through the code!
