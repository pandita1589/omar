# 🌙 Moon Studios - Sistema Administrativo Empresarial

Sistema de dashboard administrativo completo con autenticación, gestión de empleados, tareas, calendario y módulos especializados.

## 🚀 Características Principales

- **Sistema de Login Seguro** con JWT
- **Dashboard CEO** con control total
- **Paneles Especializados** por módulo (Ventas, Base de Datos, Administración, Soporte, Ilustración)
- **Gestión de Empleados** completa
- **Sistema de Tareas** asignable
- **Calendario Global** sincronizado
- **Base de Datos MongoDB** para persistencia
- **Diseño Responsive** moderno
- **Notificaciones en Tiempo Real**

## 📋 Requisitos del Sistema

- **Node.js** v16.0.0 o superior
- **MongoDB** v4.4 o superior
- **NPM** v8.0.0 o superior
- **Navegador Web** moderno (Chrome, Firefox, Safari, Edge)

## 🛠️ Instalación

### Paso 1: Clonar o Descargar el Proyecto

```bash
# Si tienes el proyecto en Git
git clone <tu-repositorio-url>
cd moon-studios-dashboard

# O si descargaste los archivos, navega al directorio
cd moon-studios-dashboard
```

### Paso 2: Instalar Dependencias

```bash
npm install
```

### Paso 3: Configurar Base de Datos MongoDB

#### Opción A: MongoDB Local

