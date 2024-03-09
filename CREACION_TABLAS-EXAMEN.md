# EXAMEN BASES DE DATOS POSTGRES

JEFFERSON DARIO BELTRAN LASPRILLA - Campuslands

### Creacion de Base de Datos

CREATE DATABASE ventas;
 
### Creacion de tablas

CREATE TABLE cliente (
	  id SERIAL PRIMARY KEY,
	  nombre VARCHAR(100) NOT NULL,
	  apellido1 VARCHAR(100) NOT NULL,
	  apellido2 VARCHAR(100),
	  ciudad VARCHAR(100),
	  categoria INTEGER
);

CREATE TABLE comercial (
	  id SERIAL PRIMARY KEY,
	  nombre VARCHAR(100) NOT NULL,
	  apellido1 VARCHAR(100) NOT NULL,
	  apellido2 VARCHAR(100),
	  comision FLOAT
);

CREATE TABLE pedido (
     	  id SERIAL PRIMARY KEY,
     	  total DOUBLE PRECISION NOT NULL,
     	  fecha DATE,
     	  id_cliente INTEGER REFERENCES cliente(id),
     	  id_comercial INTEGER REFERENCES comercial(id)
);	
