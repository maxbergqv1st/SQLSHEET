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

CREATE TABLE PokemonCards (
cardID INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(64) NOT NULL,
rarity VARCHAR(32), -- Common, Rare, Ultra Rare osv
cardSpec VARCHAR(64) -- Super Illustration Rare, Full Art, Secret Rare osv
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

INSERT INTO PokemonCards (name, rarity, cardSpec) VALUES
('Charizard ex', 'Ultra Rare', 'Super Illustration Rare'),
('Blastoise ex', 'Rare Holo', NULL),
('Venusaur ex', 'Rare Holo', NULL),
('Zapdos ex', 'Super Rare', NULL),
('Alakazam ex', 'Super Rare', NULL),
('Pikachu V', 'Rare', 'Full Art'),
('Bulbasaur', 'Common', NULL),
('Squirtle', 'Common', NULL),
('Charmander', 'Common', NULL),
('Eevee', 'Common', NULL),
('Gyarados V', 'Ultra Rare', NULL),
('Mewtwo VMAX', 'Ultra Rare', 'Rainbow Rare'),
('Snorlax V', 'Rare', NULL),
('Machamp', 'Rare', NULL),
('Arcanine', 'Rare Holo', NULL),
('Jolteon', 'Rare', NULL),
('Vaporeon', 'Rare', NULL),
('Flareon', 'Rare', NULL),
('Gengar V', 'Super Rare', NULL),
('Dragonite', 'Rare Holo', NULL);

INSERT INTO Series (seriesName, releaseDate) VALUES
('Base Series', '1999-01-01'),
('Sword & Shield', '2020-02-07'),
('Scarlet & Violet', '2023-03-31');

INSERT INTO Bundles (bundleName, releaseDate, seriesID) VALUES
('Base Set', '1999-01-01', 1),
('Shining Legends', '2019-06-15', 2),
('151', '2023-09-22', 3),
('Black Bolt', '2025-07-18', 3),
('Mega Evolution', '2025-09-26', 3);

-- Bundle ID 3 = '151'
INSERT INTO BundleCards (bundleID, cardID, price, isChaseCard) VALUES
(3, 1, 266, TRUE), -- Charizard ex
(3, 2, 79, TRUE), -- Blastoise ex
(3, 3, 75, TRUE), -- Venusaur ex
(3, 4, 72, TRUE), -- Zapdos ex
(3, 5, 52, TRUE), -- Alakazam ex
(3, 6, 10, FALSE), -- Pikachu V
(3, 7, 8, FALSE), -- Bulbasaur
(3, 8, 12, FALSE), -- Squirtle
(3, 9, 9, FALSE), -- Charmander
(3, 10, 7, FALSE), -- Eevee
(3, 11, 65, FALSE), -- Gyarados V
(3, 12, 48, FALSE), -- Mewtwo VMAX
(3, 13, 40, FALSE), -- Snorlax V
(3, 14, 35, FALSE), -- Machamp
(3, 15, 30, FALSE), -- Arcanine
(3, 16, 28, FALSE), -- Jolteon
(3, 17, 25, FALSE), -- Vaporeon
(3, 18, 22, FALSE), -- Flareon
(3, 19, 20, FALSE), -- Gengar V
(3, 20, 18, FALSE); -- Dragonite

SELECT
b.bundleName,
p.name,
p.rarity,
p.cardSpec,
bc.price,
bc.isChaseCard
FROM BundleCards bc
JOIN Bundles b ON bc.bundleID = b.bundleID
JOIN PokemonCards p ON bc.cardID = p.cardID
WHERE b.bundleName = '151'
ORDER BY bc.price DESC;
