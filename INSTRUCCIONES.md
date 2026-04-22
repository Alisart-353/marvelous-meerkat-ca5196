# ⚡ CUENTOS ÉPICOS — Guía de Despliegue en Netlify

## Estructura del proyecto

```
cuentos-epicos/
├── index.html                  ← La app completa
├── netlify.toml                ← Configuración de Netlify
├── netlify/
│   └── functions/
│       └── claude.js           ← Función serverless (proxy seguro a la API)
└── INSTRUCCIONES.md            ← Este archivo
```

---

## Paso 1 — Obtener tu API Key de Anthropic

1. Ve a https://console.anthropic.com
2. Inicia sesión o crea una cuenta
3. Ve a **API Keys** → **Create Key**
4. Copia la clave (empieza con `sk-ant-...`)
5. ¡Guárdala en un lugar seguro! Solo se muestra una vez.

---

## Paso 2 — Subir el proyecto a GitHub

1. Ve a https://github.com y crea una cuenta si no tienes
2. Clic en **New repository** → ponle nombre (ej: `cuentos-epicos`)
3. Sube los archivos: arrastra la carpeta `cuentos-epicos/` completa
   O usa Git desde terminal:
   ```bash
   git init
   git add .
   git commit -m "primera versión"
   git remote add origin https://github.com/TU_USUARIO/cuentos-epicos.git
   git push -u origin main
   ```

---

## Paso 3 — Desplegar en Netlify

1. Ve a https://netlify.com y crea una cuenta (gratis)
2. Clic en **Add new site** → **Import an existing project**
3. Conecta con GitHub y selecciona tu repositorio `cuentos-epicos`
4. En la configuración de build:
   - **Build command**: (dejar vacío)
   - **Publish directory**: `.` (un punto)
5. Clic en **Deploy site**

---

## Paso 4 — Configurar la API Key (¡MUY IMPORTANTE!)

1. En tu sitio de Netlify, ve a **Site configuration** → **Environment variables**
2. Clic en **Add a variable**
3. Ingresa exactamente:
   - **Key**: `ANTHROPIC_API_KEY`
   - **Value**: tu clave `sk-ant-...`
4. Clic en **Save**
5. Ve a **Deploys** → **Trigger deploy** → **Deploy site** para redeployar

---

## Paso 5 — ¡Probar!

1. Netlify te dará una URL como `https://cuentos-epicos-abc123.netlify.app`
2. Abre esa URL en el navegador o celular
3. Haz clic en "¡ENTRAR AL UNIVERSO!" y genera tu primer cuento

---

## ¿Por qué esta arquitectura?

```
Navegador del niño
      ↓  (llama a /api/claude)
Netlify Function (claude.js)
      ↓  (agrega la API Key de forma segura)
API de Anthropic
      ↓  (devuelve el cuento generado)
Navegador del niño
```

La API Key **nunca** queda expuesta en el código del navegador.
Solo existe en las variables de entorno de Netlify, que son privadas.

---

## Costos estimados

- **Netlify**: Gratis (plan gratuito cubre esta app)
- **Anthropic API**: ~$0.003 por cuento generado (muy barato)
  - 1,000 cuentos ≈ $3 USD

---

## ¿Problemas?

- **"Error al generar"**: Verifica que la variable `ANTHROPIC_API_KEY` esté correctamente configurada y que hayas redesplegado.
- **"Function not found"**: Asegúrate de que el archivo `netlify/functions/claude.js` esté en esa ruta exacta.
- **La app no carga**: Verifica que `index.html` esté en la raíz del repositorio.
