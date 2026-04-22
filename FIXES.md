# FIXES.md — Bug Documentation

## Fix 1
- **File:** `api/main.py`
- **Line:** 8
- **Problem:** Redis host was hardcoded as `localhost`. Inside Docker containers communicate via service names, not localhost. This causes a connection failure when running in containers.
- **Fix:** Changed to use environment variable `REDIS_HOST` with fallback to `redis` (the Docker service name).

## Fix 2
- **File:** `api/main.py`
- **Line:** 8
- **Problem:** Redis port was hardcoded as `6379`. Should be configurable via environment variable for flexibility across environments.
- **Fix:** Changed to use environment variable `REDIS_PORT` with fallback to `6379`.

## Fix 3
- **File:** `api/main.py`
- **Line:** 8
- **Problem:** `decode_responses=True` was missing from the Redis client. Without it, Redis returns bytes instead of strings, causing decode errors throughout the application.
- **Fix:** Added `decode_responses=True` to the Redis client initialization.

## Fix 4
- **File:** `worker/worker.py`
- **Line:** 6
- **Problem:** Redis host was hardcoded as `localhost`. Same container networking issue as the API.
- **Fix:** Changed to use environment variable `REDIS_HOST` with fallback to `redis`.

## Fix 5
- **File:** `worker/worker.py`
- **Line:** 6
- **Problem:** Redis port was hardcoded as `6379`. Should come from environment variable.
- **Fix:** Changed to use environment variable `REDIS_PORT` with fallback to `6379`.

## Fix 6
- **File:** `worker/worker.py`
- **Line:** 6
- **Problem:** `decode_responses=True` was missing. Redis returns bytes by default, causing `job_id.decode()` to fail when `decode_responses=True` is not set — the decode call becomes redundant and error-prone.
- **Fix:** Added `decode_responses=True` and removed unnecessary `.decode()` call on line 18.

## Fix 7
- **File:** `frontend/app.js`
- **Line:** 6
- **Problem:** API URL was hardcoded as `http://localhost:8000`. This fails inside Docker because the frontend container cannot reach the API container via localhost.
- **Fix:** Changed to use environment variable `API_URL` with fallback to `http://api:8000`.

## Fix 8
- **File:** `api/requirements.txt`
- **Line:** 1-3
- **Problem:** No version pins on dependencies. Unpinned dependencies cause non-reproducible builds — different versions may be installed at different times.
- **Fix:** Added specific version pins for all packages including test dependencies.

## Fix 9
- **File:** `worker/requirements.txt`
- **Line:** 1
- **Problem:** No version pin on redis package.
- **Fix:** Added specific version pin `redis==5.0.1`.

## Fix 10
- **File:** Repository root
- **Problem:** No `.gitignore` file present. The `api/.env` file containing secrets could accidentally be committed to the repository.
- **Fix:** Created `.gitignore` to exclude `.env` files, `node_modules`, `__pycache__`, and other generated files.

## Fix 11
- **File:** Repository root
- **Problem:** No `.env.example` file to guide developers on required environment variables.
- **Fix:** Created `.env.example` with placeholder values for all required variables.