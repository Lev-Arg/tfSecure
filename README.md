<img src=/tflogo.png>
# tfSecure
Security Management Center

# tfSecure - Security Operations Management System

A comprehensive microservices-based security management system for visitor tracking, contractor management, CCTV monitoring, waybill tracking, and key management.

## 🚀 Technology Stack

### Backend
- **Framework**: FastAPI (Python 3.11+)
- **Architecture**: Microservices (9 independent services)
- **Database**: PostgreSQL 15 with Alembic migrations
- **Cache**: Redis 7
- **Authentication**: JWT with bcrypt password hashing
- **API Documentation**: OpenAPI/Swagger (built-in with FastAPI)
- **Real-time CCTV**: OpenCV for stream testing

### Frontend
- **Framework**: Next.js 14 (React 18)
- **Language**: TypeScript
- **Styling**: Tailwind CSS + shadcn/ui components
- **State Management**: Zustand + TanStack Query
- **Real-time**: Socket.io client
- **Charts**: Recharts

### Infrastructure
- **Database**: SQLite (local) / PostgreSQL (production)
- **Database Migrations**: Alembic
- **Process Management**: Uvicorn
- **Package Management**: pip (Python), npm (Node.js)

## 📁 Project Structure

```
tfSecure/
├── backend/                      # FastAPI microservices
│   ├── shared/                  # Shared utilities and models
│   │   ├── config.py           # Configuration management
│   │   ├── database.py         # Database setup
│   │   ├── models.py           # SQLAlchemy ORM models
│   │   ├── security.py         # JWT & password hashing
│   │   ├── exceptions.py       # Custom exceptions
│   │   ├── error_handlers.py   # Centralized error handling
│   │   ├── monitoring.py       # Service monitoring utilities
│   │   ├── notification_helper.py # Automatic notifications
│   │   ├── stream_utils.py     # CCTV stream testing utilities
│   │   └── requirements.txt    # Python dependencies
│   ├── api-gateway/           # API Gateway (port 8000)
│   ├── auth-service/          # Authentication (port 8001)
│   ├── visitor-service/       # Visitor management (port 8002)
│   ├── contractor-service/    # Contractor management (port 8003)
│   ├── waybill-service/       # Waybill management (port 8004)
│   ├── cctv-service/          # CCTV monitoring (port 8005)
│   ├── keys-service/          # Key management (port 8006)
│   ├── notification-service/  # Notifications (port 8007)
│   ├── reporting-service/     # Reports & analytics (port 8008)
│   └── init_db_simple.py      # Database initialization script
│
├── frontend/                    # Next.js frontend application
│   ├── src/
│   │   ├── app/                # Next.js app directory
│   │   ├── components/         # React components
│   │   │   └── ui/            # shadcn/ui components
│   │   ├── hooks/             # Custom React hooks
│   │   └── lib/               # Utility functions
│   ├── public/                # Static assets (logos, etc.)
│   ├── package.json
│   └── next.config.js
│
├── start.py                    # Service launcher (single window)
├── kill.py                     # Emergency process killer (all platforms)
├── kill.bat                    # Emergency process killer (Windows)
├── .env.example               # Environment variables template
└── README.md                  # This file
```

## 🛠️ Microservices Architecture

### Services Overview

1. **API Gateway** (Port 8000)
   - Routes requests to appropriate microservices
   - Handles CORS and cross-cutting concerns
   - Service discovery and request proxying

2. **Authentication Service** (Port 8001)
   - User authentication with JWT tokens
   - User registration and management
   - Role-based access control (Admin, Operator, Security, Viewer)
   - Account lockout after failed login attempts
   - Password reset functionality

3. **Visitor Service** (Port 8002)
   - Visitor registration and management
   - Check-in/check-out workflows with badge generation
   - Photo capture integration
   - Automatic notifications on check-in/out
   - Visitor status tracking (Pending, Checked In, Checked Out, Expired)

4. **Contractor Service** (Port 8003)
   - Contractor registration and management
   - Permit tracking and expiry monitoring
   - Safety induction tracking
   - Insurance expiry alerts
   - Full CRUD operations with status management

5. **Waybill Service** (Port 8004)
   - Incoming, outgoing, and service waybill management
   - Container tracking for cocoa products
   - Room assignment and storage management
   - Document attachment support
   - Automatic notifications on waybill creation

6. **CCTV Service** (Port 8005)
   - Camera status monitoring with real stream testing
   - RTSP/HTTP stream connection testing using OpenCV
   - IP address and network configuration tracking
   - Maintenance scheduling and tracking
   - Stream enable/disable functionality
   - Automatic notifications for camera issues

