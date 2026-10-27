<div align="center">
  <h1><span style="color:#000000; background-color:#ffccde; padding:6px 12px; border-radius:10px;">🧩 Aprendizaje Autónomo </span></h1>


| **Información General**                |                                                                                |
|----------------------------------------|--------------------------------------------------------------------------------|
| **Facultad:**                          | Facultad de la energía, las industrias y los recursos naturales no renovables. |
| **Carrera:**                           | Computación                                                                    |
| **Semestre y paralelo:**               | Tercero “A”                                                                    |
| **Periodo Académico:**                 | 2026 - 2027                                                                    |
| **Estudiante:**                        | Selena Castillo                                                                |
| **Enlace Ejercicios (GitHub/GitLab):** | <https://github.com/seleeanis/BASE-DE-DATOS-PORTAFOLIO>                        |
| **Materia:**                           | Base de Datos                                                                  |
| **Docente Encargado:**                 | Ing. René Guamán                                                               |

</div>

## **Instrucciones**

*Agregar la evidencia correspondiente para cada uno de los siguientes ejercicios.*

- *Código SQL del modelo de cada ejercicio*

### **Ejemplo 1: Sistema de Biblioteca**

Una biblioteca registra libros y autores. Un libro puede tener varios autores y un autor varios libros. Los socios toman libros prestados; de cada préstamo interesan la fecha de retiro y la de devolución.

```sql
CREATE TABLE Libro (
    id_libro INT PRIMARY KEY,
    titulo VARCHAR(100)
);

CREATE TABLE Autor (
    id_autor INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Socio (
    id_socio INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Escribe (
    id_libro INT,
    id_autor INT,
    PRIMARY KEY (id_libro, id_autor),
    FOREIGN KEY (id_libro) REFERENCES Libro(id_libro),
    FOREIGN KEY (id_autor) REFERENCES Autor(id_autor)
);

CREATE TABLE Prestamo (
    id_libro INT,
    id_socio INT,
    fecha_retiro DATE,
    fecha_devolucion DATE,
    PRIMARY KEY (id_libro, id_socio, fecha_retiro),
    FOREIGN KEY (id_libro) REFERENCES Libro(id_libro),
    FOREIGN KEY (id_socio) REFERENCES Socio(id_socio)
);
```


### **Ejemplo 2: Pedidos de Restaurante**

Los clientes realizan pedidos que contienen varios platos con distinta cantidad. Los platos se agrupan en categorías. Cada pedido lo atiende un único mesero. El precio del plato debe registrarse tal como estaba al momento del pedido, aunque luego cambie.

```sql
CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Mesero (
    id_mesero INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Categoria (
    id_categoria INT PRIMARY KEY,
    nombre VARCHAR(30)
);

CREATE TABLE Plato (
    id_plato INT PRIMARY KEY,
    nombre VARCHAR(50),
    precio DECIMAL(8,2),
    id_categoria INT,
    FOREIGN KEY (id_categoria) REFERENCES Categoria(id_categoria)
);

CREATE TABLE Pedido (
    id_pedido INT PRIMARY KEY,
    fecha DATE,
    id_cliente INT,
    id_mesero INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente),
    FOREIGN KEY (id_mesero) REFERENCES Mesero(id_mesero)
);

CREATE TABLE Detalle_Pedido (
    id_pedido INT,
    id_plato INT,
    cantidad INT,
    precio_unitario DECIMAL(8,2),
    PRIMARY KEY (id_pedido, id_plato),
    FOREIGN KEY (id_pedido) REFERENCES Pedido(id_pedido),
    FOREIGN KEY (id_plato) REFERENCES Plato(id_plato)
);
```

### **Ejemplo 3: Clínica Médica**

Los pacientes solicitan citas; cada cita la atiende un médico a un paciente en fecha, hora y consultorio. De algunas citas se genera una receta que incluye medicamentos con dosis y duración. Cada médico pertenece a una especialidad.

