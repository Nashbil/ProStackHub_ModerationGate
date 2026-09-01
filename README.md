# ModerationGate

Content moderation layer for a chat app, ProStackHub AI internship (Task 5). Every message
gets checked by Gemini before it's shown, flagged stuff gets logged with a reason, and there's
an admin page to review flags plus a quick test to check the false-positive rate.

## Stack

- Backend: Node/Express, uses Gemini as the moderation classifier (structured json output)
- Frontend: React (Vite), Chat + Admin tabs

## Setup

Backend:
```
cd backend
cp .env.example .env
# add your gemini key
npm install
npm run dev
```

Frontend:
```
cd frontend
cp .env.example .env
npm install
npm run dev
```

Backend on :8789, frontend on :5175 (or next open port if that's taken).

Get a key: https://aistudio.google.com/apikey

## How it works

1. type a message in Chat, it gets sent to the backend
2. backend asks gemini to classify it - flagged or not, category, short reason
3. shows up in the chat as Allowed or Blocked
4. anything flagged gets logged server-side (category + reason + timestamp)
5. Admin tab shows the flagged log, and has a button to clear it
6. Admin tab also has a "run test" button that fires 20 clearly-benign messages through the
   same classifier and reports how many got flagged (should be 0, or close to it)

## Notes

- flagged log is in-memory, resets if the backend restarts. fine for a demo, would swap for a
  real db if this ever needed to be persistent
- if the gemini model gets deprecated (happens a lot lately), change GEMINI_CHAT_MODEL in
  backend/.env
- the classifier prompt is written to specifically NOT flag normal negativity/complaints/casual
  language - that's what the false-positive test in the admin tab is checking
