CMPT 371 A3 Socket Programming Trivia Game
Course: CMPT 371 - Data Communications & Networking
Instructor: Mirza Zaeem Baig
Semester: Spring 2026
RUBRIC NOTE: As per submission guidelines, only one group member will submit the link to this repository on Canvas.

Group Members
Name	        Student ID	Email
Calvin Weng	    301556001	dyw7@sfu.ca
Alexander Jiang	**	john.smith@university.edu

1. Project Overview & Description
This project is a competitive multiplayer Trivia Game built using Python's Socket API (TCP). Multiple clients connect to one central server, receive the same question at the same time, and race to answer. The first player to submit the correct answer wins the point for that round. The server manages all game state (questions, scoring, and leaderboard updates), while clients provide a simple GUI for answering.

Core behavior in this implementation:
- Supports multiple concurrent players (tested design target: at least 6 clients).
- Real-time leaderboard updates after each round.
- Graceful handling when a client disconnects during an active game.
- Short answer cooldown per player after each attempt.
- Hardcoded trivia question bank (10 questions) to keep scope focused on sockets/concurrency.

2. Project Structure

TrivialTrivia/
  backend/
    src/
      __init__.py
      server.py
      client.py
      net.py
      questions.py
  bridge/
    server.js
    package.json
  frontend/
    package.json
    src/

3. System Limitations & Edge Cases
As required by the project specifications, we have identified and handled (or defined) the following limitations and potential issues within our application scope:

Handling Multiple Clients Concurrently:
Solution: We use Python's threading module with one thread per connected client. This allows multiple players to stay connected and submit answers concurrently while the game loop continues to broadcast rounds.
Limitation: Thread-per-client is limited by host resources. A production design would likely use asyncio or a worker/thread pool architecture.

TCP Stream Buffering:
Solution: TCP is a continuous byte stream, so we implemented newline-delimited JSON framing (\n) in both client and server. This ensures each payload is processed as a complete message.

Input Validation & Security:
Solution: The server validates message type, answer format (A/B/C/D), and question ID before awarding points. Score updates are guarded by a lock to avoid race conditions when answers arrive nearly simultaneously.
Limitation: This assignment-level implementation does not include authentication, encryption, or anti-cheat protections against a modified custom client.

Graceful Exit Handling:
Solution: Server-side try/except around each client handler detects disconnects and removes players cleanly from active state. Updated leaderboard/player state is then broadcast to remaining players.

Answer Cooldown:
Solution: After each answer attempt, a player enters a short cooldown period (2 seconds) where additional answers are ignored. This is enforced on the server and mirrored on the GUI client.

4. Video Demo
Our 2-minute video demonstration covering connection establishment, data exchange, real-time gameplay, leaderboard changes, and process termination can be viewed below:
▶️ Watch Project Demo on YouTube

5. Prerequisites (Fresh Environment)


Before running the project, make sure the following are installed:

- Python 3.10 or newer
- Node.js 18 or newer
- npm

You can verify them with:

python --version
node --version
npm --version

Installation Guide

1. Clone the repository and enter the project folder

git clone <your-repo-link>
cd TrivialTrivia

2. Backend setup

The backend uses only standard Python libraries, so no additional installation is required.

3. Bridge setup

Open a terminal and run:

cd /Users/alex/TrivialTrivia/bridge
npm install
npm install express ws cors

The bridge server uses:
- express
- ws
- cors

4. Frontend setup

Open a terminal and run:

cd /Users/alex/TrivialTrivia/frontend
npm install
npm install framer-motion lucide-react canvas-confetti

The frontend uses:
- React
- Vite
- framer-motion
- lucide-react
- canvas-confetti

How to Run the Application

The application requires three separate terminals running at the same time.

Terminal 1: Backend Server

cd /Users/alex/TrivialTrivia/backend
python -m src.server

Expected output:
[STARTING] Server listening on 127.0.0.1:5555

Terminal 2: Bridge Server

cd /Users/alex/TrivialTrivia/bridge
node server.js

Expected output:
Bridge running on http://localhost:8080

Terminal 3: Frontend

cd /Users/alex/TrivialTrivia/frontend
npm run dev

Expected output will include a local development URL, usually:
http://localhost:5173

Open that URL in your browser.

How to Test the Application

1. Start all three terminals in the order shown above
2. Open the frontend in the browser
3. Enter a username and click Join
4. Wait in the lobby
5. Click Start Game
6. Answer questions by clicking A, B, C, or D
7. Verify that the leaderboard updates when a correct answer is submitted
8. Continue until the game ends
9. Verify that the final screen appears and confetti plays
10. Click Back to Home to return to the join screen

Important Notes

- All three terminals must remain running during use
- If the frontend says Disconnected, check that the bridge and backend are still running
- If a newly joined player sees that the match is already in progress, restart the backend server to reset the game state
- Back to Home resets the frontend state, but a full fresh game may still require restarting the backend depending on the current backend state

How to Restart Backend if Needed

Press Ctrl + C in the backend terminal, then run:

python -m src.server

Step 4: Gameplay
The server broadcasts each question and 4 options to all connected clients.
Players answer by clicking one of the GUI buttons (A/B/C/D).
The first correct answer receives the point for that round.
The leaderboard updates in real time after each round.
If a player disconnects mid-game, the server continues running for the remaining players.

6. Technical Protocol Details (JSON over TCP)
We designed a custom application-layer protocol for data exchange using JSON over TCP.

Transport Framing: newline-delimited JSON (\n)
Message Format: {"type": <string>, ...}

Handshake Phase:
Client sends: {"type":"hello","username":"Player1"}
Server responds: {"type":"welcome","username":"Player1"}

Gameplay Phase:
Server broadcasts question:
{"type":"question","question_id":1,"text":"...","options":["...","...","...","..."],"timeout_s":12}

Client sends answer:
{"type":"answer","question_id":1,"value":"A"}

Server broadcasts round result:
{"type":"round_result","question_id":1,"winner":"Player2","correct":"B","scores":{"Player2":1,"Player1":0}}

Server broadcasts leaderboard updates:
{"type":"leaderboard","scores":{"Player2":1,"Player1":0}}

Optional/utility events:
{"type":"player_joined","username":"Player3"}
{"type":"player_left","username":"Player3","reason":"disconnected"}
{"type":"info","message":"Player2 answered correctly!"}

7. Academic Integrity & References

Code Origin:
The socket communication approach follows standard TCP examples that can be found online. 
The core game loop, JSON protocol handling, TCP networking logic, leaderboard logic, and class and method implementations were written by our group.

GenAI Usage:
ChatGPT was used to help structure protocol design, create the frontend react UI, create base skeleton classes and their methods in the backend, and write the README.
Gemini was used for learning about sockets and threading in python, idea brainstorming and accounting for system limitations and edge cases.
Gemini was also used for debugging and helping implement certain functions we were stuck on.

References:

Google Gemini 
Python Socket Programming HOWTO - https://docs.python.org/3/howto/sockets.html 
Real Python: Intro to Python Threading - https://realpython.com/intro-to-python-threading/
Tkinter Documentation - https://realpython.com/ref/stdlib/tkinter/ 