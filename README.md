frontend/
│
├── public/                   # 정적 파일
│   ├── index.html            # 메인 HTML 파일
│
├── src/                      # React 소스 파일
│   ├── components/           # 컴포넌트 디렉토리
│   │   ├── Reservation.js    # 예약 컴포넌트
│   │   ├── User.js           # 사용자 컴포넌트
│   │   └── Feedback.js       # 피드백 컴포넌트
│   │
│   ├── pages/                # 페이지 디렉토리
│   │   ├── Home.js           # 홈 페이지
│   │   ├── Reservations.js   # 예약 페이지
│   │   └── Users.js          # 사용자 페이지
│   │
│   ├── App.js                # 메인 앱 컴포넌트
│   ├── index.js              # 엔트리 파일
│   └── api.js                # API 호출 유틸리티
│
└── package.json              # 프론트엔드 패키지
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Exhibition Management System</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter as Router } from 'react-router-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById('root')
);
// src/App.js
import React from 'react';
import { Route, Routes } from 'react-router-dom';
import Home from './pages/Home';
import Reservations from './pages/Reservations';
import Users from './pages/Users';

function App() {
  return (
    <div className="App">
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/reservations" element={<Reservations />} />
        <Route path="/users" element={<Users />} />
      </Routes>
    </div>
  );
}

export default App;
// src/api.js
import axios from 'axios';

const API_BASE_URL = 'http://localhost:5000/api'; // 백엔드 API URL

export const getReservations = () => axios.get(`${API_BASE_URL}/reservations`);
export const createReservation = (reservation) => axios.post(`${API_BASE_URL}/reservations`, reservation);
export const getUsers = () => axios.get(`${API_BASE_URL}/users`);
export const createUser = (user) => axios.post(`${API_BASE_URL}/users`, user);
// src/pages/Reservations.js
import React, { useState, useEffect } from 'react';
import { getReservations, createReservation } from '../api';

function Reservations() {
  const [reservations, setReservations] = useState([]);
  const [newReservation, setNewReservation] = useState({
    userId: '',
    exhibitionId: '',
    reservationDate: '',
  });

  useEffect(() => {
    getReservations().then(response => setReservations(response.data));
  }, []);

  const handleChange = (e) => {
    setNewReservation({
      ...newReservation,
      [e.target.name]: e.target.value,
    });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    createReservation(newReservation).then(response => {
      setReservations([...reservations, response.data]);
    });
  };

  return (
    <div>
      <h2>예약 관리</h2>
      <form onSubmit={handleSubmit}>
        <input type="text" name="userId" placeholder="사용자 ID" value={newReservation.userId} onChange={handleChange} />
        <input type="text" name="exhibitionId" placeholder="전시 ID" value={newReservation.exhibitionId} onChange={handleChange} />
        <input type="date" name="reservationDate" value={newReservation.reservationDate} onChange={handleChange} />
        <button type="submit">예약 추가</button>
      </form>
      <ul>
        {reservations.map((reservation) => (
          <li key={reservation._id}>{reservation.reservationDate}</li>
        ))}
      </ul>
    </div>
  );
}

export default Reservations;
// src/pages/Users.js
import React, { useState, useEffect } from 'react';
import { getUsers, createUser } from '../api';

function Users() {
  const [users, setUsers] = useState([]);
  const [newUser, setNewUser] = useState({
    name: '',
    email: '',
  });

  useEffect(() => {
    getUsers().then(response => setUsers(response.data));
  }, []);

  const handleChange = (e) => {
    setNewUser({
      ...newUser,
      [e.target.name]: e.target.value,
    });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    createUser(newUser).then(response => {
      setUsers([...users, response.data]);
    });
  };

  return (
    <div>
      <h2>사용자 관리</h2>
      <form onSubmit={handleSubmit}>
        <input type="text" name="name" placeholder="이름" value={newUser.name} onChange={handleChange} />
        <input type="email" name="email" placeholder="이메일" value={newUser.email} onChange={handleChange} />
        <button type="submit">사용자 추가</button>
      </form>
      <ul>
        {users.map((user) => (
          <li key={user._id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default Users;
/* src/index.css */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

nav ul {
  list-style: none;
  padding: 0;
}

nav ul li {
  display: inline;
  margin-right: 10px;
}

nav ul li a {
  text-decoration: none;
  color: blue;
}
{
  "name": "frontend",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "axios": "^1.4.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.11.2"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1",
    "react-router-dom": "^6.11.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
cd frontend
npm install react-scripts
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "^5.0.1",  // Ensure this line is present
    "react-router-dom": "^6.11.0"
  }
}
npm install
cd frontend
rm -rf node_modules
rm package-lock.json
npm cache clean --force
npm install
npm run build
