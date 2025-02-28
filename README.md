# Mindful Performance Backend

Backend service for the Mindful Performance platform that connects athletes with mental performance coaches.

## Overview

This Node.js backend provides RESTful API services for the Mindful Performance application, handling user data management, authentication, session booking, and video conferencing integration.

## Features

- **User Management**: Registration and authentication for athletes and coaches
- **Profile Storage**: Secure storage of user profiles and preferences
- **Scheduling System**: Management of coach availability and session bookings
- **Zoom Integration**: Automatic creation of Zoom meetings for coaching sessions
- **File Storage**: Firebase storage integration for profile images
- **Agora Integration**: Support for real-time communication using Agora

## Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB (Atlas)
- **Storage**: Firebase Storage
- **Authentication**: Custom implementation
- **Video Conferencing**: Zoom API
- **Real-time Communication**: Agora SDK

## Project Structure

```
backend/
├── server.js                # Main server application (primary API endpoints)
├── agora-server.js          # Agora server for real-time communication
├── .env                     # Environment variables
├── firebase-admin-json.json # Firebase service account credentials
├── package.json             # Dependencies and scripts
└── .gitignore               # Git ignore file
```

## Getting Started

### Prerequisites

- Node.js (v14 or newer)
- MongoDB Atlas account
- Firebase project with Storage enabled
- Zoom Developer account
- Agora Developer account

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/mp-backend.git
   cd mp-backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file with the following variables:
   ```
   ZOOM_API_KEY=your_zoom_api_key
   ZOOM_API_SECRET=your_zoom_api_secret
   TOKEN=your_zoom_jwt_token
   PUBLIC_AGORA_APP_ID=your_agora_app_id
   ```

4. Ensure you have the Firebase service account JSON file named `firebase-admin-json.json` in the root directory.

### Running the Server

```bash
# Run both servers (main API and Agora)
npm run dev

# Run only main API server
npm start

# Run only Agora server
npm run start-agora
```

The main API server runs on port 5001 by default, and the Agora server runs on port 4321.

## API Endpoints

### Athletes

- **POST /api/athletes**: Register a new athlete
- **POST /api/athletes/login**: Authenticate an athlete
- **PUT /api/athletes/:id**: Update athlete profile
- **GET /api/athletes/:id/bookings**: Get athlete's bookings

### Coaches

- **POST /api/coaches**: Register a new coach
- **POST /api/coaches/login**: Authenticate a coach
- **PUT /api/coaches/:id**: Update coach profile
- **GET /api/coaches**: Get all coaches
- **GET /api/coaches/:id/sessions**: Get coach's sessions
- **PUT /api/coaches/:id/availability**: Update coach's availability
- **GET /api/coaches/:id/availability**: Get coach's availability

### Agora

- **POST /join-channel**: Join an Agora channel for real-time communication

## Database Models

### Athlete Schema

```javascript
{
  fullName: String,
  email: { type: String, unique: true },
  password: String,
  sports: String,
  age: String,
  gender: String,
  playingTime: String,
  performanceAnxiety: String,
  injuries: String,
  ableToBalance: String,
  coachingAspect: String,
  profileImage: String,
  bookings: [
    {
      coachId: ObjectId,
      coachName: String,
      coachEmail: String,
      zoomMeetingId: String,
      zoomJoinUrl: String,
      bookingTimestamp: Date,
      bookingDate: String,
      startTime: String,
      endTime: String,
    }
  ]
}
```

### Coach Schema

```javascript
{
  fullName: String,
  email: { type: String, unique: true },
  password: String,
  profileImage: String,
  coaching: String,
  expertise: String,
  ageGroup: String,
  certifications: String,
  goodDealing: String,
  personalBio: String,
  previousCoaching: String,
  availableTimings: [
    {
      date: String,
      times: [String]
    }
  ],
  bookings: [
    {
      athleteId: ObjectId,
      zoomMeetingId: String,
      zoomJoinUrl: String,
      bookingTimestamp: Date,
      bookingDate: String,
      startTime: String,
      endTime: String,
    }
  ]
}
```

## External Integrations

### Zoom

The backend integrates with Zoom to create meeting links for coaching sessions. When a booking is made, a Zoom meeting is automatically created using the Zoom API.

### Firebase Storage

Profile images are stored in Firebase Storage. The backend handles upload, retrieval, and URL management for these images.

### Agora

Real-time communication is facilitated through Agora. The `agora-server.js` provides endpoints to join channels and manage communication.

## Deployment

The backend is designed to be deployed to any Node.js-compatible hosting service (e.g., Railway, Heroku, AWS, etc.). Set the environment variables according to the hosting platform's specifications.

```bash
# Production start
npm start
```

## Security Notes

- The application currently uses a simple authentication system. For production, implement a more robust authentication system with JWT or similar.
- Ensure MongoDB connection strings, Zoom credentials, and Firebase credentials are kept secure and not committed to the repository.
- Consider implementing input validation and sanitization for all endpoints.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License.
