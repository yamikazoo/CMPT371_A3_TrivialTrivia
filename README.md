CMPT 371 A3 Socket Programming Trivia Game

Course: CMPT 371 - Data Communications and Networking  
Instructor: Mirza Zaeem Baig  
Semester: Spring 2026  

Group Members  
Calvin Weng 301556001  
Alex Jiang 301566792  

------------------------------------------------------------

1. Project Overview

This project is a competitive multiplayer trivia game built using Python TCP sockets. Multiple clients connect to a central server, receive the same question at the same time, and compete to answer first. The first correct response earns a point.

The backend server manages all networking, game logic, scoring, and synchronization. A bridge server allows communication between the TCP backend and a web frontend. The frontend provides a clean interface for joining, answering, and viewing results.

This project follows the assignment requirement to build a socket-based network application using a client server architecture. :contentReference[oaicite:0]{index=0}

Core Features

- Multiple concurrent players supported using threading  
- Real time leaderboard updates  
- Question broadcasting to all clients simultaneously  
- First correct answer wins the round  
- Graceful handling of client disconnects  
- Cooldown system to prevent spam answers  
- Web based UI for improved usability  

------------------------------------------------------------

2. Project Structure

TrivialTrivia/  
  backend/  
    src/  
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

------------------------------------------------------------

3. Video Demo

A full demonstration of connection setup, gameplay, and termination is available here:

https://youtu.be/GDl-JiDYjak

This satisfies the requirement to include a working demo showing connections, data exchange, and termination. :contentReference[oaicite:1]{index=1}

------------------------------------------------------------

4. System Design and Protocol

Transport Protocol: TCP  
Message Format: JSON  
Framing: newline delimited messages  

Example messages

Client connect  
{"type":"connect_tcp","username":"alex"}  

Server question  
{"type":"question","question_id":1,"text":"...","options":["A","B","C","D"]}  

Client answer  
{"type":"answer","question_id":1,"value":"A"}  

Round result  
{"type":"round_result","winner":"alex","correct":"C","scores":{"alex":1}}  

------------------------------------------------------------

5. Limitations and Edge Cases

Handling Multiple Clients  
We use one thread per client. This allows multiple players but is limited by system resources.  

TCP Stream Handling  
Messages are newline delimited JSON to prevent partial reads.  

Input Validation  
Server validates answers and question ids. Invalid inputs are ignored.  

Client Disconnects  
Disconnected users are removed and the game continues.  

Cooldown System  
Players cannot spam answers. A short delay is enforced after each attempt.  

Security  
There is no authentication or encryption. This is acceptable for assignment scope.  

------------------------------------------------------------

6. Prerequisites

This project assumes a fresh environment.

Install the following:

Python 3.10 or newer  
Node.js 18 or newer  
npm  

Verify installation:

python --version  
node --version  
npm --version  

------------------------------------------------------------

7. Installation Guide

Step 1. Clone the repository and enter the folder

git clone <your-repo-link>  
cd TrivialTrivia  

Step 2. Backend setup

No external libraries are required.

Step 3. Bridge setup (create it manually)

If the bridge folder does not exist, create it:

mkdir bridge  
cd bridge  

Initialize Node project:

npm init -y  

Install required packages:

npm install express ws cors  

Create a file called server.js inside the bridge folder and paste the provided bridge server code into it.

Step 4. Frontend setup

cd ../frontend  
npm install  
npm install framer-motion lucide-react canvas-confetti  

------------------------------------------------------------

8. How to Run the Application

You must open three separate terminals.

Terminal 1 Backend

cd TrivialTrivia/backend  
python -m src.server  

Expected output  
Server listening on 127.0.0.1:5555  

Terminal 2 Bridge

cd TrivialTrivia/bridge  
node server.js  

Expected output  
Bridge running on http://localhost:8080  

Terminal 3 Frontend

cd TrivialTrivia/frontend  
npm run dev  

Open your browser and go to  
http://localhost:5173  

------------------------------------------------------------

9. How to Test the Application

1. Start all three terminals  
2. Open the frontend in the browser  
3. Enter a username and join  
4. Wait in the lobby  
5. Start the game  
6. Answer questions using buttons  
7. Verify leaderboard updates  
8. Continue until the game ends  
9. Observe final screen and confetti  
10. Click back to home and restart  

------------------------------------------------------------

10. Important Notes

- All three servers must be running at the same time  
- If disconnected, restart bridge or backend  
- Backend restart resets the game fully  
- Frontend reset does not reset backend state  

------------------------------------------------------------

11. How to Restart the Backend

Press Ctrl C in the backend terminal  

Then run:

python -m src.server  

------------------------------------------------------------

12. Assignment Requirements Mapping

This project satisfies the assignment requirements:

- Functional socket application using TCP  
- Multiple clients and concurrency  
- Data exchange between clients and server  
- Clean modular code structure  
- Complete README with setup instructions  
- Working demo video included  

As required, the README allows a user to run the application from a fresh environment without assistance. :contentReference[oaicite:2]{index=2}  

------------------------------------------------------------

13. Academic Integrity and References

Code Origin

All core backend logic, networking, and game flow were implemented by our group.  

GenAI Usage

ChatGPT  
Used for frontend structure, debugging, and documentation formatting  

Gemini  
Used for understanding sockets, threading, and brainstorming system design  

References

Python Socket Programming HOWTO  
https://docs.python.org/3/howto/sockets.html  

Python Threading Guide  
https://realpython.com/intro-to-python-threading/  

------------------------------------------------------------
