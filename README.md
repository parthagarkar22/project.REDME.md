-- =========================
-- PART 1: TABLE CREATION
-- =========================

-- Users Table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Events Table
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    description TEXT,
    date DATE NOT NULL,
    city VARCHAR(100),
    created_by INT NOT NULL,
    CONSTRAINT fk_event_creator FOREIGN KEY (created_by)
        REFERENCES users (id)
        ON DELETE CASCADE
);

-- RSVPs Table
CREATE TABLE rsvps (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    event_id INT NOT NULL,
    status VARCHAR(10) CHECK (status IN ('Yes', 'No', 'Maybe')),
    CONSTRAINT fk_rsvp_user FOREIGN KEY (user_id)
        REFERENCES users (id)
        ON DELETE CASCADE,
    CONSTRAINT fk_rsvp_event FOREIGN KEY (event_id)
        REFERENCES events (id)
        ON DELETE CASCADE,
    CONSTRAINT unique_user_event UNIQUE (user_id, event_id) -- Prevent duplicate RSVPs
);

-- =========================
-- PART 2: SAMPLE DATA
-- =========================

-- Insert Users (10 users)
INSERT INTO users (name, email) VALUES
('Alice Johnson', 'alice@example.com'),
('Bob Smith', 'bob@example.com'),
('Charlie Brown', 'charlie@example.com'),
('Diana Ross', 'diana@example.com'),
('Ethan Hunt', 'ethan@example.com'),
('Fiona Green', 'fiona@example.com'),
('George White', 'george@example.com'),
('Hannah Black', 'hannah@example.com'),
('Ian Gray', 'ian@example.com'),
('Julia Adams', 'julia@example.com');

-- Insert Events (5 events, each linked to a user)
INSERT INTO events (title, description, date, city, created_by) VALUES
('Tech Conference 2025', 'A conference about the latest in technology.', '2025-09-10', 'San Francisco', 1),
('Music Festival', 'A fun outdoor music event.', '2025-09-15', 'Los Angeles', 2),
('Startup Pitch Night', 'Local startups pitch their ideas.', '2025-09-20', 'New York', 3),
('Art Exhibition', 'Modern art gallery opening.', '2025-09-25', 'Chicago', 4),
('Marathon 2025', 'Annual city marathon.', '2025-09-30', 'Boston', 5);

-- Insert RSVPs (20 RSVPs across events)
INSERT INTO rsvps (user_id, event_id, status) VALUES
(1, 1, 'Yes'),
(2, 1, 'Maybe'),
(3, 1, 'Yes'),
(4, 1, 'No'),

(5, 2, 'Yes'),
(6, 2, 'Yes'),
(7, 2, 'Maybe'),
(8, 2, 'No'),

(9, 3, 'Yes'),
(10, 3, 'Maybe'),
(1, 3, 'Yes'),
(2, 3, 'No'),

(3, 4, 'Yes'),
(4, 4, 'Maybe'),
(5, 4, 'Yes'),
(6, 4, 'No'),

(7, 5, 'Yes'),
(8, 5, 'Yes'),
(9, 5, 'Maybe'),
(10, 5, 'Yes');
