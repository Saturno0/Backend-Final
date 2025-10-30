# Backend - E-commerce API

Backend de e-commerce desarrollado con Node.js, Express y MongoDB. Expone una API RESTful para la gestión de usuarios, productos, categorías, envío de emails y manejo de imágenes en S3. Incluye documentación Swagger.

## Características Principales

- API RESTful con documentación Swagger
- Autenticación JWT para usuarios
- Gestión de productos (colores, talles y stock)
- Sistema de categorías
- Subida y gestión de imágenes en AWS S3 (URLs firmadas)
- Validación robusta y middlewares de seguridad

## Dependencias Principales

- express, cors, body-parser, express-session
- mongoose, mongodb
- jsonwebtoken, bcrypt
- multer, sharp
- @aws-sdk/client-s3, @aws-sdk/s3-request-presigner
- swagger-jsdoc, swagger-ui-express
- nodemailer

Desarrollo: dotenv, nodemon, npm-check-updates

## Estructura del Proyecto

```
backend/
├─ api/
│  └─ index.js               # Punto de entrada del servidor
├─ src/
│  ├─ config/
│  │  ├─ aws.js              # Configuración AWS S3
│  │  └─ swagger.js          # Configuración Swagger
│  ├─ controllers/
│  │  ├─ categoryController.js
│  │  ├─ productController.js
│  │  └─ userController.js
│  ├─ middlewares/
│  │  ├─ uploadMiddleware.js
│  │  └─ verifyTokemMiddleware.js
│  ├─ models/
│  │  ├─ Category.js
│  │  ├─ Product.js
│  │  └─ User.js
│  ├─ routes/
│  │  ├─ categoryRoute.js
│  │  ├─ productRoute.js
│  │  ├─ userRoute.js
│  │  └─ emailRoute.js
│  └─ services/
│     ├─ categoryService.js
│     ├─ imageService.js
│     ├─ productService.js
│     └─ emailService.js
├─ config.js                  # Variables de entorno
├─ db.js                      # Conexión a la base de datos
├─ package.json
└─ env.model
```

## Configuración

Crear un archivo `.env` basado en `env.model`:

```env
PORT=3000
MONGO_URI=mongodb://localhost:27017/ecommerce
JWT_SECRET=tu_jwt_secret_aqui
DB=ecommerce

# AWS S3 Configuration
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=tu_access_key_id
AWS_SECRET_ACCESS_KEY=tu_secret_access_key
AWS_S3_BUCKET_NAME=nombre_de_tu_bucket

# Email Configuration
EMAIL_USER=tu_email@gmail.com
EMAIL_PASS=tu_app_password
```

## Instalación

```bash
npm install        # Instala dependencias
npm run dev        # Modo desarrollo
npm start          # Producción
```

## Modelos de Base de Datos

### User
```javascript
{
  nombre: String,
  email: String, // único
  password: String, // encriptado
  rol: String, // default: "user"
  activo: Boolean // default: true
}
```

### Product
```javascript
{
  name: String,
  description: String,
  price: Number,
  category: ObjectId, // ref: Category
  colores: [{ name: String, cantidad: Number, stock: Number }],
  talles: [String],
  imageUrl: String,
  stock: Boolean
}
```

### Category
```javascript
{
  nombre: String, // único
  description: String
}
```

## Endpoints y Verbos HTTP

Prefijo base: `/api`

### Usuarios (`/api/users`)
- POST `/createUser` — Crear usuario
- GET `/getUsers` — Listar usuarios
- POST `/logIn` — Inicio de sesión (JWT)
- PUT `/update/:id` — Actualizar usuario
- GET `/getRol/:id` — Obtener rol

### Productos (`/api/products`)
- GET `/getAllProducts` — Listar productos
- GET `/getProduct/:id` — Obtener producto por ID
- POST `/createProduct` — Crear producto
- PUT `/updateProduct/:id` — Actualizar producto
- DELETE `/deleteProduct/:id` — Eliminar producto
- GET `/:id/colors` — Colores por producto
- GET `/:id/sizes` — Talles por producto

### Categorías (`/api/categories`)
- GET `/` — Listar categorías
- GET `/:id` — Obtener categoría
- POST `/` — Crear categoría
- POST `/bulk` — Crear múltiples categorías

### Email (`/api/email`)
- POST `/send-confirmation` — Enviar confirmación de orden por email

## Servicio de Email

- Función: `sendOrderConfirmationEmail(orderData)`
- Valida datos de entrada, genera número de orden y envía dos correos:
  - Al administrador (resumen completo)
  - Al cliente (confirmación y detalle)
- Requiere variables `EMAIL_USER` y `EMAIL_PASS`

## Swagger

- UI: `GET /api-docs`
- Incluye especificaciones generadas por `swagger-jsdoc` a partir de anotaciones JSDoc en rutas (p. ej., `src/routes/emailRoute.js`).

## Notas

- Archivo guardado en UTF-8 para evitar problemas de acentuación.
- Endpoint de email aclarado: `POST /api/email/send-confirmation`.

