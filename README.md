# HeritageCare — Traditional Healthcare CSP

A React + Vite + Node.js/Express e-commerce-style Community Service Project website.

## Features
- E-commerce-style home page, search and categories
- Product cards for ingredients documented in the CSP report
- Learn panels explaining what the CSP documents and what it does not specify
- Shopping cart with quantity controls
- Demo checkout
- Express backend API for products and demo orders
- CSP journey and project information
- Responsive mobile/desktop design

## Run
1. Open this folder in VS Code.
2. Open Terminal.
3. Run `npm install`.
4. Run `npm run dev`.
5. Open `http://localhost:5173/`.

The frontend runs on port 5173 and the backend API on port 5000.

## Important
The product content is based on the Traditional Healthcare CSP report. The website intentionally does not invent medical dosages or guarantee cures. It presents traditional/community uses as educational information and includes safety notes.


## Ask AI feature
Select any product to open its details. Use **Ask AI** to ask product-specific questions such as what it is, traditional uses, how the CSP describes it, and precautions.

#
## Groq Ask AI setup

The Ask AI feature uses the Groq API from the Node.js backend. The API key is never sent to the browser.

1. Create a Groq API key from the Groq Console.
2. Open `.env`.
3. Set `GROQ_API_KEY=your_key_here`.
4. Keep `GROQ_MODEL=openai/gpt-oss-20b`.
5. Run `npm install`, then `npm run dev`.
6. Open the Vite URL shown in the terminal.

The project uses Groq's OpenAI-compatible Chat Completions endpoint. If no key is configured, the Ask AI request will show a backend configuration error rather than exposing a key in the frontend.


## Login and sign-up backend

- The app includes email/password sign-up and sign-in screens. Browsing remains public; checkout requires an account.
- The Express backend exposes `POST /api/auth/signup`, `POST /api/auth/login`, and `GET /api/auth/me`.
- Passwords are salted and hashed with Node.js `scrypt`; raw passwords are never saved. Sessions use signed bearer tokens with a 7-day expiry.
- User accounts and orders are stored in MySQL. Passwords are salted and hashed with Node.js `scrypt`; raw passwords are never saved. Sessions use signed bearer tokens with a 7-day expiry.

### Configure and run

1. Install Node.js 20.19+ (or a compatible current LTS).
2. Copy `.env.example` to `.env`. Add your own Groq key if you use the AI assistant, and set `AUTH_SECRET` to a long random secret.
3. Run `npm install`.
4. Run `npm run dev`.
5. Open `http://localhost:5173`. The API runs at `http://localhost:5000`.

**Security note:** Do not deploy with the sample `AUTH_SECRET`. For a real public deployment, use HTTPS, a managed database, rate limiting, and a secret stored in the hosting provider's environment settings. Never share your `.env` file or API keys.


## MySQL setup (required for login, sign-up and orders)

1. Install and start MySQL Server. You can use MySQL Workbench or the MySQL command-line client.
2. Open `database/schema.sql` in MySQL Workbench and execute the entire script. It creates the `heritagecare` database plus `users` and `orders` tables.
3. Copy `.env.example` to `.env` in the project root. Set `DB_USER` and `DB_PASSWORD` to your local MySQL credentials. Set `AUTH_SECRET` to a long random private value.
4. In the project terminal run `npm install`, then `npm run dev`. The backend checks the MySQL connection on startup and exits with a clear error if it cannot connect.
5. Open `http://localhost:5173`, create an account, sign out, and sign back in. User accounts are inserted into the MySQL `users` table. Successful checkout submissions are inserted into `orders`.

### Database tables
- `users`: account id, name, email, password salt/hash, creation time.
- `orders`: order id, account id, customer/contact details, cart items as JSON, total and creation time.

Do not commit `.env` or publish database passwords/API keys. This is a learning/demo project; before public production deployment add HTTPS, rate limiting, robust input validation, and managed secret storage.
