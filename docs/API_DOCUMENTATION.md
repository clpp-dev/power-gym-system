# API Power Gym System - Documentación de Endpoints

## Base URL
```
http://localhost:5000
```

## Información General

Esta API permite gestionar clientes y membresías del gimnasio Power Gym System. Todas las respuestas están en formato JSON.

---

## Endpoints Disponibles

### 📋 Índice
- [Autenticación](#autenticación)
- [Clientes](#clientes)
- [Membresías](#membresías)

---

## Autenticación

### Login de Administrador
**POST** `/api/login`

**Descripción:** Autentica un usuario administrador en el sistema.

**Body (JSON):**
```json
{
  "username": "admin",
  "password": "admin123"
}
```

**Nota:** Puedes usar el `username` o el `email` para hacer login.

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "message": "Login exitoso",
  "data": {
    "_id": "60d5ec49f1b2c72b8c8e4a1a",
    "username": "admin",
    "nombre": "Admin",
    "email": "admin@powergym.com",
    "rol": "admin",
    "activo": true
  }
}
```

**Respuesta de error (401):**
```json
{
  "success": false,
  "message": "Credenciales inválidas"
}
```

**Respuesta de error (400):**
```json
{
  "success": false,
  "message": "Por favor proporcione username y password"
}
```

---

## Clientes

### 1. Obtener todos los clientes
**GET** `/api/clientes`

**Descripción:** Obtiene la lista completa de todos los clientes registrados.

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "count": 2,
  "data": [
    {
      "_id": "60d5ec49f1b2c72b8c8e4a1b",
      "nombre": "Juan",
      "apellido": "Martínez",
      "cedula": "123456789",
      "email": "juan.martinez@email.com",
      "telefono": "321-123-3343",
      "fechaNacimiento": "1990-05-15T00:00:00.000Z",
      "direccion": "Calle Principal 123",
      "documento": {
        "tipo": "DNI",
        "numero": "123456789"
      },
      "membresia": {
        "_id": "60d5ec49f1b2c72b8c8e4a1c",
        "nombre": "Plan Full Access",
        "tipo": "Mensual",
        "precio": 350000
      },
      "fechaInicioMembresia": "2024-01-15T00:00:00.000Z",
      "fechaFinMembresia": "2024-12-15T00:00:00.000Z",
      "fechaInscripcion": "2024-01-15T00:00:00.000Z",
      "estado": "Activo",
      "activo": true,
      "createdAt": "2024-01-10T10:30:00.000Z",
      "updatedAt": "2024-01-10T10:30:00.000Z"
    }
  ]
}
```

---

### 2. Obtener un cliente por ID
**GET** `/api/clientes/:id`

**Descripción:** Obtiene los detalles de un cliente específico.

**Parámetros de ruta:**
- `id` (string, requerido): ID del cliente

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "data": {
    "_id": "60d5ec49f1b2c72b8c8e4a1b",
    "nombre": "Juan",
    "apellido": "Pérez",
    "email": "juan.perez@email.com",
    "telefono": "+1234567890",
    "fechaNacimiento": "1990-05-15T00:00:00.000Z",
    "direccion": "Calle Principal 123",
    "documento": {
      "tipo": "DNI",
      "numero": "12345678"
    },
    "membresia": {
      "_id": "60d5ec49f1b2c72b8c8e4a1c",
      "nombre": "Premium",
      "precio": 45.00
    },
    "fechaInscripcion": "2024-01-10T00:00:00.000Z",
    "activo": true
  }
}
```

**Respuesta de error (404):**
```json
{
  "success": false,
  "message": "Cliente no encontrado"
}
```

---

### 3. Crear un nuevo cliente
**POST** `/api/clientes`

**Descripción:** Crea un nuevo cliente en el sistema.

**Body (JSON):**
```json
{
  "nombre": "María",
  "apellido": "González",
  "cedula": "87654321",
  "email": "maria.gonzalez@email.com",
  "telefono": "315-987-6543",
  "fechaNacimiento": "1995-08-20",
  "direccion": "Avenida Secundaria 456",
  "documento": {
    "tipo": "Cédula",
    "numero": "87654321"
  },
  "membresia": "60d5ec49f1b2c72b8c8e4a1c",
  "fechaInicioMembresia": "2024-12-13",
  "fechaFinMembresia": "2025-01-13",
  "estado": "Activo"
}
```

**Campos requeridos:**
- `nombre` (string)
- `apellido` (string)
- `cedula` (string, único)
- `email` (string, único)
- `telefono` (string)
- `fechaNacimiento` (date)
- `documento.numero` (string, único)

**Campos opcionales:**
- `direccion` (string)
- `documento.tipo` (string: 'DNI', 'Pasaporte', 'Cédula') - Default: 'DNI'
- `membresia` (ObjectId)
- `fechaInicioMembresia` (date)
- `fechaFinMembresia` (date)
- `estado` (string: 'Activo', 'Inactivo', 'Próximo a Vencer') - Default: 'Activo'
- `activo` (boolean) - Default: true

**Respuesta exitosa (201):**
```json
{
  "success": true,
  "message": "Cliente creado exitosamente",
  "data": {
    "_id": "60d5ec49f1b2c72b8c8e4a1d",
    "nombre": "María",
    "apellido": "González",
    "email": "maria.gonzalez@email.com",
    "telefono": "+9876543210",
    "fechaNacimiento": "1995-08-20T00:00:00.000Z",
    "direccion": "Avenida Secundaria 456",
    "documento": {
      "tipo": "DNI",
      "numero": "87654321"
    },
    "membresia": "60d5ec49f1b2c72b8c8e4a1c",
    "fechaInscripcion": "2024-12-13T00:00:00.000Z",
    "activo": true
  }
}
```

**Respuesta de error (400):**
```json
{
  "success": false,
  "message": "Error al crear el cliente",
  "error": "El email ya existe"
}
```

---

### 4. Actualizar un cliente
**PUT** `/api/clientes/:id`

**Descripción:** Actualiza la información de un cliente existente.

**Parámetros de ruta:**
- `id` (string, requerido): ID del cliente

**Body (JSON):**
```json
{
  "telefono": "+1111111111",
  "direccion": "Nueva Dirección 789",
  "membresia": "60d5ec49f1b2c72b8c8e4a1e"
}
```

**Nota:** Solo se deben enviar los campos que se desean actualizar.

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "message": "Cliente actualizado exitosamente",
  "data": {
    "_id": "60d5ec49f1b2c72b8c8e4a1b",
    "nombre": "Juan",
    "apellido": "Pérez",
    "email": "juan.perez@email.com",
    "telefono": "+1111111111",
    "fechaNacimiento": "1990-05-15T00:00:00.000Z",
    "direccion": "Nueva Dirección 789",
    "documento": {
      "tipo": "DNI",
      "numero": "12345678"
    },
    "membresia": {
      "_id": "60d5ec49f1b2c72b8c8e4a1e",
      "nombre": "Básica",
      "precio": 25.00
    },
    "activo": true
  }
}
```

**Respuesta de error (404):**
```json
{
  "success": false,
  "message": "Cliente no encontrado"
}
```

---

### 5. Eliminar un cliente
**DELETE** `/api/clientes/:id`

**Descripción:** Elimina un cliente del sistema.

**Parámetros de ruta:**
- `id` (string, requerido): ID del cliente

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "message": "Cliente eliminado exitosamente",
  "data": {}
}
```

**Respuesta de error (404):**
```json
{
  "success": false,
  "message": "Cliente no encontrado"
}
```

---

## Membresías

### 1. Obtener todas las membresías
**GET** `/api/membresias`

**Descripción:** Obtiene la lista completa de todas las membresías disponibles.

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "count": 3,
  "data": [
    {
      "_id": "60d5ec49f1b2c72b8c8e4a1c",
      "nombre": "Plan Full Access",
      "tipo": "Mensual",
      "descripcion": "Acceso completo a todas las instalaciones.",
      "precio": 350000,
      "duracion": 30,
      "beneficios": [
        "Acceso ilimitado al gimnasio",
        "Uso de todas las instalaciones",
        "Clases grupales",
        "Casillero"
      ],
      "activo": true,
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-01T00:00:00.000Z"
    },
    {
      "_id": "60d5ec49f1b2c72b8c8e4a1d",
      "nombre": "Plan Mañanas",
      "tipo": "Mensual",
      "descripcion": "Acceso solo en horario de mañana (6am - 3pm).",
      "precio": 195000,
      "duracion": 30,
      "beneficios": [
        "Acceso al gimnasio en horario de mañana",
        "Uso de equipamiento básico",
        "Casillero"
      ],
      "activo": true,
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-01T00:00:00.000Z"
    }
  ]
}
```

