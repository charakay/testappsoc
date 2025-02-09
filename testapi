from fastapi import FastAPI, Request, HTTPException, Depends
import logging

app = FastAPI()

# API Token (this will be "leaked" in the attack simulation)
API_TOKEN = "supersecrettoken123"

# Configure logging
logging.basicConfig(filename="api_requests.log", level=logging.INFO, format="%(asctime)s - %(message)s")

def verify_token(request: Request):
    token = request.headers.get("Authorization")
    if not token or token != f"Bearer {API_TOKEN}":
        logging.warning(f"Unauthorized access attempt from {request.client.host}")
        raise HTTPException(status_code=401, detail="Unauthorized")
    return token

@app.get("/users")
def get_users(token: str = Depends(verify_token)):
    """ Simulates an endpoint returning user data. """
    logging.info("/users endpoint accessed")
    return {"users": [{"id": 1, "name": "Alice"}, {"id": 2, "name": "Bob"}]}

@app.get("/admin")
def admin_action(token: str = Depends(verify_token)):
    """ Simulates an admin-only action. """
    logging.info("/admin endpoint accessed - ADMIN ACTION")
    return {"message": "Admin action performed"}

@app.get("/logs")
def get_logs(token: str = Depends(verify_token)):
    """ Simulates returning system logs. """
    logging.info("/logs endpoint accessed")
    return {"logs": ["Log entry 1", "Log entry 2"]}

@app.get("/")
def home():
    return {"message": "API is running. Use an API token to access endpoints."}
