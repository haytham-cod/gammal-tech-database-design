# Gammal Tech Healthcare - Database Design

## Overview

This database schema is designed for a unified healthcare ecosystem that connects patients with medical, fitness, and nutrition services.

The system is **patient-centric**, meaning every interaction (meals, workouts, appointments, AI predictions) is linked back to the patient.

---

## Core Entities

### 1. Patients

Basic user information (name, email, gender, birth date).

### 2. Patient Profiles

Stores personalized health data such as:

* Height & weight
* Activity level
* Health goals

### 3. Doctors & Appointments

Patients can book appointments with doctors across different specializations.

### 4. Restaurants & Meals

* Restaurants provide food
* Food items store nutritional values
* Meal logs connect patients to what they consumed

### 5. Gyms & Workouts

* Gyms represent physical locations
* Workout logs track visits
* Workout details store exercises performed

### 6. AI Predictions

Stores AI-generated insights such as:

* Health risks
* Recommendations

Each prediction is linked to a patient and optionally to a related activity (meal, workout, etc.).

---

## Relationships

* A patient can have many meals, workouts, appointments, and predictions
* Meals are linked to food items and restaurants
* Workouts are linked to gyms and exercises
* Appointments connect patients with doctors

---

## Design Principles

* **Minimized redundancy**: reusable entities (food_items, exercises)
* **Scalable**: easy to extend (wearables, labs, medications)
* **AI-ready**: structured data supports intelligent predictions
* **Modular**: each component can evolve independently

---

## Example User Flow

Patient `p1`:

* Logs meals from restaurants
* Visits the gym and performs exercises
* Books a doctor appointment
* Receives AI recommendations

---

# How to Query

## 1. Find all cardiologists in Cairo

### SQL Example:

```sql
SELECT *
FROM doctors
WHERE specialization = 'Cardiology'
AND city = 'Cairo';
```

### Explanation:

* Filters doctors by specialization
* Filters by location
* Returns all cardiologists available in Cairo

> Note: If location is not stored in doctors, it should be added for real-world use.

---

## 2. Show this patient's meals this week with calorie totals

### SQL Example:

```sql
SELECT 
    m.patient_id,
    SUM(f.calories * m.quantity) AS total_calories
FROM meal_logs m
JOIN food_items f ON m.food_id = f.food_id
WHERE m.patient_id = 'p1'
AND m.date >= DATE_SUB(CURDATE(), INTERVAL 7 DAY)
GROUP BY m.patient_id;
```

### Explanation:

* Joins meal logs with food items
* Calculates total calories
* Filters meals from the last 7 days
* Groups results per patient

---

## Future Improvements

* Add wearable device integration
* Real-time health tracking
* Medication tracking system
* Social and community features

---

## Final Note

This schema is designed to support **AI-driven preventive healthcare**, where user data from multiple domains (nutrition, fitness, medical) is combined to deliver personalized insights.
