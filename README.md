# Bus Scheduling & Route Optimization System

**Smart India Hackathon Project** — Automated scheduling and route management for Delhi Transport Corporation (DTC)

---

## 📋 Overview

An intelligent **full-stack web application** designed to streamline and automate bus scheduling, crew management, and route optimization for large-scale public transport operations. Built during **Smart India Hackathon 2024** with a real-world problem statement from Delhi Transport Corporation.

**What it solves:**
- Manual scheduling inefficiencies causing crew overload and delays
- Suboptimal route planning leading to congestion and poor service coverage
- Lack of real-time visibility into bus and crew assignments
- Route overlap redundancies and scheduling conflicts

**Outcome**: Competed among **465+ teams**, demonstrating full-stack problem-solving ability

---

## 🎯 Key Features

### 1. **Linked Duty Scheduling**
- Assign crews to specific buses for entire shift duration
- Maintain crew-bus familiarity and accountability
- Automated roster management
- Real-time duty status tracking

### 2. **Unlinked Duty Scheduling**
- Flexible crew handover between buses
- Automatic rest period management
- Dynamic crew reallocation after breaks
- Optimized crew utilization

### 3. **Route Management & Optimization**
- Visual map integration for route visualization
- Draw and create new routes in real-time
- Automatic overlap detection with existing routes
- GIS-based route optimization
- Congestion-aware path planning

### 4. **Smart Automation**
- ML-based predictive scheduling
- Demand forecasting for peak hours
- Automated conflict resolution
- Historical data analysis for optimization

---

## 🛠️ Technology Stack

### Frontend
- **Framework**: React 18+
- **Styling**: CSS3, Tailwind CSS (for responsive UI)
- **State Management**: React Hooks / Context API
- **Mapping**: Leaflet.js / Google Maps API
- **Build Tool**: Vite / Create React App

### Backend
- **Framework**: Django (Python)
- **Database**: PostgreSQL / MySQL
- **API**: RESTful APIs with Django REST Framework
- **Authentication**: JWT / Token-based auth
- **ML Model**: ML algorithm for scheduling optimization

### DevOps & Deployment
- **Version Control**: Git & GitHub
- **Hosting**: Vercel (Frontend) / Heroku / AWS (Backend)
- **APIs**: RESTful architecture

---

## 📁 Project Structure

```
Bus-Scheduling-Smart-India-Hackathon/
├── frontend/
│   ├── src/
│   │   ├── components/      # React components (Map, Schedule, Dashboard)
│   │   ├── pages/          # Page components
│   │   ├── styles/         # CSS & Tailwind styles
│   │   ├── utils/          # Helper functions
│   │   └── App.jsx
│   ├── public/
│   ├── package.json
│   └── vite.config.js
├── backend/
│   ├── api/                # Django REST API endpoints
│   ├── models.py           # Database models (Bus, Crew, Route, Schedule)
│   ├── serializers.py      # API serializers
│   ├── views.py            # API views & logic
│   ├── urls.py             # URL routing
│   ├── settings.py         # Django configuration
│   └── manage.py
├── ml_model/               # Machine learning for optimization
│   ├── scheduling_model.py
│   ├── route_optimization.py
│   └── data/
├── docs/
│   ├── README.md
│   ├── API_DOCUMENTATION.md
│   ├── SETUP.md
│   └── ARCHITECTURE.md
├── .gitignore
├── requirements.txt        # Python dependencies
├── package.json           # Node dependencies
└── PPT/                   # Hackathon presentation
```

---

## 🚀 Quick Start

### Prerequisites
- Node.js 16+ & npm
- Python 3.8+
- PostgreSQL / MySQL
- Git

### Frontend Setup

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Configure environment variables
echo "REACT_APP_API_URL=http://localhost:8000" > .env

# Start development server
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser

### Backend Setup

```bash
# Navigate to backend directory
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure database
# Update settings.py with your database credentials

# Run migrations
python manage.py makemigrations
python manage.py migrate

# Start development server
python manage.py runserver
```

