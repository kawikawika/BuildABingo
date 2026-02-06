# BuildABingo — GitHub Pages + Firebase MVP

This is a client-only MVP that stores game state in Firebase Realtime Database so the app can be hosted as static files (e.g. GitHub Pages).

How it works
- The frontend (React + Vite) is deployed to GitHub Pages.
- Firebase Realtime Database holds games, players, and calls. Clients listen to the database to get realtime updates.

Setup

1. Create a Firebase project
- Go to https://console.firebase.google.com and create a project.
- Add a Web app to the project and copy the firebaseConfig values.

2. Enable Realtime Database
- In the Firebase Console, open "Realtime Database" and create a database.
- For development/testing you can set rules to public:
  {
    "rules": {
      ".read": true,
      ".write": true
    }
  }
  WARNING: This is insecure. For production, set proper rules or use Firebase Auth + Cloud Functions to secure warden actions.

3. Add your firebaseConfig
- Open `src/App.jsx` and replace the firebaseConfig object with the config from step 1.

4. Install and run locally
- npm install
- npm run dev
- Visit http://localhost:5173 (Vite default) to test.

5. Deploy to GitHub Pages
- Commit the repository to GitHub.
- Ensure `homepage` in package.json is set if you host on a project page (optional).
- Run:
  npm run deploy
  This uses gh-pages to publish the `dist` directory to the gh-pages branch which GitHub Pages will serve.

Notes and improvements
- Security: this MVP checks the wardenToken in the browser before making a call but does not enforce server-side verification. A malicious user can modify client code and write to the database. For production:
  - Use Firebase Authentication for wardens and players, and write rules that only allow authenticated wardens to push calls.
  - Or use Cloud Functions as a trusted backend endpoint that validates a server-side warden token and pushes calls to the DB.
- Persistence and export: data is stored in Realtime DB (persisted).
- Scaling: Firebase handles scaling for moderate loads.
- Additional features: automatic bingo detection, unique card-per-player guarantees, nicer UI.
