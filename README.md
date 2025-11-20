# 🌍 Country Blog Site (FastAPI + React)

The **Country Blog Site** is a full-stack web application built with **FastAPI** (backend) and **React.js** (frontend). It allows users to upload images, create, view, and delete blog posts stored in a SQLite database. CORS is configured so the React frontend can talk to the FastAPI backend seamlessly.

---

## 🚀 Project Overview

**Tech Stack**

* **Backend:** FastAPI (Python)
* **Database:** SQLite (local)
* **Frontend:** React.js
* **File Uploads:** `python-multipart`, `aiofiles` (FastAPI static files)
* **CORS:** `fastapi.middleware.cors.CORSMiddleware`
* **Server:** Uvicorn (ASGI)
* **ORM / DB tools:** SQLAlchemy, Pydantic

**Main Features**

* REST endpoints for posts (create, list, delete).
* Image upload endpoint that saves images to a local `images/` folder and returns its path.
* SQLite-backed `DbPost` model (id, image_url, title, content, creator, timestamp).
* React UI to create posts (with image upload), view all posts (reverse chronological), and delete posts.
* Static file serving for images at `/images`.

---

## ⚙️ Installation

### 1️⃣ Clone the repository

```bash
git clone https://github.com/yourusername/country-blog-site.git
cd country-blog-site
```

### 2️⃣ Create and activate virtual environment (Windows example)

```bash
python -m venv fastapi-env
fastapi-env\Scripts\activate.bat
```

### 3️⃣ Install backend dependencies

```bash
pip install -r requirements.txt
```

**Example `requirements.txt`:**

```
fastapi
uvicorn
sqlalchemy
pydantic
python-multipart
aiofiles
```

### 4️⃣ Start FastAPI backend

```bash
uvicorn main:app --reload --port 8000
```

API base: `http://localhost:8000/`

---

## 🧩 Backend — Key Files & Routes

**main.py**

* Mounts static `/images` folder.
* Includes router `post`.
* Creates DB tables.

**database**

* `database.py` — SQLAlchemy engine, `SessionLocal`, `Base`, `get_db()`.
* `models.py` — `DbPost` model: `id, image_url, title, content, creator, timestamp`.

**routers/post.py**

* `POST /post` — create post (body: `PostBase` JSON).
* `GET /post/all` — list all posts.
* `DELETE /post/{id}` — delete a post.
* `POST /post/image` — upload an image file (returns saved path).

**db_post.py**

* `create(db, request)` — create DB record and return it.
* `get_all(db)` — return all posts.
* `delete(id, db)` — delete post by id, raises 404 if missing.

---

## 🖥️ Frontend (React) — Setup

### 1️⃣ Create and start React app

```bash
cd fastapi_web
npx create-react-app .
npm install
npm start
```

### 2️⃣ App overview

* `App.js` — fetches `POST /post/all`, displays posts (reversed).
* `Post.js` — component showing `post.image_url`, `title`, `content`, `creator`; includes delete button calling `DELETE /post/{id}`.
* `NewPost.js` — image upload (POST `/post/image`) followed by creating post (POST `/post`).

### 3️⃣ CORS

Backend must allow `http://localhost:3000` in `CORSMiddleware` to enable React <> FastAPI requests.

---

## 📦 Media & Static Setup

* Uploaded images saved to `images/` folder (served by `app.mount('/images', StaticFiles(...))`).
* In React the image `src` uses `http://localhost:8000/<image_path>`.

---

## 🔧 Run the Full App (dev)

1. Start FastAPI (backend)

```bash
uvicorn main:app --reload --port 8000
```

2. Start React (frontend)

```bash
cd fastapi_web
npm start
```

3. Open:

* Frontend: `http://localhost:3000`
* Backend docs (auto-generated): `http://localhost:8000/docs`

---

## 🔐 Security & Notes

* Image upload writes files to local `images/` directory — validate file types and add limits for production.
* For production, serve static files via a proper static server or use cloud storage (S3).
* Replace SQLite with PostgreSQL for production and configure environment variables for DB credentials.
* Add authentication (JWT/session) before enabling user-specific actions if needed.

---

## 🔧 Future Enhancements

* Add user authentication (sign-up/login) and associate posts with real users.
* Pagination for posts and search/filter by country or tag.
* Image resizing and validation on upload.
* Deploy backend + frontend (Docker, or separate hosting) and use cloud storage for images.
* Add automated tests and CI pipeline.

---

## 👨‍💻 Author

**N. Aravind Santhosh**
