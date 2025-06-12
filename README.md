# 📁 ESTRUCTURA DE ARCHIVOS - MOON STUDIOS

## 🗂️ **ORGANIZACIÓN COMPLETA DEL PROYECTO**

```
moon-studios-dashboard/
│
├── 📄 server.js                    # ✅ Servidor principal Node.js
├── 📄 package.json                 # ✅ Dependencias
├── 📄 .env                         # ✅ Variables de entorno
├── 📄 setup-demo-data.js           # ✅ Datos de demostración
├── 📄 README.md                    # ✅ Documentación
├── 📄 QUICK-START.md               # ✅ Inicio rápido
│
├── 📁 public/                      # Frontend completo
│   │
│   ├── 📄 index.html               # → Redirección automática a login
│   ├── 📄 login.html               # ✅ Sistema de login principal
│   │
│   ├── 📁 dashboard/               # Paneles de usuario
│   │   ├── 📄 ceo.html             # ✅ Dashboard CEO (completo)
│   │   ├── 📄 ventas.html          # ✅ Panel Ventas
│   │   ├── 📄 database.html        # 🔥 Panel Base de Datos (nuevo)
│   │   ├── 📄 admin.html           # 🔥 Panel Administración (nuevo)
│   │   ├── 📄 soporte.html         # ✅ Panel Soporte
│   │   └── 📄 ilustracion.html     # 🔥 Panel Ilustración (nuevo)
│   │
│   ├── 📁 css/                     # Estilos
│   │   ├── 📄 login.css            # ✅ Estilos login
│   │   ├── 📄 dashboard.css        # ✅ Estilos comunes
│   │   ├── 📄 ceo.css              # ✅ Estilos CEO
│   │   ├── 📄 employee.css         # ✅ Estilos empleados
│   │   ├── 📄 support.css          # ✅ Estilos soporte
│   │   ├── 📄 database.css         # 🔥 Estilos base datos (nuevo)
│   │   ├── 📄 admin.css            # 🔥 Estilos admin (nuevo)
│   │   └── 📄 ilustracion.css      # 🔥 Estilos ilustración (nuevo)
│   │
│   ├── 📁 js/                      # JavaScript
│   │   ├── 📄 login.js             # ✅ Lógica login
│   │   ├── 📄 dashboard-common.js  # ✅ Funciones comunes
│   │   ├── 📄 ceo-dashboard.js     # ✅ Lógica CEO
│   │   ├── 📄 employee-dashboard.js # ✅ Lógica base empleados
│   │   ├── 📄 support-dashboard.js # ✅ Lógica soporte
│   │   ├── 📄 database-dashboard.js # 🔥 Lógica base datos (nuevo)
│   │   ├── 📄 admin-dashboard.js   # 🔥 Lógica admin (nuevo)
│   │   └── 📄 ilustracion-dashboard.js # 🔥 Lógica ilustración (nuevo)
│   │
│   └── 📁 assets/                  # Recursos
│       ├── 📄 logo.png             # Logo empresa
│       ├── 📁 profiles/            # 🔥 Fotos de perfil (nuevo)
│       │   ├── 📄 default-avatar.png
│       │   └── 📄 [user-id].jpg
│       └── 📁 uploads/             # Archivos subidos
│
├── 📁 uploads/                     # Archivos del servidor
│   ├── 📁 profiles/                # Fotos de perfil usuarios
│   └── 📁 company/                 # Archivos de empresa
│
└── 📁 logs/                        # Logs del sistema
    ├── 📄 app.log                  # Log principal
    ├── 📄 error.log                # Errores
    └── 📄 access.log               # Accesos
```

## 🎯 **CÓMO FUNCIONA EL SISTEMA:**

### 🔄 **Flujo de Login:**
```
1. Usuario va a → http://localhost:3000
2. index.html → Redirecciona a login.html
3. login.html → Valida credenciales con server.js
4. server.js → Verifica rol y envía token JWT
5. login.js → Redirecciona según rol:
   - CEO → dashboard/ceo.html
   - Ventas → dashboard/ventas.html
   - Base de Datos → dashboard/database.html
   - Administración → dashboard/admin.html
   - Soporte → dashboard/soporte.html
   - Ilustración → dashboard/ilustracion.html
```

### 📊 **Arquitectura de Dashboard:**
```
Cada panel carga:
1. HTML específico (dashboard/[rol].html)
2. CSS común (css/dashboard.css) + CSS específico
3. JS común (js/dashboard-common.js) + JS específico
4. Datos desde server.js via APIs REST
```

## 🔧 **CONFIGURACIÓN PARA QUE FUNCIONE:**

### 1. **Instalar dependencias:**
```bash
npm install
```

### 2. **Configurar MongoDB:**
```bash
# En .env:
MONGODB_URI=mongodb://localhost:27017/moonstudios
# O tu string de MongoDB Atlas
```

### 3. **Crear datos iniciales:**
```bash
node setup-demo-data.js
```

### 4. **Iniciar servidor:**
```bash
npm run dev
```

### 5. **Acceder:**
```
http://localhost:3000 → Login → Dashboard correspondiente
```

## 🎨 **PERSONALIZACIÓN:**

### **Cambiar colores:**
Editar `css/dashboard.css` líneas 1-10:
```css
:root {
    --primary-color: #tu-color;
    --secondary-color: #tu-color;
}
```

### **Agregar nuevos módulos:**
1. Crear `dashboard/nuevo-modulo.html`
2. Crear `css/nuevo-modulo.css`
3. Crear `js/nuevo-modulo-dashboard.js`
4. Agregar rol en `server.js` enum

### **Fotos de perfil:**
- Suben a `/uploads/profiles/`
- Se muestran en todos los dashboards
- Fallback a avatar por defecto

## ✅ **VERIFICAR QUE FUNCIONE:**

### 🔍 **Checklist:**
- [ ] Servidor inicia sin errores
- [ ] Login redirecciona correctamente
- [ ] Cada panel carga sus datos
- [ ] Fotos de perfil funcionan
- [ ] APIs responden correctamente
- [ ] Base de datos conecta

### 🐛 **Solución problemas:**
```bash
# Ver logs del servidor
tail -f logs/app.log

# Verificar MongoDB
mongo mongodb://localhost:27017/moonstudios

# Probar APIs
curl http://localhost:3000/api/auth/verify
```

---

**¡Con esta estructura todo va a funcionar perfectamente! 🎯**