```sql
CREATE TABLE Paciente (
    id_paciente INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Especialidad (
    id_especialidad INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Medico (
    id_medico INT PRIMARY KEY,
    nombre VARCHAR(50),
    id_especialidad INT,
    FOREIGN KEY (id_especialidad) REFERENCES Especialidad(id_especialidad)
);

CREATE TABLE Cita (
    id_cita INT PRIMARY KEY,
    fecha DATE,
    hora TIME,
    consultorio VARCHAR(20),
    id_paciente INT,
    id_medico INT,
    FOREIGN KEY (id_paciente) REFERENCES Paciente(id_paciente),
    FOREIGN KEY (id_medico) REFERENCES Medico(id_medico)
);

CREATE TABLE Receta (
    id_receta INT PRIMARY KEY,
    id_cita INT UNIQUE,
    FOREIGN KEY (id_cita) REFERENCES Cita(id_cita)
);

CREATE TABLE Medicamento (
    id_medicamento INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Detalle_Receta (
    id_receta INT,
    id_medicamento INT,
    dosis VARCHAR(50),
    duracion VARCHAR(50),
    PRIMARY KEY (id_receta, id_medicamento),
    FOREIGN KEY (id_receta) REFERENCES Receta(id_receta),
    FOREIGN KEY (id_medicamento) REFERENCES Medicamento(id_medicamento)
);
```

### **Ejemplo 4: Sistema de Aerolínea**

Una aerolínea opera vuelos (cada uno con número, fecha, origen y destino), cada vuelo lo realiza un avión. Los pasajeros realizan reservas para un vuelo específico, indicando asiento y tarifa pagada en el momento de la reserva.

```sql
CREATE TABLE Pasajero (
    ci VARCHAR(10) PRIMARY KEY,
    passport VARCHAR(20)
);

CREATE TABLE Avion (
    placa VARCHAR(10) PRIMARY KEY
);

CREATE TABLE Vuelo (
    num_vuelo INT PRIMARY KEY,
    fecha DATE,
    origen VARCHAR(50),
    destino VARCHAR(50),
    placa VARCHAR(10),
    FOREIGN KEY (placa) REFERENCES Avion(placa)
);

CREATE TABLE Reserva (
    ci VARCHAR(10),
    num_vuelo INT,
    asiento VARCHAR(5),
    tarifa DECIMAL(8,2),
    PRIMARY KEY (ci, num_vuelo),
    FOREIGN KEY (ci) REFERENCES Pasajero(ci),
    FOREIGN KEY (num_vuelo) REFERENCES Vuelo(num_vuelo)
);
```

### **Ejercicios Propuestos**

#### **Ejercicio 5: Empresa Discográfica**

Una empresa discográfica necesita modelar los datos sobre sus diferentes recursos según las siguientes características:

- Manager: de cada mánager se almacena un identificador de manager y su nombre. Un manager representa a una serie de artistas.

- Artista: un artista es representado por un manager. De los artistas se almacena su nombre completo (usando un único atributo) y su NIF.

- Evento de promoción: los artistas participan en eventos de promoción para dar a conocer sus trabajos. En un evento de promoción pueden participar varios artistas. De un evento se almacena un identificador único, la fecha de celebración y el número de asistentes.

```sql
CREATE TABLE Manager (
    id_manager INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Artista (
    nif VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100),
    id_manager INT,
    FOREIGN KEY (id_manager) REFERENCES Manager(id_manager)
);

CREATE TABLE Evento (
    id_evento INT PRIMARY KEY,
    fecha DATE,
    num_asistentes INT
);

CREATE TABLE Participa (
    nif VARCHAR(15),
    id_evento INT,
    PRIMARY KEY (nif, id_evento),
    FOREIGN KEY (nif) REFERENCES Artista(nif),
    FOREIGN KEY (id_evento) REFERENCES Evento(id_evento)
);
```

#### **Ejercicio 6: Tienda Informática**

Se desea crear una aplicación para la gestión de una tienda informática. La tienda dispone de una serie de productos que se venden a los clientes. De cada producto informático se desea guardar el código, descripción, precio y número de existencias. De cada cliente se desea guardar el código, nombre, apellidos, dirección y número telefónico. Un cliente puede comprar varios productos en la tienda y un mismo producto puede ser comprado por varios clientes. Cada vez que se compre un artículo quedará registrada la compra en la base de datos junto con la fecha en la que se compró el artículo. La tienda tiene contactos con varios proveedores que son los que suministran los productos. Un mismo producto puede ser suministrado por varios proveedores. De cada proveedor se desea guardar el código, nombres, apellidos, dirección, provincia y número de teléfono.