7. **Keys Service** (Port 8006)
   - Office key management and tracking
   - Key issuance with holder information
   - Room status monitoring
   - Overdue key tracking
   - Lost key reporting and management

8. **Notification Service** (Port 8007)
   - System notifications and alerts
   - Priority-based filtering (info, warning, error, critical)
   - Action tracking and management
   - Category-based organization
   - Target user/role filtering

9. **Reporting Service** (Port 8008)
   - Dashboard statistics and real-time metrics
   - Comprehensive report generation
   - Date range filtering for all entities
   - Export functionality (JSON, CSV)
   - Summary reports with current status

## 🚀 Getting Started

### Prerequisites

- Python 3.11+ 
- Node.js 18+ (for frontend development)
- pip (Python package manager)
- npm (Node.js package manager)

### Quick Start (No Docker Required)

The system uses SQLite for local development, so no external database or Docker is required.

#### All Users (Windows, Linux, Mac)

1. **Install Python dependencies** (one-time setup)
   ```bash
   python -m ensurepip --upgrade
   python -m pip install fastapi uvicorn sqlalchemy pydantic pydantic-settings python-jose passlib bcrypt requests opencv-python
   ```

2. **Start all backend services** (single window)
   ```bash
   python start.py
   ```
   This will start all 9 microservices in a single window with colored status output.

3. **Start the frontend** (in a new terminal)
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

4. **Access the application**
   - Frontend: http://localhost:3000
   - API Gateway: http://localhost:8000
   - API Documentation: http://localhost:8000/docs

#### Service Launcher Commands

When running `python start.py`, you can use these interactive commands:
- `status` - Show service status
- `stop` - Stop all services
- `restart` - Restart all services
- `exit` - Stop and exit
- `help` - Show available commands

Press Ctrl+C to gracefully stop all services.

### Emergency Kill Switch

If services don't stop properly, use the kill scripts:

**Windows:**
```bash
kill.bat
```

**All Platforms:**
```bash
python kill.py
```

This will forcefully terminate all Python and Node processes related to tfSecure.

### Local Development (Individual Services)

#### Backend Development

For each service, navigate to its directory and run:

```bash
cd backend/auth-service  # or any other service
pip install -r ../shared/requirements.txt
python main.py
```

#### Frontend Development

```bash
cd frontend
npm install
npm run dev
```

Frontend will be available at http://localhost:3000

### Database Initialization

The project includes a simple database initialization script for local development:

```bash
cd backend
python init_db_simple.py --all          # Full initialization
python init_db_simple.py --init         # Create tables only
python init_db_simple.py --seed-admin    # Create admin user
python init_db_simple.py --seed-sample   # Create sample data
python init_db_simple.py --reset         # Reset database
```

**Default Admin Credentials:**
- Username: `admin`
- Password: `admin123` (CHANGE THIS IN PRODUCTION!)

## 📊 Database Schema

The application uses SQLite for local development (easily upgradeable to PostgreSQL for production) with the following main tables:

- **users**: User accounts with roles (admin, operator, security, viewer)
- **visitors**: Visitor management with check-in/check-out tracking
- **contractors**: Contractor information with permit and insurance tracking
- **waybills**: Waybill and shipment tracking for cocoa products
- **cctv_cameras**: CCTV camera monitoring with stream configuration
- **office_keys**: Office key management with issuance tracking
- **notifications**: System notifications and alerts

The database is automatically created on first run using SQLAlchemy ORM, and can be initialized with the provided `init_db_simple.py` script.

## 🔐 Security Features

- **JWT Authentication**: Secure token-based authentication with access and refresh tokens
- **Password Hashing**: Bcrypt for secure password storage
- **Role-Based Access Control**: Granular permissions (Admin, Operator, Security, Viewer)
- **Account Lockout**: Automatic account locking after 5 failed login attempts
- **CORS Protection**: Configurable CORS policies
- **Custom Exception Handling**: Centralized error handling with consistent responses
- **Environment Variables**: Sensitive data stored in environment variables
- **Stream Authentication**: Secure CCTV stream credential management

## 🎯 Key Features

### Visitor Management
- Visitor registration with photo capture
- Check-in/check-out with badge generation
- Host assignment and department tracking
- Automatic notifications for check-in/out events
- Visitor status tracking and expiry management

### Contractor Management
- Contractor registration with company information
- Permit tracking with expiry alerts
- Safety induction monitoring
- Insurance expiry management
- Full CRUD operations with status management

### Waybill Management
- Support for incoming, outgoing, and service waybills
- Container tracking for cocoa products (Cocoa Cake, Cocoa Butter, Cocoa Liquor)
- Room assignment (1-300) for storage management
- Document attachment support
- Automatic notifications on waybill creation

