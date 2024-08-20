# Hotel Booking API

This is a RESTful API for a hotel booking system built with Node.js, Express, and PostgreSQL. It allows users to view hotel details, room information, and create new hotels and rooms with image upload functionality.
## [**Frontend Link🔗**](https://github.com/bishworup002/hotel-frontend)

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Endpoints](#api-endpoints)
- [Database Schema](#database-schema)
- [Sample Data](#sample-data)
- [Contributing](#contributing)
- [License](#license)

## Features

- Retrieve hotel details
- List rooms for a specific hotel
- Create new hotels with multiple image uploads
- Add new rooms with image upload
- PostgreSQL database for data persistence
- Image storage in local 'uploads' folder

## Prerequisites

- Node.js (v14 or later)
- npm (v6 or later)
- PostgreSQL (v12 or later)

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/bishworup002/hotel-backend.git
   cd hotel-backend
   ```

2. Install dependencies:
   ```
   npm install
   ```

## Configuration

1. Create a `.env` file in the root directory with the following content:
   ```
   DB_USER=your_username
   DB_HOST=your_host
   DB_NAME=your_database_name
   DB_PASSWORD=your_password
   DB_PORT=5432
   PORT=5000
   ```

2. Replace the placeholder values with your actual PostgreSQL credentials and desired port number.

## Running the Application

1. Start the server:
   ```
   node server.js
   ```

2. The API will be available at `http://localhost:5000` (or the port you specified in the `.env` file).

## API Endpoints

- `GET /api/hotel/:slug` - Get hotel details
- `POST /api/hotel` - Create a new hotel (with image upload)
- `GET /api/hotel/:slug/rooms` - Get rooms for a specific hotel
- `POST /api/room` - Create a new room (with image upload)

## Database Schema

The application uses the following database schema:

### Hotels Table
```sql
CREATE TABLE hotels (
  id SERIAL PRIMARY KEY,
  slug VARCHAR(255) UNIQUE NOT NULL,
  title VARCHAR(255) NOT NULL,
  description TEXT,
  guest_count INTEGER,
  bedroom_count INTEGER,
  bathroom_count INTEGER,
  amenities TEXT[],
  host_name VARCHAR(255),
  host_image VARCHAR(255),
  address TEXT,
  latitude DECIMAL(10, 8),
  longitude DECIMAL(11, 8)
);
```

### Hotel Images Table
```sql
CREATE TABLE hotel_images (
  id SERIAL PRIMARY KEY,
  hotel_id INTEGER REFERENCES hotels(id),
  image_url VARCHAR(255)
);
```

### Rooms Table
```sql
CREATE TABLE rooms (
  id SERIAL PRIMARY KEY,
  hotel_slug VARCHAR(255) REFERENCES hotels(slug),
  room_slug VARCHAR(255),
  room_image VARCHAR(255),
  room_title VARCHAR(255),
  bedroom_count INTEGER
);
```




## Sample Data

To populate your database with sample data, you can use the following SQL commands:

```sql
-- Sample data for hotels table
INSERT INTO hotels (slug, title, description, guest_count, bedroom_count, bathroom_count, amenities, host_name, host_image, address, latitude, longitude)
VALUES 
('seaside-villa', 'Seaside Villa', 
'Welcome to the Seaside Villa, a stunning property overlooking the magnificent ocean. Enjoy breathtaking views from every room and relax in the comfort of a fully equipped home. Perfect for families or groups, this villa offers all the amenities you need for a memorable stay. Conveniently located near popular attractions and beaches, you will have plenty to explore during your visit. The spacious outdoor area and private pool make it an ideal getaway.', 
6, 3, 2, ARRAY['WiFi', 'Pool', 'Gym','Beach Access','Kitchen','TV','Washer','Smoke alarm','Refrigerator'], 
'John Doe', '/uploads/host-john-doe.jpg', '123 Coastal Road, Beachtown', 34.052235, -118.243683),

('mountain-lodge', 'Mountain Lodge', 
'Experience the charm of our cozy Mountain Lodge, nestled in the heart of the breathtaking mountains. This retreat offers a perfect blend of rustic charm and modern comfort. Enjoy hiking trails right at your doorstep and cozy up by the fireplace after a day of adventure. With ample space and amenities, it’s perfect for families or groups looking for a serene escape in nature. The lodge’s unique location provides both tranquility and accessibility to nearby attractions.', 
8, 4, 3, ARRAY['WiFi', 'Fireplace', 'Gym','Hiking Trails','Beach Access','Kitchen','TV','Washer','Smoke alarm','Refrigerator'], 
'Jane Smith', '/uploads/host-jane-smith.jpg', '456 Mountain Pass, Alpineville', 39.739236, -104.990251),

('city-loft', 'Downtown City Loft', 
'Discover the modern comfort of our Downtown City Loft, situated in the bustling heart of the city. This stylish loft is ideal for solo travelers or couples looking for an urban retreat. With contemporary furnishings and a rooftop terrace offering stunning city views, you’ll feel right at home. The loft’s central location provides easy access to the city’s top attractions, dining, and shopping districts, making it the perfect base for your city adventure.', 
2, 1, 1, ARRAY['Wi-Fi', 'Gym', 'Rooftop Terrace'], 
'Mike Johnson', '/uploads/host-mike-johnson.jpg', '789 Main Street, Metropolis', 40.712776, -74.005974);


-- Sample data for hotel_images table
INSERT INTO hotel_images (hotel_id, image_url)
VALUES 
(1, '/uploads/hotel12.jpg'),
(1, '/uploads/hotel11.jpg'),
(1, '/uploads/hotel13.jpg'),
(1, '/uploads/hotel14.jpg'),
(1, '/uploads/hotel15.jpg'),
(1, '/uploads/hotel16.jpg'),
(1, '/uploads/hotel7.jpg'),
(1, '/uploads/hotel8.jpg'),
(1, '/uploads/hotel9.jpg'),
(1, '/uploads/hotel10.jpg'),
(1, '/uploads/hotel11.jpg'),
(1, '/uploads/hotel14.jpg'),
(2, '/uploads/hotel11.jpg'),
(2, '/uploads/hotel12.jpg'),
(2, '/uploads/hotel13.jpg'),
(2, '/uploads/hotel14.jpg'),
(2, '/uploads/hotel15.jpg'),
(3, '/uploads/hotel11.jpg'),
(3, '/uploads/hotel1.jpg'),
(3, '/uploads/hotel11.jpg'),
(3, '/uploads/hotel12.jpg'),
(3, '/uploads/hotel3.jpg');

-- Sample data for rooms table
INSERT INTO rooms (hotel_slug, room_slug, room_image, room_title, bedroom_count)
VALUES 
('seaside-villa', 'master-suite', '/uploads/bedroom5.jpg', 'Master Suite', 1),
('seaside-villa', 'ocean-view-room', '/uploads/bedroom2.jpg', 'Ocean View Room', 1),
('seaside-villa', 'queen-room', '/uploads/bedroom3.jpg', 'Queen Room', 1),
('seaside-villa', 'garden-room', '/uploads/bedroom4.jpg', 'Garden Room', 1),
('mountain-lodge', 'pine-suite', '/uploads/bedroom2.jpg', 'Pine Suite', 1),
('mountain-lodge', 'elk-room', '/uploads/bedroom4.jpg', 'Elk Room', 1),
('mountain-lodge', 'bear-den', '/uploads/bedroom1.jpg', 'Bear Den', 2),
('city-loft', 'urban-suite', '/uploads/bedroom5.jpg', 'Urban Suite', 1);
```

To use this sample data:

1. Make sure your PostgreSQL database is running and you're connected to the correct database.
2. Copy the SQL commands above.
3. Paste these commands into your PostgreSQL client (e.g., psql command line, pgAdmin, or any other database management tool you're using).
4. Execute the commands to insert the data into your tables.

This sample data provides a good starting point for testing your API. It includes:
- Three hotels with varying characteristics
- Multiple images for each hotel
- Several rooms for each hotel

After inserting this data, you can test your API endpoints to retrieve hotel and room information.


## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT Licens