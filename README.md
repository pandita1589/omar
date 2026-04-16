# 🚀 Guía de Configuración - Sistema Electoral Moon Studios

## 📋 Índice

1. [Configuración Firebase](#1-configuración-firebase)
2. [Configuración Supabase](#2-configuración-supabase)
3. [Configuración Discord OAuth](#3-configuración-discord-oauth)
4. [Variables de Entorno](#4-variables-de-entorno)
5. [Firestore Rules](#5-firestore-rules)
6. [Despliegue](#6-despliegue)

---

## 1. Configuración Firebase

### Paso 1.1: Crear Proyecto Firebase

1. Ir a [Firebase Console](https://console.firebase.google.com/)
2. Click en "Agregar proyecto"
3. Nombre del proyecto: `moon-studios-electoral`
4. Desactivar Google Analytics (opcional)
5. Click en "Crear proyecto"

### Paso 1.2: Activar Authentication

1. En el menú lateral, ir a "Build" → "Authentication"
2. Click en "Comenzar"
3. Ir a la pestaña "Sign-in method"
4. Habilitar "Discord" (ver sección 3)
5. Habilitar "Email/Password" (opcional para 2FA)

### Paso 1.3: Configurar Firestore

1. En el menú lateral, ir a "Build" → "Firestore Database"
2. Click en "Crear base de datos"
3. Seleccionar "Comenzar en modo de prueba" (luego cambiar a producción)
4. Elegir ubicación: `us-central` (o la más cercana)
5. Click en "Activar"

### Paso 1.4: Obtener Credenciales

1. Ir a "Configuración del proyecto" (icono de engranaje)
2. En la pestaña "General", bajar a "Tus aplicaciones"
3. Click en "</>" para crear una aplicación web
4. Registrar app con nombre: `moon-electoral-web`
5. **Copiar las credenciales** que aparecen:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "tu-proyecto.firebaseapp.com",
  projectId: "tu-proyecto",
  storageBucket: "tu-proyecto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

---

## 2. Configuración Supabase

### Paso 2.1: Crear Proyecto Supabase

1. Ir a [Supabase](https://supabase.com/)
2. Click en "New Project"
3. Organización: Crear nueva o seleccionar existente
4. Nombre del proyecto: `moon-electoral-storage`
5. Database Password: Generar contraseña segura
6. Región: Seleccionar la más cercana
7. Click en "Create new project"

### Paso 2.2: Crear Bucket de Imágenes

1. En el menú lateral, ir a "Storage"
2. Click en "New bucket"
3. Nombre del bucket: `candidates`
4. Click en "Save"

### Paso 2.3: Configurar Políticas del Bucket

1. Dentro del bucket `candidates`, ir a "Policies"
2. Click en "New policy"
3. Seleccionar "For full customization, create a policy from scratch"
4. Crear las siguientes políticas:

**Política SELECT (lectura pública):**
```sql
CREATE POLICY "Public Access" ON storage.objects
  FOR SELECT USING (bucket_id = 'candidates');
```

**Política INSERT (usuarios autenticados):**
```sql
CREATE POLICY "Authenticated Upload" ON storage.objects
  FOR INSERT WITH CHECK (
    bucket_id = 'candidates' AND 
    auth.role() = 'authenticated'
  );
```

**Política DELETE (solo admins):**
```sql
CREATE POLICY "Admin Delete" ON storage.objects
  FOR DELETE USING (
    bucket_id = 'candidates' AND 
    auth.uid() IN (
      SELECT uid FROM users WHERE role IN ('admin', 'superadmin')
    )
  );
```

### Paso 2.4: Obtener Credenciales

1. Ir a "Project Settings" (icono de engranaje)
2. Seleccionar "API"
3. Copiar:
   - **URL**: `https://tu-proyecto.supabase.co`
   - **anon/public**: `eyJhbGciOiJIUzI1NiIs...`

---

## 3. Configuración Discord OAuth

### Paso 3.1: Crear Aplicación Discord

1. Ir a [Discord Developer Portal](https://discord.com/developers/applications)
2. Click en "New Application"
3. Nombre: `Moon Studios Electoral`
4. Click en "Create"

### Paso 3.2: Configurar OAuth2

1. En el menú lateral, ir a "OAuth2" → "General"
2. En "Redirects", agregar:
   - Producción: `https://tu-dominio.com/__/auth/handler`
   - Desarrollo: `http://localhost:5173/__/auth/handler`
3. Click en "Save Changes"

### Paso 3.3: Obtener Credenciales

1. En "OAuth2" → "General", copiar:
   - **CLIENT ID**: `1234567890123456789`
   - **CLIENT SECRET**: Click en "Reset Secret" y copiar

### Paso 3.4: Configurar en Firebase

1. Volver a Firebase Console
2. Ir a "Authentication" → "Sign-in method"
3. Buscar "Discord" y click en "Editar"
4. Activar el proveedor
5. Pegar:
   - Client ID de Discord
   - Client Secret de Discord
6. Click en "Guardar"

---

## 4. Variables de Entorno

### Paso 4.1: Crear Archivo .env

En la raíz del proyecto, crear archivo `.env`:

```bash
cp .env.example .env
```

### Paso 4.2: Configurar Variables

Editar el archivo `.env` con tus credenciales:

```env
# ============================================
# Firebase Configuration
# ============================================
VITE_FIREBASE_API_KEY=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
VITE_FIREBASE_AUTH_DOMAIN=tu-proyecto.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=tu-proyecto
VITE_FIREBASE_STORAGE_BUCKET=tu-proyecto.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123456789:web:abcdef

# ============================================
# Supabase Configuration
# ============================================
VITE_SUPABASE_URL=https://tu-proyecto.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## 5. Firestore Rules

### Paso 5.1: Configurar Reglas de Seguridad

1. Ir a Firebase Console → Firestore Database
2. Click en "Reglas"
3. Reemplazar con las siguientes reglas:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Función auxiliar para verificar rol de admin
    function isAdmin() {
      return request.auth != null && 
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role in ['admin', 'superadmin'];
    }
    
    // Función auxiliar para verificar autenticación
    function isAuthenticated() {
      return request.auth != null;
    }
    
    // Función auxiliar para verificar propiedad del recurso
    function isOwner(userId) {
      return request.auth != null && request.auth.uid == userId;
    }
    
    // ==========================================
    // COLECCIÓN: users
    // ==========================================
    match /users/{userId} {
      // Lectura: propio o admin
      allow read: if isOwner(userId) || isAdmin();
      
      // Creación: autenticado (solo Firebase Auth puede crear)
      allow create: if isAuthenticated();
      
      // Actualización: propio o admin
      allow update: if isOwner(userId) || isAdmin();
      
      // Eliminación: solo admin
      allow delete: if isAdmin();
    }
    
    // ==========================================
    // COLECCIÓN: elections
    // ==========================================
    match /elections/{electionId} {
      // Lectura: cualquier usuario autenticado
      allow read: if isAuthenticated();
      
      // Escritura: solo admins
      allow create, update, delete: if isAdmin();
    }
    
    // ==========================================
    // COLECCIÓN: candidates
    // ==========================================
    match /candidates/{candidateId} {
      // Lectura: cualquier usuario autenticado
      allow read: if isAuthenticated();
      
      // Creación: cualquier usuario autenticado
      allow create: if isAuthenticated();
      
      // Actualización: propietario, admin, o moderador
      allow update: if isAuthenticated() && (
        resource.data.uid == request.auth.uid || 
        isAdmin() ||
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'moderator'
      );
      
      // Eliminación: propietario o admin
      allow delete: if isAuthenticated() && (
        resource.data.uid == request.auth.uid || isAdmin()
      );
    }
    
    // ==========================================
    // COLECCIÓN: votes
    // ==========================================
    match /votes/{voteId} {
      // Lectura: solo admins (para auditoría)
      allow read: if isAdmin();
      
      // Creación: usuarios autenticados (una sola vez)
      allow create: if isAuthenticated() && 
        request.resource.data.voterId == request.auth.uid;
      
      // No permitir actualización ni eliminación
      allow update, delete: if false;
    }
    
    // ==========================================
    // COLECCIÓN: voteRecords
    // ==========================================
    match /voteRecords/{recordId} {
      // Lectura: propietario o admin
      allow read: if isAuthenticated() && (
        resource.data.voterId == request.auth.uid || isAdmin()
      );
      
      // Creación/Actualización: propietario
      allow create, update: if isAuthenticated() && 
        request.resource.data.voterId == request.auth.uid;
      
      // Eliminación: solo admin
      allow delete: if isAdmin();
    }
    
    // ==========================================
    // COLECCIÓN: auditLogs
    // ==========================================
    match /auditLogs/{logId} {
      // Lectura: solo admins
      allow read: if isAdmin();
      
      // Escritura: solo admins (via Cloud Functions)
      allow create: if isAdmin();
      
      // No permitir actualización ni eliminación
      allow update, delete: if false;
    }
  }
}
```

4. Click en "Publicar"

---

## 6. Despliegue

### Paso 6.1: Instalar Dependencias

```bash
npm install
```

### Paso 6.2: Compilar para Producción

```bash
npm run build
```

### Paso 6.3: Desplegar en Vercel

```bash
# Instalar Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
vercel --prod
```

### Paso 6.4: Configurar Variables en Vercel

1. Ir a [Vercel Dashboard](https://vercel.com/dashboard)
2. Seleccionar el proyecto
3. Ir a "Settings" → "Environment Variables"
4. Agregar todas las variables del archivo `.env`
5. Re-deploy: `vercel --prod`

---

## ✅ Checklist de Configuración

- [ ] Proyecto Firebase creado
- [ ] Authentication activado (Discord)
- [ ] Firestore Database creada
- [ ] Proyecto Supabase creado
- [ ] Bucket "candidates" creado
- [ ] Políticas de Storage configuradas
- [ ] Aplicación Discord creada
- [ ] OAuth2 configurado en Discord
- [ ] Discord OAuth configurado en Firebase
- [ ] Variables de entorno configuradas
- [ ] Firestore Rules publicadas
- [ ] Build exitoso
- [ ] Deploy completado

---

## 🆘 Solución de Problemas

### Error: "auth/invalid-api-key"
- Verificar que `VITE_FIREBASE_API_KEY` sea correcta
- Regenerar en Firebase Console → Configuración → Web API Key

### Error: "auth/popup-closed-by-user"
- Permitir popups en el navegador
- Verificar que el redirect URI de Discord sea correcto

### Error: "storage/unauthorized"
- Verificar políticas de Supabase Storage
- Asegurar que el usuario esté autenticado

### Error: "permission-denied" en Firestore
- Verificar Firestore Rules
- Asegurar que el usuario tenga el rol correcto

---

## 📞 Soporte

Para soporte técnico, contactar al equipo de desarrollo de Moon Studios.

**Versión del Sistema**: 1.0.0  
**Última Actualización**: Abril 2025