```sql
CREATE TABLE Cliente (
    cod_cliente INT PRIMARY KEY,
    nombre VARCHAR(50),
    apellido VARCHAR(50),
    direccion VARCHAR(100),
    telefono VARCHAR(10)
);

CREATE TABLE Producto (
    cod_producto INT PRIMARY KEY,
    descripcion VARCHAR(100),
    precio DECIMAL(8,2),
    existencias INT
);

CREATE TABLE Proveedor (
    cod_proveedor INT PRIMARY KEY,
    nombre VARCHAR(50),
    apellido VARCHAR(50),
    direccion VARCHAR(100),
    provincia VARCHAR(50),
    telefono VARCHAR(10)
);

CREATE TABLE Compra (
    cod_cliente INT,
    cod_producto INT,
    fecha DATE,
    PRIMARY KEY (cod_cliente, cod_producto, fecha),
    FOREIGN KEY (cod_cliente) REFERENCES Cliente(cod_cliente),
    FOREIGN KEY (cod_producto) REFERENCES Producto(cod_producto)
);

CREATE TABLE Suministra (
    cod_proveedor INT,
    cod_producto INT,
    PRIMARY KEY (cod_proveedor, cod_producto),
    FOREIGN KEY (cod_proveedor) REFERENCES Proveedor(cod_proveedor),
    FOREIGN KEY (cod_producto) REFERENCES Producto(cod_producto)
);
```

#### **Ejercicio 7: Discos Musicales**

- La tienda vende discos de diferentes géneros musicales y cantantes

- Cada género musical tiene un identificador

- Cada disco tiene titulo, genero musical, precio y cantante

- De un cantante se registra el nombre y su país, Todo disco pertenece a un cantante y un cantante puede tener muchos discos. Además un disco tiene un conjunto de canciones(identificador y título)

- Un disco tiene muchas canciones y una canción puede estar en varios discos. Interesa conocer la posición de una canción en un determinado disco

```sql
CREATE TABLE Genero (
    id_genero INT PRIMARY KEY,
    nombre VARCHAR(30)
);

CREATE TABLE Cantante (
    id_cantante INT PRIMARY KEY,
    nombre VARCHAR(50),
    pais VARCHAR(30)
);

CREATE TABLE Disco (
    id_disco INT PRIMARY KEY,
    titulo VARCHAR(100),
    precio DECIMAL(8,2),
    id_genero INT,
    id_cantante INT,
    FOREIGN KEY (id_genero) REFERENCES Genero(id_genero),
    FOREIGN KEY (id_cantante) REFERENCES Cantante(id_cantante)
);

CREATE TABLE Cancion (
    id_cancion INT PRIMARY KEY,
    titulo VARCHAR(100)
);

CREATE TABLE Incluye (
    id_disco INT,
    id_cancion INT,
    posicion INT,
    PRIMARY KEY (id_disco, id_cancion),
    FOREIGN KEY (id_disco) REFERENCES Disco(id_disco),
    FOREIGN KEY (id_cancion) REFERENCES Cancion(id_cancion)
);
```

#### **Ejercicio 8: Camiones**

Se desea informatizar la gestión de una empresa de transporte que reparte paquetes por toda España. Los encargados de llevar los paquetes son los camioneros, de los que se quiere guardar la cédula, nombre, teléfono, dirección, salario y población en que vive. De los paquetes transportados interesa conocer el código del paquete, descripción, destinatario y dirección del destinatario.

Un camionero distribuye muchos paquetes, y un paquete sólo puede ser distribuido por un camionero. De las provincias a las que llegan los paquetes interesa guardar el código de provincia y el nombre. Un paquete sólo puede llegar a una provincia. Sin embargo, a una provincia pueden llegar varios paquetes.

De los camiones que llevan los camioneros, interesa conocer la matrícula, modelo, tipo y potencia. Un camionero puede conducir diferentes camiones en fechas diferentes, y un camión puede ser conducido por varios camioneros.

