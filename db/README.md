
# کدهای دیتابیس


## دیتابیس فضانوردان


```SQL
-- =========================================================
-- NASA Sample Database
-- A small, teaching-friendly database covering:
--   spacecraft, missions, astronauts, and mission crew assignments
-- Built from real, publicly known NASA mission facts.
-- Good for practicing: SELECT, JOIN, GROUP BY, aggregate functions,
-- subqueries, and foreign key constraints.
-- =========================================================

DROP DATABASE IF EXISTS nasa_sample;
CREATE DATABASE nasa_sample;
USE nasa_sample;

-- ---------------------------------------------------------
-- Table: spacecraft
-- ---------------------------------------------------------
CREATE TABLE spacecraft (
    spacecraft_id   INT PRIMARY KEY AUTO_INCREMENT,
    name            VARCHAR(100) NOT NULL,
    program         VARCHAR(50)  NOT NULL,     -- Mercury, Gemini, Apollo, Shuttle
    crew_capacity   INT          NOT NULL,
    first_flight    DATE
);

-- ---------------------------------------------------------
-- Table: astronauts
-- ---------------------------------------------------------
CREATE TABLE astronauts (
    astronaut_id    INT PRIMARY KEY AUTO_INCREMENT,
    full_name       VARCHAR(100) NOT NULL,
    birth_year      INT,
    astronaut_group INT,          -- NASA astronaut selection group number
    military        BOOLEAN DEFAULT TRUE
);

-- ---------------------------------------------------------
-- Table: missions
-- ---------------------------------------------------------
CREATE TABLE missions (
    mission_id      INT PRIMARY KEY AUTO_INCREMENT,
    mission_name    VARCHAR(100) NOT NULL,
    spacecraft_id   INT,
    launch_date     DATE,
    duration_hours  INT,
    outcome         VARCHAR(50),   -- Success, Partial Success, Failure
    FOREIGN KEY (spacecraft_id) REFERENCES spacecraft(spacecraft_id)
);

-- ---------------------------------------------------------
-- Table: mission_crew (many-to-many: missions <-> astronauts)
-- ---------------------------------------------------------
CREATE TABLE mission_crew (
    mission_id      INT,
    astronaut_id    INT,
    role            VARCHAR(50),   -- Commander, Pilot, Mission Specialist, etc.
    PRIMARY KEY (mission_id, astronaut_id),
    FOREIGN KEY (mission_id) REFERENCES missions(mission_id),
    FOREIGN KEY (astronaut_id) REFERENCES astronauts(astronaut_id)
);

-- ---------------------------------------------------------
-- Sample data: spacecraft
-- ---------------------------------------------------------
INSERT INTO spacecraft (name, program, crew_capacity, first_flight) VALUES
('Friendship 7',      'Mercury', 1, '1962-02-20'),
('Gemini 4',          'Gemini',  2, '1965-06-03'),
('Apollo 11 CSM/LM',  'Apollo',  3, '1969-07-16'),
('Apollo 13 CSM/LM',  'Apollo',  3, '1970-04-11'),
('Columbia',          'Shuttle', 7, '1981-04-12'),
('Challenger',        'Shuttle', 7, '1983-04-04'),
('Discovery',         'Shuttle', 7, '1984-08-30');

-- ---------------------------------------------------------
-- Sample data: astronauts
-- ---------------------------------------------------------
INSERT INTO astronauts (full_name, birth_year, astronaut_group, military) VALUES
('John Glenn',        1921, 1, TRUE),
('Edward White',      1930, 2, TRUE),
('James McDivitt',    1929, 2, TRUE),
('Neil Armstrong',    1930, 2, TRUE),
('Buzz Aldrin',       1930, 3, TRUE),
('Michael Collins',   1930, 3, TRUE),
('James Lovell',      1928, 2, TRUE),
('Jack Swigert',      1931, 5, TRUE),
('Fred Haise',        1933, 5, FALSE),
('John Young',        1930, 2, TRUE),
('Sally Ride',        1951, 8, FALSE);

-- ---------------------------------------------------------
-- Sample data: missions
-- ---------------------------------------------------------
INSERT INTO missions (mission_name, spacecraft_id, launch_date, duration_hours, outcome) VALUES
('Mercury-Atlas 6',   1, '1962-02-20', 5,    'Success'),
('Gemini 4',          2, '1965-06-03', 98,   'Success'),
('Apollo 11',         3, '1969-07-16', 195,  'Success'),
('Apollo 13',         4, '1970-04-11', 143,  'Partial Success'),
('STS-1',             5, '1981-04-12', 54,   'Success'),
('STS-7',             6, '1983-06-18', 146,  'Success');

-- ---------------------------------------------------------
-- Sample data: mission_crew
-- ---------------------------------------------------------
INSERT INTO mission_crew (mission_id, astronaut_id, role) VALUES
(1, 1, 'Pilot'),
(2, 2, 'Pilot'),
(2, 3, 'Commander'),
(3, 4, 'Commander'),
(3, 5, 'Lunar Module Pilot'),
(3, 6, 'Command Module Pilot'),
(4, 7, 'Commander'),
(4, 8, 'Command Module Pilot'),
(4, 9, 'Lunar Module Pilot'),
(5, 10, 'Commander'),
(6, 11, 'Mission Specialist');

```