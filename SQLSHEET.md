# CREATE TABLE

CREATE TABLE Enrollments (
StudentID INT NOT NULL,
CourseID INT NOT NULL,
PRIMARY KEY (StudentID, CourseID),
FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

# SELECT

# WHERE 1

SELECT FirstName, LastName FROM Students
WHERE LastName LIKE "L%"
ORDER BY FirstName ASC LIMIT 10;

# WHERE 2

SELECT COUNT(\*) FROM Students
WHERE Email LIKE "%example.com"

# WHERE 3

SELECT \* FROM classrooms
WHERE Capacity >= 30;

## -----------------------

# JOINS 1

SELECT
t.TeacherID,
t.FirstName,
t.LastName,
COUNT(c.CourseID) AS CourseCount
FROM teachers AS t
LEFT JOIN Courses AS c ON c.TeacherID = t.TeacherID
GROUP BY t.TeacherID, t.FirstName, t.LastName
ORDER BY CourseCount DESC;

# JOINS 2

SELECT
t.FirstName,
t.LastName,
c.CourseName,
c.TeacherID
FROM teachers AS t
LEFT JOIN Courses AS c ON c.TeacherID = t.TeacherID

# JOINS 3

SELECT
s.FirstName,
c.CourseName
FROM Enrollments e
JOIN Students s ON e.StudentID = s.StudentID
JOIN Courses c ON e.CourseID = c.CourseID;

# POKEMON DATABASE

-- 1️⃣ Droppa gamla tabeller
SET FOREIGN_KEY_CHECKS = 0;
DROP TABLE IF EXISTS BundleCards;
DROP TABLE IF EXISTS Bundles;
DROP TABLE IF EXISTS PokemonCards;
DROP TABLE IF EXISTS Series;
SET FOREIGN_KEY_CHECKS = 1;

-- 2️⃣ Skapa tabeller
CREATE TABLE PokemonCards (
cardID INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(64) NOT NULL,
rarity VARCHAR(32),
cardSpec VARCHAR(64)
);

CREATE TABLE Series (
seriesID INT AUTO_INCREMENT PRIMARY KEY,
seriesName VARCHAR(64) NOT NULL,
releaseDate DATE
);

CREATE TABLE Bundles (
bundleID INT AUTO_INCREMENT PRIMARY KEY,
bundleName VARCHAR(64) NOT NULL,
releaseDate DATE,
seriesID INT,
FOREIGN KEY (seriesID) REFERENCES Series(seriesID)
);

CREATE TABLE BundleCards (
bundleID INT,
cardID INT,
price INT DEFAULT 0,
isChaseCard BOOLEAN DEFAULT FALSE,
PRIMARY KEY (bundleID, cardID),
FOREIGN KEY (bundleID) REFERENCES Bundles(bundleID),
FOREIGN KEY (cardID) REFERENCES PokemonCards(cardID)
);

-- 3️⃣ Lägg in kort (utökat med fler populära kort)
INSERT INTO PokemonCards (name, rarity, cardSpec) VALUES
-- Base
('Pikachu', 'Common', NULL),
('Charizard', 'Ultra Rare', 'Holo Rare'),
('Bulbasaur', 'Common', NULL),
('Mewtwo', 'Rare', 'Holo Rare'),
('Gyarados', 'Rare Holo', NULL),
-- Shining Legends
('Lugia', 'Rare', 'Full Art'),
('Ho-Oh', 'Rare', 'Full Art'),
('Rayquaza', 'Rare', 'Full Art'),
('Gardevoir', 'Rare', NULL),
('Eevee', 'Common', NULL),
-- 151
('Charizard ex', 'Ultra Rare', 'Super Illustration Rare'),
('Blastoise ex', 'Rare Holo', NULL),
('Venusaur ex', 'Rare Holo', NULL),
('Zapdos ex', 'Super Rare', NULL),
('Alakazam ex', 'Super Rare', NULL),
('Pikachu V', 'Rare', 'Full Art'),
('Squirtle', 'Common', NULL),
('Charmander', 'Common', NULL),
('Gyarados V', 'Ultra Rare', NULL),
('Mewtwo VMAX', 'Ultra Rare', 'Rainbow Rare'),
('Snorlax V', 'Rare', NULL),
('Machamp', 'Rare', NULL),
('Arcanine', 'Rare Holo', NULL),
('Jolteon', 'Rare', NULL),
('Vaporeon', 'Rare', NULL),
('Flareon', 'Rare', NULL),
('Gengar V', 'Super Rare', NULL),
('Dragonite', 'Rare Holo', NULL),
-- Black Bolt
('Hydreigon', 'Ultra Rare', 'Full Art'),
('Zekrom', 'Rare Holo', NULL),
('Reshiram', 'Rare Holo', NULL),
('Kyurem', 'Super Rare', NULL),
('Garchomp', 'Rare', NULL),
-- Mega Evolution
('Mega Charizard X', 'Ultra Rare', 'Full Art'),
('Mega Blastoise', 'Rare Holo', NULL),
('Mega Venusaur', 'Rare Holo', NULL),
('Mega Gengar', 'Super Rare', NULL),
('Mega Mewtwo', 'Ultra Rare', 'Full Art'),
-- Extra moderna kort
('Lugia V', 'Ultra Rare', 'Full Art'),
('Ho-Oh V', 'Ultra Rare', 'Full Art'),
('Rayquaza V', 'Ultra Rare', 'Full Art'),
('Mew V', 'Ultra Rare', 'Rainbow Rare'),
('Zacian V', 'Ultra Rare', 'Full Art'),
('Zamazenta V', 'Ultra Rare', 'Full Art'),
('Charizard VMAX', 'Ultra Rare', 'Rainbow Rare');

-- 4️⃣ Lägg in serier
INSERT INTO Series (seriesName, releaseDate) VALUES
('Base Series', '1999-01-01'),
('Neo & e-Card Era', '2000-12-16'),
('Sword & Shield', '2020-02-07'),
('Scarlet & Violet', '2023-03-31');

-- 5️⃣ Lägg in bundles
INSERT INTO Bundles (bundleName, releaseDate, seriesID) VALUES
('Base Set', '1999-01-01', 1),
('Jungle', '1999-06-16', 1),
('Fossil', '1999-10-10', 1),
('Shining Legends', '2019-06-15', 3),
('151', '2023-09-22', 4),
('Black Bolt', '2025-07-18', 4),
('Mega Evolution', '2025-09-26', 4),
('Sword & Shield', '2020-02-07', 3),
('Rebel Clash', '2020-05-01', 3),
('Darkness Ablaze', '2020-08-14', 3),
('Vivid Voltage', '2020-11-13', 3),
('Chilling Reign', '2021-06-11', 3),
('Evolving Skies', '2021-08-27', 3),
('Celebrations', '2021-10-08', 3),
('Scarlet & Violet', '2023-03-31', 4),
('Paradox Rift', '2023-11-03', 4),
('Paldea Evolved', '2023-06-09', 4),
('Crown Zenith', '2023-01-20', 3);

-- 6️⃣ Lägg in BundleCards (exempel med priser och chase cards)
-- Base Set (bundleID=1)
INSERT INTO BundleCards (bundleID, cardID, price, isChaseCard) VALUES
(1, 1, 50, FALSE),
(1, 2, 200, TRUE),
(1, 3, 60, FALSE),
(1, 4, 150, TRUE),
(1, 5, 180, TRUE);

-- Shining Legends (bundleID=4)
INSERT INTO BundleCards (bundleID, cardID, price, isChaseCard) VALUES
(4, 6, 120, TRUE),
(4, 7, 115, TRUE),
(4, 8, 110, TRUE),
(4, 9, 50, FALSE),
(4, 10, 15, FALSE);

-- 151 (bundleID=5)
INSERT INTO BundleCards (bundleID, cardID, price, isChaseCard) VALUES
(5, 11, 266, TRUE),
(5, 12, 79, TRUE),
(5, 13, 75, TRUE),
(5, 14, 72, TRUE),
(5, 15, 52, TRUE),
(5, 16, 10, FALSE),
(5, 17, 8, FALSE),
(5, 18, 12, FALSE),
(5, 19, 9, FALSE),
(5, 20, 7, FALSE);

-- Black Bolt (bundleID=6)
INSERT INTO BundleCards (bundleID, cardID, price, isChaseCard) VALUES
(6, 31, 200, TRUE),
(6, 32, 180, TRUE),
(6, 33, 170, TRUE),
(6, 34, 150, TRUE),
(6, 35, 100, FALSE);

-- Mega Evolution (bundleID=7)
INSERT INTO BundleCards (bundleID, cardID, price, isChaseCard) VALUES
(7, 36, 300, TRUE),
(7, 37, 250, TRUE),
(7, 38, 240, TRUE),
(7, 39, 200, TRUE),
(7, 40, 350, TRUE);

-- 7️⃣ Kontrollera data
SELECT b.bundleName, p.name, p.rarity, p.cardSpec, bc.price, bc.isChaseCard
FROM BundleCards bc
JOIN Bundles b ON bc.bundleID = b.bundleID
JOIN PokemonCards p ON bc.cardID = p.cardID
ORDER BY b.bundleID, bc.price DESC;
