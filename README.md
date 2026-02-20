# 🏥 Patient Management REST API

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-Modern%20API-green)
![Pydantic](https://img.shields.io/badge/Pydantic-v2-orange)
![Status](https://img.shields.io/badge/Project-Active-success)

A fully functional **CRUD-based REST API** built using **FastAPI** for managing patient records.

This project demonstrates backend fundamentals including:

* REST principles
* Request validation
* Computed fields (BMI)
* Partial updates
* Error handling
* JSON-based persistence

---

## 🚀 Tech Stack

* **Framework:** FastAPI
* **Language:** Python 3.10+
* **Validation:** Pydantic v2
* **Server:** Uvicorn (ASGI)
* **Storage:** JSON file (`patients.json`)

---

## ✨ Features

* ✅ Create a new patient
* ✅ Retrieve all patients
* ✅ Retrieve patient by ID
* ✅ Update patient (Partial Update)
* ✅ Delete patient
* ✅ Sort by height, weight, or BMI
* ✅ Automatic BMI calculation
* ✅ Health verdict generation
* ✅ Input validation
* ✅ Proper HTTP status codes

---

## 📊 BMI Calculation Logic

```
BMI = weight (kg) / (height in meter)^2
```

The API automatically computes:

* `bmi`
* `verdict` (Underweight, Normal, Overweight, Obese)

---

## 📂 Project Structure

```
.
├── main.py
├── patients.json
└── README.md
```

---

## 🔌 API Endpoints

| Method | Endpoint                         | Description              |
| ------ | -------------------------------- | ------------------------ |
| GET    | `/`                              | Project information      |
| GET    | `/view`                          | Get all patients         |
| GET    | `/patients/{patient_id}`         | Get patient by ID        |
| POST   | `/create`                        | Create new patient       |
| PUT    | `/edit/{patient_id}`             | Update patient (partial) |
| DELETE | `/delete/{patient_id}`           | Delete patient           |
| GET    | `/sort?sort_by=height&order=asc` | Sort patients            |

---

## 📥 Sample Request Body (Create Patient)

```json
{
  "id": "P001",
  "name": "Ram",
  "city": "Mumbai",
  "age": 25,
  "gender": "male",
  "height": 170,
  "weight": 70
}
```

---

## 🛠 Installation & Setup

### 1️⃣ Clone Repository

```bash
git clone <your-repo-link>
cd <project-folder>
```

### 2️⃣ Create Virtual Environment (Recommended)

```bash
python -m venv venv
source venv/bin/activate  # Mac/Linux
venv\Scripts\activate     # Windows
```

### 3️⃣ Install Dependencies

```bash
pip install fastapi uvicorn
```

### 4️⃣ Run the Server

```bash
uvicorn main:app --reload
```

---

## 📖 API Documentation

After running the server, open:

* Swagger UI → [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
* OpenAPI JSON → [http://127.0.0.1:8000/openapi.json](http://127.0.0.1:8000/openapi.json)

---

## 🧪 Example Sorting Request

```
GET /sort?sort_by=bmi&order=desc
```

---

## 🔐 Validation Rules

* Age: 1 – 119
* Height: > 0
* Weight: > 0
* Gender: `male`, `female`, `others`

---

## 📈 Future Enhancements

* Database integration (SQLite / PostgreSQL)
* Authentication (JWT)
* Pagination
* Filtering endpoints
* Docker support
* Unit testing (pytest)

---

## 🎯 Learning Outcomes

This project demonstrates:

* REST API Design
* CRUD Operations
* Data Validation
* Computed Fields
* Partial Updates
* Clean Backend Architecture

---

## 👨‍💻 Author

Built as a backend development practice project using FastAPI.

---

⭐ If you found this project helpful, consider giving it a star!
