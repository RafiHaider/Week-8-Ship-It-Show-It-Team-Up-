# 🚀 P2-API: FastAPI Deep Learning Inference Service

A production-ready FastAPI backend serving deep learning image classification models with robust input validation, error handling, automated testing, and latency benchmarking. Developed for Lab 8 coursework.

---

## 📂 Project Structure
p2-api/

├── .venv/             # Isolated Python virtual environment

├── app.py   # Main FastAPI application & endpoint definitions

├── test_api.py # Pytest suite (includes parity and input validation tests)

├── bench.py    # Latency benchmarking script (n=50)

├── week6_model.pt   # PyTorch trained model weights

├── sample.jpeg   # Default validation test image

└── README.md            # Project documentation

---
### 1. Swagger UI Overview (App Interface):
![Picture 1: App Interface and Swagger UI Overview](<img width="512" height="272" alt="pic" src="https://github.com/user-attachments/assets/5f293738-7fca-4b8f-b3d2-56e12618e068" />
)

## 🛠️ API Endpoints

* **GET /health**
  *Description: Health check endpoint to verify server status, confirm model loading, and list supported classification classes.
  * Response:
   ``` JSON
  {
   "status": "ok",
   "model_loaded": true,
   "classes": [
     "NORMAL",
     "PNEUMONIA"
   ],
   "positive": "PNEUMONIA"
  }
Health Check (/health):
![Picture 2: GET /health Endpoint Response](<img width="739" height="576" alt="pic1" src="https://github.com/user-attachments/assets/2db15870-e4a3-47d2-ab1e-4fbfa4afac5a" />

)

* **POST /predict**
  Description: Accepts an image file upload (sample.jpeg, with optional x-api-key header), validates format constraints, runs inference through the PyTorch model, and returns class probabilities and latency.
  Image Prediction (/predict):
![Picture 3: POST /predict Request and Live Response](<img width="512" height="405" alt="pic2" src="https://github.com/user-attachments/assets/1fca7497-23f9-47a2-8e2d-cc9ec0135b7e" /> <img width="512" height="247" alt="pic_2" src="https://github.com/user-attachments/assets/5679b892-1e7e-469b-9884-18a856de6f8c" />

)
* **POST /analyze**
 Description: Accepts an image file upload along with a text note (and optional x-api-key header) to perform combined vision prediction, note handling, urgency/decision routing, and decision-support output.
Combined Vision + Note Analysis (/analyze):
![Picture 6: POST /analyze Response Schemas (200 & 422)](<img width="512" height="425" alt="pic3" src="https://github.com/user-attachments/assets/50c50393-67f8-4b58-9150-104587fdc202" /> <img width="732" height="562" alt="pic_3" src="https://github.com/user-attachments/assets/4aa58f4d-575a-47ed-94e4-f87996476279" />

)
---

## ⚙️ Local Setup & Execution

1. Clone the repository or navigate to the project directory:
   ``` Bash
   cd p2-api

3. Create and activate a virtual environment:
    ``` Bash
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1

5. Install required dependencies:
    ``` Bash
   pip install fastapi uvicorn torch torchvision pillow pytest httpx

7. Run the FastAPI server:
    ``` Bash
   uvicorn app:app --reload
  (The server will run locally at http://127.0.0.1:8000/docs)

---
## 📸 Application & Endpoint Walkthrough
1. Swagger UI Overview (App Interface)
   Displays the Project 2 Vision + Text API Swagger documentation interface with the /health, /predict, and /analyze endpoints along with request/response schemas.
2. Displays the Project 2 Vision + Text API Swagger documentation interface with the /health, /predict, and /analyze endpoints along with request/response schemas.
3. mage Prediction (/predict)
Uploading sample.jpeg via multipart/form-data to POST /predict returns HTTP 200 with the predicted label ("NORMAL"), per-class probability scores, and inference latency (latency_ms), along with documented 200 and 422 response schemas.
4. Combined Vision + Note Analysis (/analyze)
Submitting sample.jpeg and a note to POST /analyze returns HTTP 200 containing the vision prediction (label, probabilities, latency_ms), note_masked, fields, urgent_terms, decision, reasons, and decision-support disclaimer, followed by the full 200 and 422 response schemas.



## 🧪 Testing & Performance Metrics

1. **Automated Testing (Pytest)**
   Run the test suite to verify endpoint correctness, input schema validation, and model parity:
    ``` Bash
   pytest -v test_api.py
  * **Result:** 8 passed successfully.

3. **Latency Benchmarking**
   To test throughput and latency metrics under load (n=50 requests):
    ``` Bash
   python bench.py
  * **Hardware Environment:** Windows Host Workstation
  * **Latency (p50):** 87 ms
  * **Latency (p95):** 115 ms
  * **Max Latency:** 134 ms
  * **Throughput (1 worker):** ~11.0 requests/s

---

## 📊 Little's Law Capacity Estimation
Using Little's Law:
* At a p50 latency of 87 ms (0.087 s), a single worker handles ~11.49 requests/second.
* Scaled to a full 24-hour day (86,400 seconds):
  Daily Capacity = 11.494 * 86,400 = ~993,100 requests/day per worker.

---

## 🛡️ Error Handling
The API includes strict request validation and handles invalid or malformed inputs safely:
* **Invalid File Type/Schema Mismatch:** Returns 415 Unsupported Media Type or 422 Unprocessable Entity.
* **Missing Payload:** Handled via Pydantic schemas without exposing raw stack traces.
