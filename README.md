# examen1-BD-Kenneth-Chavez
Examen 1 de Teoria de Basede Datos- 16-05-2024

SCRIPT:

CREATE database Sistema_Tickets;
USE SistemaTickets;

-- Crear la tabla Usuario
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Usuario')
BEGIN
    CREATE TABLE Usuario (
        id INT IDENTITY(1,1) PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL,
        correo VARCHAR(100) NOT NULL,
        telefono VARCHAR(20)
    );
END

-- Crear la tabla Ingeniero
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Ingeniero')
BEGIN
    CREATE TABLE Ingeniero (
        id INT IDENTITY(1,1) PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL,
        correo VARCHAR(100) NOT NULL,
        especialidad VARCHAR(50),
        cargaLaboral INT DEFAULT 0
    );
END

-- Crear la tabla Categoria
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Categoria')
BEGIN
    CREATE TABLE Categoria (
        id INT IDENTITY(1,1) PRIMARY KEY,
        nombre VARCHAR(50) NOT NULL
    );
END


-- Crear la tabla Ticket para administrar Tickets
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Ticket')
BEGIN
    CREATE TABLE Ticket (
        id INT IDENTITY(1,1) PRIMARY KEY,
        usuarioId INT,
        ingenieroId INT,
        categoriaId INT,
        descripcion TEXT NOT NULL,
        fechaApertura DATETIME DEFAULT CURRENT_TIMESTAMP,
        fechaCierre DATETIME NULL,
        estado VARCHAR(20) DEFAULT 'Abierto',
        CONSTRAINT FK_Usuario FOREIGN KEY (usuarioId) REFERENCES Usuario(id),
        CONSTRAINT FK_Ingeniero FOREIGN KEY (ingenieroId) REFERENCES Ingeniero(id),
        CONSTRAINT FK_Categoria FOREIGN KEY (categoriaId) REFERENCES Categoria(id)
    );
END



-- Crear la tabla Procedimiento para almacenar 
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Procedimiento')
BEGIN
    CREATE TABLE Procedimiento (
        id INT IDENTITY(1,1) PRIMARY KEY,
        ticketId INT,
        descripcion TEXT NOT NULL,
        fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
        CONSTRAINT FK_Ticket FOREIGN KEY (ticketId) REFERENCES Ticket(id)
    );
END

-- Crear la tabla Calificacion si no existe
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Calificacion')
BEGIN
    CREATE TABLE Calificacion (
        id INT IDENTITY(1,1) PRIMARY KEY,
        ticketId INT,
        puntuacion INT CHECK (puntuacion >= 1 AND puntuacion <= 5),
        comentario NVARCHAR(MAX),
        CONSTRAINT FK_Ticket_Calificacion FOREIGN KEY (ticketId) REFERENCES Ticket(id)
    );
END



-- Insertar datos en la tabla Usuarios
INSERT INTO Usuario (nombre, correo, telefono) VALUES
('Kenneth Chavez', 'kenneth@gmail.com', '888888889'),
('Gabriela Morataya', 'gaby@gmail.com', '95959595'),
('Emily Mendoza González', 'mina@gmail.com', '9999999'); -- Pedro no tiene teléfono registrado

-- Insertar datos en la tabla Ingeniero
INSERT INTO Ingeniero (nombre, correo, especialidad, cargaLaboral) VALUES
('Gabriel Sabiondo', 'Gabo@gmail.com', 'IT', 20),
('Juan Hernandez', 'juan@gmail.com', 'Hardware', 30),
('Kenneth Munguia', 'jean@gmail.com.com', 'Software', 60);


-- Insertar datos en la tabla Categoria
INSERT INTO Categoria (nombre) VALUES
('Soporte Técnico'),
('Redes'),
('Software');

-- Insertar datos en la tabla Ticket
INSERT INTO Ticket (usuarioId, ingenieroId, categoriaId, descripcion, fechaApertura, estado) VALUES
(1, 2, 3, 'Problemas de internet', GETDATE(), 'Abierto'),
(2, 1, 2, 'La impresora vuela', GETDATE(), 'Abierto'),
(3, 3, 1, 'No hay sistema ', GETDATE(), 'Abierto');


-- Insertar datos en la tabla Procedimiento
INSERT INTO Procedimiento (ticketId, descripcion, fecha) VALUES
(1, 'Reinicio de modem ', GETDATE()),
(2, 'se corto alas a impresora ', GETDATE()),
(3, 'se solvento el sistema', GETDATE());


-- Insertar datos en la tabla Calificacion
INSERT INTO Calificacion (ticketId, puntuacion, comentario) VALUES
(1, 4, 'El problema fue resuelto rápidamente.'),
(2, 3, 'se solvento.'),
(3, 5, 'solvento el sistema.');![diagrama-ER](https://github.com/KennethChavez/examen1-BD-Kenneth-Chavez/assets/91275412/5a75b483-ee53-4be9-8284-d0fb6729716b)