---

### 2. Obtener una membresía por ID
**GET** `/api/membresias/:id`

**Descripción:** Obtiene los detalles de una membresía específica.

**Parámetros de ruta:**
- `id` (string, requerido): ID de la membresía

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "data": {
    "_id": "60d5ec49f1b2c72b8c8e4a1c",
    "nombre": "Premium",
    "descripcion": "Membresía premium con todos los beneficios",
    "precio": 45.00,
    "duracion": 30,
    "beneficios": [
      "Acceso ilimitado al gimnasio",
      "Uso de todas las instalaciones",
      "Clases grupales",
      "Casillero premium",
      "Toalla incluida",
      "1 sesión de entrenador personal"
    ],
    "activo": true
  }
}
```

**Respuesta de error (404):**
```json
{
  "success": false,
  "message": "Membresía no encontrada"
}
```

---

### 3. Crear una nueva membresía
**POST** `/api/membresias`

**Descripción:** Crea una nueva membresía en el sistema.

**Body (JSON):**
```json
{
  "nombre": "Plan Semestral",
  "tipo": "Semestral",
  "descripcion": "Membresía semestral con descuento especial",
  "precio": 1800000,
  "duracion": 180,
  "beneficios": [
    "Acceso ilimitado al gimnasio",
    "Uso de todas las instalaciones",
    "Clases grupales",
    "Casillero premium",
    "Descuento del 15%"
  ]
}
```

**Campos requeridos:**
- `nombre` (string, único)
- `tipo` (string: 'Mensual', 'Trimestral', 'Semestral', 'Anual')
- `descripcion` (string)
- `precio` (number, mínimo 0)
- `duracion` (number, mínimo 1) - duración en días

**Campos opcionales:**
- `beneficios` (array de strings)
- `activo` (boolean) - Default: true

**Respuesta exitosa (201):**
```json
{
  "success": true,
  "message": "Membresía creada exitosamente",
  "data": {
    "_id": "60d5ec49f1b2c72b8c8e4a1e",
    "nombre": "VIP",
    "descripcion": "Membresía VIP con beneficios exclusivos",
    "precio": 75.00,
    "duracion": 30,
    "beneficios": [
      "Acceso ilimitado 24/7",
      "Entrenador personal dedicado",
      "Nutricionista",
      "Spa y sauna",
      "Estacionamiento privado"
    ],
    "activo": true
  }
}
```

**Respuesta de error (400):**
```json
{
  "success": false,
  "message": "Error al crear la membresía",
  "error": "El nombre de la membresía ya existe"
}
```

---

### 4. Actualizar una membresía
**PUT** `/api/membresias/:id`

**Descripción:** Actualiza la información de una membresía existente.

**Parámetros de ruta:**
- `id` (string, requerido): ID de la membresía

**Body (JSON):**
```json
{
  "precio": 50.00,
  "beneficios": [
    "Acceso ilimitado al gimnasio",
    "Uso de todas las instalaciones",
    "Clases grupales",
    "Casillero premium",
    "Toalla incluida",
    "2 sesiones de entrenador personal"
  ]
}
```

**Nota:** Solo se deben enviar los campos que se desean actualizar.

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "message": "Membresía actualizada exitosamente",
  "data": {
    "_id": "60d5ec49f1b2c72b8c8e4a1c",
    "nombre": "Premium",
    "descripcion": "Membresía premium con todos los beneficios",
    "precio": 50.00,
    "duracion": 30,
    "beneficios": [
      "Acceso ilimitado al gimnasio",
      "Uso de todas las instalaciones",
      "Clases grupales",
      "Casillero premium",
      "Toalla incluida",
      "2 sesiones de entrenador personal"
    ],
    "activo": true
  }
}
```

