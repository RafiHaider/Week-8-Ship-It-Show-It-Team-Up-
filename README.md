# 🚀 P2-API: FastAPI Deep Learning Inference Service

A production-ready FastAPI backend serving deep learning image classification models with robust input validation, error handling, automated testing, and latency benchmarking. Developed for Lab 8 coursework.

---

## 📂 Project Structure
p2-api/
├── .venv/               # Isolated Python virtual environment
├── app.py               # Main FastAPI application & endpoint definitions
├── test_api.py          # Pytest suite (includes parity and input validation tests)
├── bench.py             # Latency benchmarking script (n=50)
├── week6_model.pt       # PyTorch trained model weights
├── sample.jpeg          # Default validation test image
└── README.md            # Project documentation

---

## 🛠️ API Endpoints

* **GET /health**
  * Description: Health check endpoint to verify server status.
  * Response: {"status": "healthy"}

* **POST /predict**
  * Description: Accepts an image file upload, validates format constraints, runs inference through the PyTorch model, and returns prediction results.

---

## ⚙️ Local Setup & Execution

1. Clone the repository or navigate to the project directory:
   cd p2-api

2. Create and activate a virtual environment:
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1

3. Install required dependencies:
   pip install fastapi uvicorn torch torchvision pillow pytest httpx

4. Run the FastAPI server:
   uvicorn app:app --reload
   (The server will run locally at http://127.0.0.1:8000)

---

## 🧪 Testing & Performance Metrics

1. **Automated Testing (Pytest)**
   Run the test suite to verify endpoint correctness, input schema validation, and model parity:
   pytest -v test_api.py
   * **Result:** 8 passed successfully.

2. **Latency Benchmarking**
   To test throughput and latency metrics under load (n=50 requests):
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
  Daily Capacity = 11.49 * 86,400 = ~992,700 requests/day per worker.

---

## 🛡️ Error Handling
The API includes strict request validation and handles malicious or malformed inputs safely:
* **Invalid File Type:** Returns 415 Unsupported Media Type or 422 Unprocessable Entity.
* **Missing Payload:** Handled via Pydantic schemas without exposing raw stack traces.