```sql
CREATE TABLE Camionero (
    ci VARCHAR(10) PRIMARY KEY,
    nombre VARCHAR(50),
    telefono VARCHAR(10),
    direccion VARCHAR(100),
    salario DECIMAL(8,2),
    poblacion VARCHAR(50)
);

CREATE TABLE Camion (
    matricula VARCHAR(10) PRIMARY KEY,
    modelo VARCHAR(30),
    tipo VARCHAR(30),
    potencia INT
);

CREATE TABLE Provincia (
    cod_provincia INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE Destinatario (
    id_destinatario INT PRIMARY KEY,
    nombre VARCHAR(50),
    direccion VARCHAR(100)
);

CREATE TABLE Paquete (
    id_paquete INT PRIMARY KEY,
    descripcion VARCHAR(100),
    ci VARCHAR(10),
    cod_provincia INT,
    id_destinatario INT,
    FOREIGN KEY (ci) REFERENCES Camionero(ci),
    FOREIGN KEY (cod_provincia) REFERENCES Provincia(cod_provincia),
    FOREIGN KEY (id_destinatario) REFERENCES Destinatario(id_destinatario)
);

CREATE TABLE Conduce (
    ci VARCHAR(10),
    matricula VARCHAR(10),
    fecha DATE,
    PRIMARY KEY (ci, matricula, fecha),
    FOREIGN KEY (ci) REFERENCES Camionero(ci),
    FOREIGN KEY (matricula) REFERENCES Camion(matricula)
);
```

#### **Ejercicio 9: Veterinaria**

- Del propietario interesa: cédula, apellidos, nombres, dirección , teléfonos

- De la mascota interesa: identificador, nombre, fecha de nacimiento, tipo

- Un propietario puede llevar una o varias mascotas

- Una mascota la lleva uno y solo un propietario

- Del personal de la clínica veterinaria interesa un código, el cédula, nombre

- Entre el personal de la clínica se distingue a los veterinarios y los auxiliares, pudiendo haber otros tipos

  - Del veterinario se guarda además su fecha_alta y especialidad

  - De los auxiliares interesa su base de cotización

- Las mascotas pasan consulta con los veterinarios. De manera que:

  - un veterinario puede pasar consulta a ninguna o a varias mascotas, y

  - una misma mascota de la clínica ha pasado consulta una o más veces en fechas diferentes con varios veterinarios.

  - Al pasar consulta se realiza un diagnóstico.

- Cada propietario tiene un familiar de contacto para casos de emergencia.

  - De este familiar interesa la cédula, nombre y teléfono.

  - Si el propietario se da de alta en la clínica veterinaria, el familiar ya no interesa.

  - El familiar es contacto de un solo propietario

```sql
CREATE TABLE Propietario (
    cedula VARCHAR(10) PRIMARY KEY,
    apellidos VARCHAR(50),
    nombres VARCHAR(50),
    direccion VARCHAR(100),
    telefonos VARCHAR(30)
);

CREATE TABLE Familiar_Contacto (
    cedula VARCHAR(10) PRIMARY KEY,
    nombre VARCHAR(50),
    telefono VARCHAR(10),
    cedula_propietario VARCHAR(10) UNIQUE,
    FOREIGN KEY (cedula_propietario) REFERENCES Propietario(cedula)
);

CREATE TABLE Mascota (
    id_mascota INT PRIMARY KEY,
    nombre VARCHAR(50),
    fecha_nac DATE,
    tipo VARCHAR(30),
    cedula_propietario VARCHAR(10),
    FOREIGN KEY (cedula_propietario) REFERENCES Propietario(cedula)
);

CREATE TABLE Personal (
    cod_personal INT PRIMARY KEY,
    cedula VARCHAR(10),
    nombre VARCHAR(50)
);

CREATE TABLE Veterinario (
    cod_personal INT PRIMARY KEY,
    fecha_alta DATE,
    especialidad VARCHAR(50),
    FOREIGN KEY (cod_personal) REFERENCES Personal(cod_personal)
);

CREATE TABLE Auxiliar (
    cod_personal INT PRIMARY KEY,
    base_cotizacion DECIMAL(8,2),
    FOREIGN KEY (cod_personal) REFERENCES Personal(cod_personal)
);

CREATE TABLE Consulta (
    id_consulta INT PRIMARY KEY,
    fecha DATE,
    diagnostico VARCHAR(200),
    id_mascota INT,
    cod_personal INT,
    FOREIGN KEY (id_mascota) REFERENCES Mascota(id_mascota),
    FOREIGN KEY (cod_personal) REFERENCES Veterinario(cod_personal)
);
```