**Respuesta de error (404):**
```json
{
  "success": false,
  "message": "Membresía no encontrada"
}
```

---

### 5. Eliminar una membresía
**DELETE** `/api/membresias/:id`

**Descripción:** Elimina una membresía del sistema.

**Parámetros de ruta:**
- `id` (string, requerido): ID de la membresía

**Respuesta exitosa (200):**
```json
{
  "success": true,
  "message": "Membresía eliminada exitosamente",
  "data": {}
}
```

**Respuesta de error (404):**
```json
{
  "success": false,
  "message": "Membresía no encontrada"
}
```

---

## Códigos de Estado HTTP

- `200 OK`: Solicitud exitosa
- `201 Created`: Recurso creado exitosamente
- `400 Bad Request`: Error en los datos enviados
- `404 Not Found`: Recurso no encontrado
- `500 Internal Server Error`: Error interno del servidor

---

## Ejemplos de Uso con cURL

### Login de administrador
```bash
curl -X POST http://localhost:5000/api/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "admin123"
  }'
```

### Obtener todos los clientes
```bash
curl -X GET http://localhost:5000/api/clientes
```

### Crear un nuevo cliente
```bash
curl -X POST http://localhost:5000/api/clientes \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Carlos",
    "apellido": "Rodríguez",
    "email": "carlos.rodriguez@email.com",
    "telefono": "+1234567890",
    "fechaNacimiento": "1988-03-12",
    "documento": {
      "tipo": "DNI",
      "numero": "11223344"
    }
  }'
```

### Actualizar un cliente
```bash
curl -X PUT http://localhost:5000/api/clientes/60d5ec49f1b2c72b8c8e4a1b \
  -H "Content-Type: application/json" \
  -d '{
    "telefono": "+9999999999"
  }'
```

### Eliminar un cliente
```bash
curl -X DELETE http://localhost:5000/api/clientes/60d5ec49f1b2c72b8c8e4a1b
```

### Crear una nueva membresía
```bash
curl -X POST http://localhost:5000/api/membresias \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Estudiante",
    "descripcion": "Membresía con descuento para estudiantes",
    "precio": 20.00,
    "duracion": 30,
    "beneficios": [
      "Acceso al gimnasio",
      "Uso de equipamiento básico"
    ]
  }'
```

---

## Notas Adicionales

- Todas las fechas están en formato ISO 8601
- Los IDs son generados automáticamente por MongoDB
- Los campos `createdAt` y `updatedAt` se gestionan automáticamente
- CORS está habilitado para todas las peticiones
- La API acepta y devuelve datos en formato JSON

---

## Usuario Administrador por Defecto

**Username:** admin  
**Email:** admin@powergym.com  
**Password:** admin123  
**Rol:** admin

---

## Contacto y Soporte

Para más información o soporte, contactar al equipo de desarrollo.
