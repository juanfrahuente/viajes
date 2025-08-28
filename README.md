# TripBoard — Viajes en Pareja (Checklist + Mensajes + Recordatorios + Calendario)

Web app estática, gratis y fácil de desplegar (Netlify / GitHub Pages).

## 🚀 Qué incluye
- Autenticación (correo/contraseña o Google) vía Firebase.
- Compartir viajes con un **código** (ambos ven y editan).
- **Checklist** colaborativa, **mensajes** tipo chat, **recordatorios** con fecha/hora.
- **Calendario** (FullCalendar) que muestra rango del viaje y recordatorios.
- **Enlaces** (vuelos, hoteles, documentos).
- **Tips** esenciales de viaje.
- Diseño con Tailwind.

## 🧩 Requisitos
- Cuenta de Firebase (gratuita).

## 🛠️ Configuración Firebase (5 min)
1. Ve a https://console.firebase.google.com y crea un **Proyecto**.
2. En «Visión general del proyecto» → “Agregar app” → **Web**.
3. Copia la **configuración** (apiKey, authDomain, projectId, etc.).
4. En Firestore: Crea la base de datos en modo producción.
5. En Authentication: Habilita **Email/Password** y **Google** (opcional).

### 🔒 Reglas de seguridad Firestore (pegar en la pestaña `Rules`)
```
// Permite acceso solo a miembros del viaje
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /trips/{tripId} {
      allow read, write: if request.auth != null && request.auth.uid in resource.data.members;
    }
    match /trips/{tripId}/{collection=**}/{doc} {
      allow read, write: if request.auth != null &&
        request.auth.uid in get(/databases/$(database)/documents/trips/$(tripId)).data.members;
    }
  }
}
```

## 🧪 Pegar configuración en el código
Abre `index.html` y reemplaza el objeto `firebaseConfig`:
```js
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_AUTH_DOMAIN",
  projectId: "TU_PROJECT_ID",
  storageBucket: "TU_BUCKET",
  messagingSenderId: "TU_SENDER_ID",
  appId: "TU_APP_ID"
};
```

## 🌐 Despliegue Gratis (elige 1)
### Opción A — Netlify (más fácil)
1. Ve a https://app.netlify.com/drop
2. Arrastra y suelta **esta carpeta** (o el ZIP). ¡Listo!
3. Copia la URL pública y compártela con tu pareja.

### Opción B — GitHub Pages
1. Crea un repo nuevo con este `index.html`.
2. Sube el archivo y ve a Settings → Pages.
3. “Source”: selecciona la rama `main` y carpeta `/root`.
4. Espera a que aparezca la URL pública.

## 🧭 Uso básico
- Crea tu cuenta e inicia sesión.
- Clic en **+ Nuevo** para crear un viaje (nombre/destino/fechas). Se genera un **código**.
- Tu pareja va a “Unirse con código” e ingresa ese código.
- Usa las pestañas: **Checklist**, **Mensajes**, **Recordatorios**, **Calendario**, **Enlaces**, **Tips**.

## 🔔 Notas
- Los recordatorios se muestran en el calendario. (Para notificaciones push necesitarías FCM + service worker, opcional).
- Todo queda sincronizado en tiempo real con Firestore.
- Si algo no carga, revisa la consola del navegador (F12).

¡Buen viaje!