Backend runs on [http://localhost:8000](http://localhost:8000)

---

## 🔌 API Documentation

### Core Endpoints

#### Bus Management
```
GET    /api/buses/              # List all buses
POST   /api/buses/              # Create new bus
GET    /api/buses/{id}/         # Get bus details
PUT    /api/buses/{id}/         # Update bus
DELETE /api/buses/{id}/         # Delete bus
```

#### Crew Management
```
GET    /api/crews/              # List all crews
POST   /api/crews/              # Register new crew
GET    /api/crews/{id}/         # Get crew details
PUT    /api/crews/{id}/         # Update crew info
DELETE /api/crews/{id}/         # Remove crew
```

#### Schedule Management
```
GET    /api/schedules/          # Get all schedules
POST   /api/schedules/          # Create schedule
GET    /api/schedules/{id}/     # Get schedule details
PUT    /api/schedules/{id}/     # Update schedule
DELETE /api/schedules/{id}/     # Cancel schedule
```

#### Route Management
```
GET    /api/routes/             # List all routes
POST   /api/routes/             # Create new route
GET    /api/routes/{id}/        # Get route details
POST   /api/routes/optimize/    # Optimize route
POST   /api/routes/detect-overlap/  # Detect overlaps
```

#### Real-time Tracking
```
WS     /ws/bus-tracking/        # WebSocket for live bus tracking
```

---

## 💾 Database Schema

### Core Tables

**buses**
```sql
CREATE TABLE buses (
    id PRIMARY KEY,
    bus_number VARCHAR(50) UNIQUE,
    capacity INT,
    route_id FOREIGN KEY,
    status ENUM ('active', 'maintenance', 'inactive'),
    created_at TIMESTAMP
);
```

**crews**
```sql
CREATE TABLE crews (
    id PRIMARY KEY,
    name VARCHAR(100),
    employee_id VARCHAR(50) UNIQUE,
    qualification VARCHAR(100),
    shift_preference VARCHAR(50),
    status ENUM ('active', 'on_leave', 'retired')
);
```

**routes**
```sql
CREATE TABLE routes (
    id PRIMARY KEY,
    route_number VARCHAR(50),
    source VARCHAR(100),
    destination VARCHAR(100),
    distance DECIMAL(10, 2),
    estimated_time INT,
    geometry GEOSPATIAL
);
```

**schedules**
```sql
CREATE TABLE schedules (
    id PRIMARY KEY,
    bus_id FOREIGN KEY,
    crew_id FOREIGN KEY,
    route_id FOREIGN KEY,
    start_time DATETIME,
    end_time DATETIME,
    status ENUM ('scheduled', 'in_progress', 'completed', 'cancelled')
);
```

---

## 🤖 ML Model Integration

### Scheduling Optimization Algorithm

The system uses **constraint satisfaction + genetic algorithm** for optimal scheduling:

1. **Input**: Bus fleet data, crew availability, routes, demand patterns
2. **Constraints**: 
   - No crew exceeds max duty hours
   - No route without assigned bus
   - Crew preferences respected when possible
   - Maintenance windows scheduled
3. **Optimization**: Minimize total delay, maximize crew satisfaction
4. **Output**: Optimized schedule with assignment matrix

### Route Optimization

- **Algorithm**: Modified A* with traffic-aware heuristics
- **Data Source**: Historical traffic patterns, real-time GPS
- **Output**: Best route with ETA

---

## 📊 Features Demonstrated

| Feature | Status | Recruiter Value |
|---------|--------|-----------------|
| User Authentication | ✅ Complete | Secure backend |
| Dashboard & UI | ✅ Complete | Frontend mastery |
| Schedule Management | ✅ Complete | Business logic |
| Route Visualization | ✅ In Progress | GIS integration |
| Real-time Tracking | ✅ In Progress | WebSocket skills |
| ML Optimization | ✅ Model Ready | Algorithm knowledge |
| Database Design | ✅ Complete | Schema design |
| API Development | ✅ Complete | RESTful architecture |
| Error Handling | ✅ Complete | Robust backend |
| Documentation | ✅ Complete | Professional standards |

---

## 🎓 What This Project Demonstrates

✅ **Full-stack development capability** (React + Django)  
✅ **Real-world problem solving** (Hackathon challenge)  
✅ **Database design** (Complex relational schema)  
✅ **API development** (RESTful architecture)  
✅ **Frontend UI/UX** (Responsive design with maps)  
✅ **Algorithm implementation** (Scheduling optimization)  
✅ **Team collaboration** (Hackathon team project)  
✅ **Scalable architecture** (Microservices-ready)  
✅ **Documentation** (Professional project docs)  

---

## 🔮 Future Enhancements

### Phase 2 - Advanced Features
- [ ] Mobile app for drivers (React Native)
- [ ] Real-time GPS tracking with live updates
- [ ] SMS/WhatsApp notifications for crew
- [ ] Passenger booking integration
- [ ] Predictive maintenance alerts
- [ ] Dynamic pricing based on demand
- [ ] Multi-transport integration (metro, auto, cab)

### Phase 3 - AI/ML Enhancements
- [ ] Deep learning for demand forecasting
- [ ] Reinforcement learning for schedule optimization
- [ ] Anomaly detection for route issues
- [ ] NLP chatbot for user queries

### Phase 4 - DevOps & Scalability
- [ ] Docker containerization
- [ ] Kubernetes orchestration
- [ ] CI/CD pipeline
- [ ] Load balancing
- [ ] Database replication

---

## 📸 Screenshots & Demo

### Dashboard
![Dashboard Preview](./docs/screenshots/dashboard.png)

### Route Optimization
![Route Map](./docs/screenshots/route-map.png)

### Schedule Management
![Schedule UI](./docs/screenshots/schedule.png)

---

## 🏆 Hackathon Achievement

- **Hackathon**: Smart India Hackathon 2024
- **Problem Statement**: Automated Bus Scheduling & Route Optimization
- **Client**: Delhi Transport Corporation (DTC)
- **Team Size**: 5 members
- **Participants**: 465+ teams
- **Achievement**: Qualified with working prototype

---

## 👥 Team & Contributions

| Role | Contributor | Responsibility |
|------|-------------|-----------------|
| **Full Stack Lead** | Jatin Ahuja | Architecture, React, Django |
| **Frontend** | Team Member | UI/UX, Map Integration |
| **Backend** | Team Member | API, Database, Logic |
| **ML Engineer** | Team Member | Algorithm, Optimization |
| **DevOps** | Team Member | Deployment, Setup |

---

## 📝 Documentation

- **[Setup Guide](./docs/SETUP.md)** - Installation & configuration
- **[API Docs](./docs/API_DOCUMENTATION.md)** - Detailed endpoint reference
- **[Architecture](./docs/ARCHITECTURE.md)** - System design & decisions
- **[Contributing](./CONTRIBUTING.md)** - Contribution guidelines

---

## 📄 License

MIT License - See LICENSE file for details

---

## 🔗 Related Links

- **Main Portfolio**: [GitHub Profile](https://github.com/2403051050553)
- **Personal Portfolio**: [jatin-ahuja-portfolio.vercel.app](https://jatin-ahuja-portfolio.vercel.app)
- **DSA Solutions**: [LeetCode Repository](https://github.com/2403051050553/LeetCode-Solutions)
- **LinkedIn**: [Jatin Tehalram Ahuja](https://www.linkedin.com/in/jatin-tehalram-ahuja-0386b5390/)

---

## 📞 Contact & Support

For questions, suggestions, or collaboration:
- 📧 **Email**: [2403051050553@paruluniversity.ac.in](mailto:2403051050553@paruluniversity.ac.in)
- 💼 **LinkedIn**: [Connect](https://www.linkedin.com/in/jatin-tehalram-ahuja-0386b5390/)
- 🐙 **GitHub**: [@2403051050553](https://github.com/2403051050553)

---

<div align="center">

**Built with determination & passion for solving real-world problems**

⭐ [Star this repo](https://github.com/2403051050553/Bus-Scheduling-Smart-India-Hackathon-) if you find it helpful!

Last Updated: October 2025

</div>