### CCTV Monitoring
- Real-time stream testing using OpenCV
- RTSP/HTTP stream support
- IP address and network configuration
- Maintenance scheduling and tracking
- Automatic notifications for camera issues
- Stream enable/disable functionality

### Key Management
- Office key tracking with room assignment
- Key issuance with holder information
- Overdue key tracking and alerts
- Lost key reporting
- Room type classification (admin, utility, storage)

### Reporting & Analytics
- Real-time dashboard statistics
- Comprehensive reports for all entities
- Date range filtering
- Export functionality
- Summary reports with current status

### Automatic Notifications
- Background notification system using threading
- Category-based organization (visitor, contractor, waybill, cctv, key, system)
- Priority levels (info, warning, error, critical)
- Action tracking and management
- Integration with all major service events

## 📈 Monitoring & Health Checks

All services include enhanced health check endpoints:

- `/health` - Basic health check
- `/health/detailed` - Detailed health with dependency status
- `/health/ready` - Readiness probe for Kubernetes
- `/health/live` - Liveness probe for Kubernetes

Service monitoring utilities are available in `shared/monitoring.py` for checking all services.

## 🌐 API Documentation

Once the services are running, access the interactive API documentation:

- **API Gateway**: http://localhost:8000/docs
- **Auth Service**: http://localhost:8001/docs
- **Visitor Service**: http://localhost:8002/docs
- **Contractor Service**: http://localhost:8003/docs
- **Waybill Service**: http://localhost:8004/docs
- **CCTV Service**: http://localhost:8005/docs
- **Keys Service**: http://localhost:8006/docs
- **Notification Service**: http://localhost:8007/docs
- **Reporting Service**: http://localhost:8008/docs

## 📝 Environment Variables

The system uses sensible defaults for local development. For production, you can set these environment variables:

- `DATABASE_URL`: Database connection string (defaults to SQLite for local)
- `SECRET_KEY`: JWT signing key (CHANGE IN PRODUCTION)
- `DEBUG`: Debug mode toggle (defaults to false)
- `ENVIRONMENT`: Environment name (defaults to development)
- `CORS_ORIGINS`: Allowed CORS origins (defaults to localhost:3000,3001)

For local development, no environment variables are required - the system will use SQLite and sensible defaults.

## 🚢 Deployment

### Local Deployment

The system is designed for local development without requiring Docker. Simply run:

```bash
python start.py              # Start all backend services in single window
# In a new terminal:
cd frontend && npm run dev   # Start frontend
```

### Service Ports

- API Gateway: 8000
- Auth Service: 8001
- Visitor Service: 8002
- Contractor Service: 8003
- Waybill Service: 8004
- CCTV Service: 8005
- Keys Service: 8006
- Notification Service: 8007
- Reporting Service: 8008
- Frontend: 3000

### Production Deployment

For production deployment, you can:
1. Use PostgreSQL instead of SQLite by setting `DATABASE_URL` environment variable
2. Add Redis for caching by setting `REDIS_URL` environment variable
3. Use process managers like supervisor or systemd to manage services
4. Configure nginx as a reverse proxy for the API gateway

## 🔧 Development

### Adding New Features

1. **Backend**: Add new endpoints to the appropriate service
2. **Frontend**: Create new components in `frontend/src/components/`
3. **Database**: Create migrations using Alembic
4. **Shared**: Add utilities to `backend/shared/`

### Code Organization

- **Shared Module**: Reusable code across all services
- **Error Handling**: Centralized exception handling in `error_handlers.py`
- **Monitoring**: Service health checks in `monitoring.py`
- **Notifications**: Automatic notification system in `notification_helper.py`
- **Stream Testing**: CCTV utilities in `stream_utils.py`

## 🗺️ Project Status

### Completed Features ✅
- Complete microservices architecture (9 services)
- JWT authentication with role-based access control
- Visitor management with check-in/check-out
- Contractor management with permit tracking
- Waybill management for shipments
- CCTV monitoring with real stream testing
- Key management with issuance tracking
- Automatic notification system
- Comprehensive reporting and analytics
- SQLite database with auto-creation
- Centralized error handling
- Service health monitoring
- Single-window service launcher
- Database initialization scripts

### Future Enhancements 🚧
- WebSocket real-time updates
- Advanced authentication (MFA)
- File upload handling
- Mobile application
- Advanced analytics with AI insights
- External system integrations
- Multi-language support

## 📄 License

Proprietary software for TF Commodities Ghana Ltd.

## 🆘 Support

For support or questions, refer to the system documentation or contact the development team.

---

**Built with ❤️ for TF Commodities Ghana Ltd Security Operations**
