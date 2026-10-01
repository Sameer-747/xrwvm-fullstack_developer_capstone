# Best Cars Dealership – Full-Stack Developer Capstone

**Project name:** Best Cars Dealership – a national car retailer web application

A responsive full-stack web application for **Best Cars Dealership**, a national car retailer in the U.S. Users can browse dealership branches across the country, filter dealers by state, view dealer details and reviews (with sentiment analysis), and, after signing up or logging in, post their own reviews. Admins manage car makes and models through the Django admin site.

## Architecture

| Component | Technology | Purpose |
|-----------|------------|---------|
| Frontend | React + Bootstrap, static HTML (Home / About / Contact) | UI for browsing dealers, login, register, posting reviews |
| Main app / API gateway | Django (SQLite) | User auth, Car Make / Car Model models, proxy views to microservices |
| Dealers & Reviews API | Node.js + Express + MongoDB (Mongoose), Docker | Stores and serves dealerships and reviews |
| Sentiment analyzer | Flask + NLTK VADER on IBM Cloud Code Engine | Classifies review text as positive / neutral / negative |
| Deployment | Docker, Kubernetes (`server/deployment.yaml`) | Containerised deployment of the Django app |
| CI/CD | GitHub Actions (`.github/workflows/main.yml`) | Lints Python (flake8) and JavaScript (JSHint) on every push / PR |

## Django endpoints

| Endpoint | Description |
|----------|-------------|
| `POST /djangoapp/login` | Log in |
| `GET /djangoapp/logout` | Log out |
| `POST /djangoapp/register` | Sign up |
| `GET /djangoapp/get_dealers[/<state>]` | All dealers / dealers by state |
| `GET /djangoapp/dealer/<id>` | Dealer details |
| `GET /djangoapp/reviews/dealer/<id>` | Reviews for a dealer (with sentiment) |
| `POST /djangoapp/add_review` | Post a review (logged-in users) |
| `GET /djangoapp/get_cars` | All car makes and models |

## Running locally

```bash
# 1. Dealers/Reviews API (MongoDB + Node)
cd server/database
docker build . -t nodeapp && docker-compose up

# 2. Sentiment analyzer
cd server/djangoapp/microservices
docker build . -t senti_analyzer
docker run -p 5050:5000 senti_analyzer

# 3. React frontend
cd server/frontend && npm install && npm run build

# 4. Django
cd server
pip install -r requirements.txt
python3 manage.py makemigrations && python3 manage.py migrate --run-syncdb
python3 manage.py createsuperuser
python3 manage.py runserver
```

Set `backend_url` and `sentiment_analyzer_url` in `server/djangoapp/.env`.

## Author

Shaik Sameer – Full-Stack Developer
