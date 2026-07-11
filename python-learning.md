## FastAPI architecture

Typical request flow:

HTML
→ script.js
→ api.js
→ FastAPI Route
→ Service
→ Database
→ Service
→ Route
→ api.js
→ script.js
→ HTML

Responsibilities:

- HTML
  Displays the interface.

- script.js
  Reads user input and updates the page.

- api.js
  Sends HTTP requests.

- Route
  Receives requests and forwards them.

- Service
  Contains business logic and SQL.

- Database
  Stores the data.

Debugging rule:

Always identify which layer is failing before changing code.