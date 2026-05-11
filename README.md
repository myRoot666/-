# -
完整项目及数据集
# ShortVision

ShortVision is a FastAPI + Vue application for short-video browsing, recommendation, moderation, and chat.

## Runtime entry points

- Vue login: `http://127.0.0.1:8000/app/login`
- Vue register: `http://127.0.0.1:8000/app/register`
- Vue user portal: `http://127.0.0.1:8000/app/user`
- Vue admin portal: `http://127.0.0.1:8000/app/admin`
- Backend service: `backend/`

Legacy root HTML pages have been archived into `legacy/` and are no longer used as the primary frontend.

## Database

The backend uses local MySQL by default:

```env
DATABASE_URL=mysql+mysqlconnector://root:root@localhost:3306/short_video_recommendation
```

Config files:

- `backend/.env`
- `backend/.env.example`

## Persisted data

The backend writes directly to MySQL. The main persisted entities are:

- users and authentication data
- categories, tags, and authors
- video metadata
- watch, like, save, comment, share, and follow behaviors
- recommendation history
- admin-side video analysis outputs and feature summaries

## Database initialization

Make sure MySQL is running and the configured account can connect, then run:

```bash
cd backend
python scripts/init_db.py
```

This process creates the database schema and seeds baseline data.

Default accounts:

- admin: `admin / admin123456`
- demo user: `demo_user / demo123456`

## KuaiRec data pipeline

Available scripts:

- `backend/scripts/00_download_kuairec.py`
- `backend/scripts/01_clean_kuairec.py`
- `backend/scripts/02_create_mysql_tables.sql`
- `backend/scripts/03_import_to_mysql.py`

Typical flow:

```bash
cd backend
python scripts/00_download_kuairec.py
python scripts/01_clean_kuairec.py --matrix small
python scripts/03_import_to_mysql.py --mysql-password root
```

Outputs:

- `backend/clean_data/users.csv`
- `backend/clean_data/videos.csv`
- `backend/clean_data/interactions.csv`

## Start the project

Backend:

```bash
cd backend
pip install -r requirements.txt
python -m uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

Frontend build:

```bash
cd frontend
npm.cmd install
npm.cmd run build
```

## Main APIs

- `POST /api/auth/login`
- `GET /api/user/profile`
- `GET /api/videos`
- `POST /api/videos/{video_id}/interact`
- `GET /api/dashboard/overview`
- `POST /api/video/analyze`