1. **Instalar MongoDB Community Edition:**
   - Windows: Descargar desde [mongodb.com](https://www.mongodb.com/try/download/community)
   - macOS: `brew install mongodb-community`
   - Ubuntu: `sudo apt install mongodb`

2. **Iniciar MongoDB:**
   ```bash
   # Windows (como servicio)
   net start MongoDB
   
   # macOS/Linux
   sudo systemctl start mongod
   # o
   mongod
   ```

#### Opción B: MongoDB Atlas (Nube)

1. Crear cuenta en [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Crear un cluster gratuito
3. Obtener la cadena de conexión
4. Actualizar `.env` con tu URI de Atlas

### Paso 4: Configurar Variables de Entorno

Crear archivo `.env` en la raíz del proyecto:

```bash
# Configuración del Servidor
PORT=3000
NODE_ENV=development

# Base de Datos MongoDB
MONGODB_URI=mongodb://localhost:27017/moonstudios
# Para MongoDB Atlas usar: mongodb+srv://usuario:password@cluster.mongodb.net/moonstudios

# JWT Secret (CAMBIAR EN PRODUCCIÓN)
JWT_SECRET=moon_studios_super_secret_key_2025_secure_production

# Configuración de la Empresa
COMPANY_NAME=Moon Studios
COMPANY_EMAIL=admin@moonstudios.com

# CEO por defecto
DEFAULT_CEO_EMAIL=ceo@moonstudios.com
DEFAULT_CEO_PASSWORD=admin123

# Configuraciones adicionales
TIMEZONE=America/Lima
CURRENCY=PEN
LANGUAGE=es
```

### Paso 5: Estructura de Directorios

Crear la siguiente estructura de carpetas:

```
moon-studios-dashboard/
├── server.js                 # Servidor principal
├── package.json             # Dependencias
├── .env                     # Variables de entorno
├── README.md               # Este archivo
├── public/                 # Archivos estáticos
│   ├── index.html         # Redirección al login
│   ├── css/               # Estilos CSS
│   │   ├── login.css
│   │   ├── dashboard.css
│   │   └── ceo.css
│   ├── js/                # JavaScript
│   │   ├── login.js
│   │   ├── dashboard-common.js
│   │   └── ceo-dashboard.js
│   ├── dashboard/         # Dashboards específicos
│   │   ├── ceo.html
│   │   ├── ventas.html
│   │   ├── database.html
│   │   ├── admin.html
│   │   ├── soporte.html
│   │   └── ilustracion.html
│   ├── assets/           # Recursos
│   │   └── logo.png
│   └── login.html       # Página de login
└── uploads/             # Archivos subidos
```

### Paso 6: Crear Archivos Faltantes

#### public/index.html
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Moon Studios</title>
</head>
<body>
    <script>
        // Redireccionar al login
        window.location.href = '/login.html';
    </script>
</body>
</html>
```

## 🏃‍♂️ Ejecutar el Sistema

### Desarrollo

```bash
# Opción 1: Con nodemon (recomendado para desarrollo)
npm run dev

# Opción 2: Node estándar
npm start
```

### Producción

```bash
# Configurar NODE_ENV en .env
NODE_ENV=production

# Ejecutar
npm start
```

El servidor estará disponible en: `http://localhost:3000`

## 👤 Acceso al Sistema

### CEO (Administrador Principal)
- **Email:** `ceo@moonstudios.com`
- **Contraseña:** `admin123`
- **Acceso:** Dashboard completo con todos los permisos

### Cuentas de Demostración

El sistema incluye botones de demo en el login para pruebas rápidas:

1. **CEO** - Control total del sistema
2. **Ventas** - Panel de ventas y clientes
3. **Soporte** - Panel de atención al cliente

## 🔧 Configuración Inicial

### 1. Primer Login como CEO

1. Ir a `http://localhost:3000`
2. Usar credenciales del CEO
3. Cambiar contraseña por defecto
4. Configurar información de la empresa

### 2. Agregar Empleados

1. En el Dashboard CEO → Sección "Empleados"
2. Hacer clic en "Agregar Empleado"
3. Llenar formulario con datos del empleado
4. Asignar módulo correspondiente
5. El empleado recibirá credenciales temporales

### 3. Personalizar Empresa

1. Dashboard CEO → Configuración
2. Cambiar nombre de empresa
3. Subir logo personalizado
4. Ajustar zona horaria y moneda

### 4. Crear Tareas

1. Dashboard CEO → Tareas
2. "Crear Tarea"
3. Asignar a empleado específico
4. Establecer prioridad y fecha límite

### 5. Gestionar Calendario

1. Dashboard CEO → Calendario
2. "Agregar Evento"
3. Configurar eventos globales
4. Todos los empleados verán eventos en sus paneles

## 📊 Módulos Disponibles

### 1. Ventas
- Gestión de clientes
- Seguimiento de oportunidades
- Reportes de ingresos
- Pipeline de ventas

### 2. Base de Datos
- Administración de datos
- Backup y restore
- Consultas personalizadas
- Mantenimiento

### 3. Administración
- Configuración del sistema
- Gestión de usuarios
- Políticas y procedimientos
- Reportes administrativos

### 4. Soporte
- Tickets de soporte
- Base de conocimientos
- Chat en vivo
- Métricas de satisfacción

### 5. Ilustración
- Proyectos creativos
- Galería de trabajos
- Gestión de assets
- Colaboración creativa

## 🔒 Seguridad

### Características de Seguridad Implementadas

- **JWT Tokens** para autenticación
- **Contraseñas hasheadas** con bcrypt
- **Validación de permisos** por rol
- **Sanitización de datos** de entrada
- **CORS** configurado
- **Rate limiting** básico

### Recomendaciones de Producción

1. **Cambiar JWT_SECRET** en `.env`
2. **Usar HTTPS** en producción
3. **Configurar firewall** para MongoDB
4. **Implementar backup** regular
5. **Monitorear logs** de seguridad

## 🚀 Despliegue en Producción

### Opción 1: Servidor VPS/Cloud

```bash
# Instalar PM2 para gestión de procesos
npm install -g pm2

# Iniciar aplicación
pm2 start server.js --name "moon-studios"

# Configurar PM2 para auto-inicio
pm2 startup
pm2 save
```

### Opción 2: Docker

```dockerfile
# Dockerfile
FROM node:16

WORKDIR /app
COPY package*.json ./
RUN npm install --production

COPY . .

EXPOSE 3000
CMD ["npm", "start"]
```

```bash
# Construir y ejecutar
docker build -t moon-studios .
docker run -p 3000:3000 moon-studios
```

### Opción 3: Heroku

1. Crear cuenta en Heroku
2. Instalar Heroku CLI
3. Configurar variables de entorno
4. Desplegar:

```bash
heroku create moon-studios-app
heroku config:set NODE_ENV=production
heroku config:set MONGODB_URI=tu_mongodb_atlas_uri
heroku config:set JWT_SECRET=tu_jwt_secret_seguro
git push heroku main
```

## 🛠️ Mantenimiento

### Backup de Base de Datos

```bash
# Crear backup
mongodump --db moonstudios --out backup/

# Restaurar backup
mongorestore backup/moonstudios/
```

### Logs del Sistema

```bash
# Ver logs en tiempo real
tail -f logs/app.log

# Con PM2
pm2 logs moon-studios
```

### Actualización del Sistema

```bash
# Detener aplicación
pm2 stop moon-studios

# Actualizar código
git pull origin main
npm install

# Reiniciar
pm2 start moon-studios
```

## 🐛 Solución de Problemas

### Error: "Cannot connect to MongoDB"

1. Verificar que MongoDB esté ejecutándose
2. Comprobar URI en `.env`
3. Verificar conectividad de red
4. Revisar logs de MongoDB

### Error: "JWT token invalid"

1. Limpiar localStorage del navegador
2. Verificar JWT_SECRET en `.env`
3. Re-hacer login

### Error: "Port 3000 already in use"

```bash
# Cambiar puerto en .env
PORT=3001

# O matar proceso existente
lsof -ti:3000 | xargs kill -9
```

### Problemas de Permisos

1. Verificar rol del usuario en base de datos
2. Comprobar token de autenticación
3. Revisar middleware de autorización

## 📞 Soporte

Para soporte técnico:

- **Email:** admin@moonstudios.com
- **Documentación:** Este README
- **Logs:** Revisar archivos de log del sistema

## 📝 Changelog

### v1.0.0 (2025-06-11)
- ✅ Sistema de autenticación completo
- ✅ Dashboard CEO funcional
- ✅ Gestión de empleados
- ✅ Sistema de tareas
- ✅ Calendario global
- ✅ 5 módulos especializados
- ✅ Base de datos MongoDB
- ✅ Diseño responsive

## 📄 Licencia

Este proyecto está bajo licencia MIT. Ver archivo `LICENSE` para más detalles.

---

**Moon Studios Dashboard v1.0.0**  
Desarrollado con ❤️ para tu empresa
