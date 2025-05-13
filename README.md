# lab3-database
scripts lab3 

-- Tabla Estudiante
CREATE TABLE Estudiante (
    codigo VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL
);

-- Tabla Correo
CREATE TABLE Correo (
    codigoEstudiante VARCHAR(20) REFERENCES Estudiante(codigo) ON DELETE CASCADE,
    correo VARCHAR(100) NOT NULL,
    PRIMARY KEY (codigoEstudiante, correo)
);

-- Tabla Asignatura
CREATE TABLE Asignatura (
    codigo VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    horas INTEGER NOT NULL CHECK (horas > 0)
);

-- Tabla Profesor
CREATE TABLE Profesor (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL
);

-- Tabla Curso
CREATE TABLE Curso (
    id SERIAL PRIMARY KEY,
    fecha DATE NOT NULL,
    codigoAsignatura VARCHAR(20) REFERENCES Asignatura(codigo) ON DELETE CASCADE,
    profesorId INTEGER REFERENCES Profesor(id) ON DELETE SET NULL
);

-- Tabla Aula
CREATE TABLE Aula (
    numero VARCHAR(20) PRIMARY KEY,
    capacidad INTEGER NOT NULL CHECK (capacidad > 0),
    edificio VARCHAR(50) NOT NULL,
    piso INTEGER NOT NULL,
    etiqueta VARCHAR(50)
);

-- Tabla CursoAula
CREATE TABLE CursoAula (
    id SERIAL PRIMARY KEY,
    idCurso INTEGER REFERENCES Curso(id) ON DELETE CASCADE,
    numeroAula VARCHAR(20) REFERENCES Aula(numero) ON DELETE SET NULL
);

-- Tabla EstudianteCurso
CREATE TABLE EstudianteCurso (
    codigoEstudiante VARCHAR(20) REFERENCES Estudiante(codigo) ON DELETE CASCADE,
    idCurso INTEGER REFERENCES Curso(id) ON DELETE CASCADE,
    PRIMARY KEY (codigoEstudiante, idCurso)
);

-- Tabla Horario
CREATE TABLE Horario (
    id SERIAL PRIMARY KEY,
    cursoId INTEGER REFERENCES Curso(id) ON DELETE CASCADE,
    dia VARCHAR(50) NOT NULL CHECK (dia IN ('Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado')),
    inicio TIME NOT NULL,
    fin TIME NOT NULL,
    CHECK (inicio < fin)
);


-- Insertar estudiantes
INSERT INTO Estudiante (codigo, nombre, apellido) VALUES
('1', 'Juan', 'Pérez'),
('2', 'Ana', 'López'),
('3', 'Carlos', 'Martínez'),
('4', 'Lucía', 'Ramírez'),
('5', 'David', 'García'),
('6', 'María', 'Hernández'),
('7', 'Pedro', 'Torres'),
('8', 'Elena', 'Sánchez'),
('9', 'Sofía', 'Vargas'),
('10', 'Miguel', 'Castro');

-- Insertar asignaturas
INSERT INTO Asignatura (codigo, nombre, horas) VALUES
('MAT101', 'Cálculo I', 5),
('FIS101', 'Física General', 4),
('QUI101', 'Química Básica', 4);

-- Insertar profesores
INSERT INTO Profesor (nombre, apellido) VALUES
('Ursula', 'Iguaran'),
('Mario', 'Zapata'),
('Laura', 'Gómez'),
('Esteban', 'Rojas'),
('Juliana', 'Moreno');

-- Insertar cursos
INSERT INTO Curso (fecha, codigoAsignatura, profesorId) VALUES
('2025-02-01', 'MAT101', 1),
('2025-02-01', 'FIS101', 2),
('2025-02-01', 'QUI101', 3),
('2025-02-01', 'MAT101', 4),
('2025-02-01', 'FIS101', 5),
('2025-02-01', 'QUI101', 1),
('2025-02-01', 'MAT101', 2),
('2025-02-01', 'FIS101', 3);

-- Insertar aulas
INSERT INTO Aula (numero, capacidad, edificio, piso, etiqueta) VALUES
('A101', 40, 'Edificio A', 1, 'Aula grande'),
('A102', 30, 'Edificio A', 1, 'Aula mediana'),
('B201', 25, 'Edificio B', 2, 'Laboratorio'),
('B202', 35, 'Edificio B', 2, 'Sala multimedia'),
('C301', 20, 'Edificio C', 3, 'Aula pequeña'),
('C302', 40, 'Edificio C', 3, 'Conferencias'),
('D401', 45, 'Edificio D', 4, 'Taller'),
('D402', 50, 'Edificio D', 4, 'Magistral'),
('E501', 30, 'Edificio E', 5, 'Virtual Room');

-- Asignar cursos a aulas (CursoAula)
INSERT INTO CursoAula (idCurso, numeroAula) VALUES
(1, 'A101'),
(2, 'A102'),
(3, 'B201'),
(4, 'B202'),
(5, 'C301'),
(6, 'C302'),
(7, 'D401'),
(8, 'D402');

-- punto 3.

-- Asignar estudiantes a cursos (EstudianteCurso)
INSERT INTO EstudianteCurso (codigoEstudiante, idCurso) VALUES
('1', 1), ('1', 2),
('2', 2), ('2', 3),
('3', 3), ('3', 4),
('4', 4), ('4', 5),
('5', 5), ('5', 6),
('6', 6), ('6', 7),
('7', 7), ('7', 8),
('8', 1), ('8', 3),
('9', 2), ('9', 4),
('10', 5), ('10', 7);

-- Crear horarios para cursos (Horario)
INSERT INTO Horario (cursoId, dia, inicio, fin) VALUES
(1, 'Lunes',     '08:00:00', '10:00:00'),
(2, 'Martes',    '10:00:00', '12:00:00'),
(3, 'Miércoles', '14:00:00', '16:00:00'),
(4, 'Jueves',    '08:00:00', '10:00:00'),
(5, 'Viernes',   '10:00:00', '12:00:00'),
(6, 'Lunes',     '14:00:00', '16:00:00'),
(7, 'Martes',    '08:00:00', '10:00:00'),
(8, 'Miércoles', '10:00:00', '12:00:00');
