# FastAPI Book API

## Overview

This project is a RESTful API built with FastAPI for managing a book collection. It provides comprehensive CRUD (Create, Read, Update, Delete) operations for books with proper error handling, input validation, and documentation.

## Features/ characteristics

- 📚 Book management (CRUD operations)
- ✅ Input validation using Pydantic models
- 🔍 Enum-based genre classification
- 🧪 Complete test coverage
- 📝 API documentation (auto-generated by FastAPI)
- 🔒 CORS middleware enabled

## Project Structure

```
fastapi-book-project/
├── api/
│   ├── db/
│   │   ├── __init__.py
│   │   └── schemas.py      # Data models
│   ├── routes/
│   │   ├── __init__.py
│   │   └── books.py        # Book route 
│   └── router.py           # API router 
├── core/
│   ├── __init__.py
│   └── config.py           # Application settings
├── tests/
│   ├── __init__.py
│   └── test_books.py       # API endpoint tests
├── main.py                 # Application entry point
├── requirements.txt        # Project dependencies
└── README.md
```

## Technologies 

- Python 3.12
- FastAPI
- Pydantic
- pytest
- uvicorn

## Installations

1. Clone the repository:

```bash
git clone https://github.com/hng12-devbotops/fastapi-book-project.git
cd fastapi-book-project
```

2. Create a virtual environment and activate it:

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

## Running the Application

1. Start the server:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

2. Access the API documentation:

- Swagger UI: [http://localhost:8000/docs](http://localhost:8000/docs)
- ReDoc: [http://localhost:8000/redoc](http://localhost:8000/redoc)

## API Endpoints

### Books

- `GET /api/v1/books/` - Get all books
- `GET /api/v1/books/{book_id}` - Get a specific book
- `POST /api/v1/books/` - Create a new book
- `PUT /api/v1/books/{book_id}` - Update a book
- `DELETE /api/v1/books/{book_id}` - Delete a book

### Health Check

- `GET /healthcheck` - Check API status

```bash
curl http://127.0.0.1:8000/healthcheck
```

## Book Schema

```json
{
  "id": 1,
  "title": "Book Title",
  "author": "Author Name",
  "publication_year": 2024,
  "genre": "Fantasy"
}
```

Available genres:

- Science Fiction
- Fantasy
- Horror
- Mystery
- Romance
- Thriller

## Running Tests

```bash
pytest
```

## Deployment Guide (AWS + Nginx + Gunicorn)

### 1️⃣ Connect to Your VPS
```bash
ssh user@your-vps-ip
```

### 2️⃣ Update & Install Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3-pip python3-venv nginx git
```

### 3️⃣ Clone Your Repository
```bash
git clone https://github.com/YOUR_GITHUB_USERNAME/fastapi-book-project.git
cd fastapi-book-project
```

### 4️⃣ Set Up a Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

### 5️⃣ Run the FastAPI App
```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

### 6️⃣ Use a Process Manager (Gunicorn + Systemd)
```bash
pip install gunicorn
```
Create a Gunicorn service file:
```bash
sudo nano /etc/systemd/system/fastapi.service
```
Add the following:
```
[Unit]
Description=FastAPI Application
After=network.target

[Service]
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/fastapi-book-project
ExecStart=/home/ubuntu/fastapi-book-project/venv/bin/gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app --bind 0.0.0.0:8000

Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```
Reload systemd and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable fastapi
sudo systemctl start fastapi
```

### 7️⃣ Configure Nginx as a Reverse Proxy
```bash
sudo nano /etc/nginx/sites-available/fastapi
```
Add:
```
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
Enable it:
```bash
sudo ln -s /etc/nginx/sites-available/fastapi /etc/nginx/sites-enabled
sudo systemctl restart nginx
```

### 8️⃣ Automate Deployment with Git
```bash
cd /home/ubuntu/fastapi-book-project
git pull origin main
sudo systemctl restart fastapi
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

