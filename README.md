# MERN Task Management Application

A full-stack task management application built with MongoDB, Express, React, and Node.js. Features authentication, role-based access control, CRUD operations, dark mode, and pagination.

## Features

- ✅ User authentication (Sign Up / Sign In)
- ✅ JWT-based authentication
- ✅ Role-based access control (Admin / User)
- ✅ CRUD operations for tasks
- ✅ Task status management (Pending / Completed)
- ✅ Dark/Light mode toggle (persisted in localStorage)
- ✅ Responsive Material UI design
- ✅ Pagination for task lists
- ✅ Protected routes
- ✅ Admin-only delete functionality

## Tech Stack

### Frontend
- React 18
- Vite
- Material UI (MUI)
- React Router
- Axios
- Context API

### Backend
- Node.js
- Express
- MongoDB
- Mongoose
- JWT (jsonwebtoken)
- bcryptjs

## Project Structure

```
.
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── context/
│   │   ├── pages/
│   │   ├── utils/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
├── backend/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── scripts/
│   ├── server.js
│   └── package.json
├── README.md
└── .gitignore
```

## Getting Started

### Prerequisites

- Node.js (v16 or higher)
- MongoDB (local installation or MongoDB Atlas)
- npm or yarn

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd ciphrix
   ```

2. **Backend Setup**
   ```bash
   cd backend
   npm install
   ```

3. **Create backend environment file**
   ```bash
   # Create .env file in backend directory
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/taskmanagement
   JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
   NODE_ENV=development
   ```

4. **Frontend Setup**
   ```bash
   cd ../frontend
   npm install
   ```

5. **Create frontend environment file (optional)**
   ```bash
   # Create .env file in frontend directory
   VITE_API_URL=http://localhost:5000/api
   ```

### Running the Application

1. **Start MongoDB**
   - Make sure MongoDB is running on your system
   - Default connection: `mongodb://localhost:27017`

2. **Start Backend Server**
   ```bash
   cd backend
   npm run dev
   ```
   Server will run on `http://localhost:5000`

3. **Start Frontend Development Server**
   ```bash
   cd frontend
   npm run dev
   ```
   Frontend will run on `http://localhost:3000`

4. **Open your browser**
   Navigate to `http://localhost:3000`

### Seeding Admin User (Optional)

To create a default admin user:

```bash
cd backend
npm run seed
```

This will create an admin user with:
- Email: `admin@example.com`
- Password: `admin123`
- Role: `admin`

## API Endpoints

### Authentication
- `POST /api/auth/signup` - Register a new user
- `POST /api/auth/signin` - Sign in user

### Tasks
- `GET /api/tasks?page=1&limit=10` - Get all tasks (paginated)
- `POST /api/tasks` - Create a new task
- `PUT /api/tasks/:id` - Update a task
- `DELETE /api/tasks/:id` - Delete a task (Admin only)

## User Roles

- **User**: Can create, view, and edit their own tasks
- **Admin**: Can create, view, edit, and delete all tasks

## Default Credentials

After seeding:
- **Admin**: admin@example.com / admin123
- **Regular User**: Sign up through the UI

## Features in Detail

### Authentication
- JWT tokens stored in localStorage
- Automatic token refresh on API calls
- Protected routes with PrivateRoute component

### Task Management
- Create tasks with title, description, and status
- Edit existing tasks
- Filter tasks by status (All, Pending, Completed)
- Pagination (10 tasks per page)
- Admin can see all users' tasks
- Regular users see only their own tasks

### Dark Mode
- Toggle between light and dark themes
- Preference saved in localStorage
- Applies to all pages

## Development

### Backend Scripts
- `npm start` - Start production server
- `npm run dev` - Start development server with nodemon
- `npm run seed` - Seed admin user

### Frontend Scripts
- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## Environment Variables

### Backend (.env)
```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/taskmanagement
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
NODE_ENV=development
```

### Frontend (.env)
```
VITE_API_URL=http://localhost:5000/api
```

## Troubleshooting

1. **MongoDB Connection Error**
   - Ensure MongoDB is running
   - Check MONGODB_URI in backend/.env

2. **CORS Issues**
   - Backend CORS is configured for localhost:3000
   - Adjust if using different ports

3. **Token Expiration**
   - Tokens expire after 30 days
   - Sign out and sign in again if token expires

## Deployment

This application can be deployed to:
- **Frontend**: Vercel (recommended)
- **Backend**: Render (recommended)
- **Database**: MongoDB Atlas (required for production)

See [DEPLOYMENT.md](./DEPLOYMENT.md) for detailed step-by-step deployment instructions.

## License

ISC

## Author

Built with ❤️ using MERN stack

