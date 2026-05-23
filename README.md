# 🤖 Asistente de Campaña con IA — José Antonio Kast

Asistente conversacional basado en **Claude (Anthropic)** que permite a ciudadanos, periodistas y equipos de campaña consultar sobre el programa de gobierno, propuestas y posiciones de José Antonio Kast.

Desarrollado por **[Aidtogrow](https://www.aidtogrow.com/es/)** Daniel Olivares como demostración del servicio de asistentes IA para candidatos políticos en América Latina.

---

## 🖥️ Demo en vivo

> Agrega el link de tu deploy aquí después de publicar en Vercel

---

## 🏗️ Arquitectura

```
Usuario (navegador)
    ↕  chat UI (HTML/CSS/JS vanilla)
/public/index.html
    ↕  POST /api/chat
/api/chat.js  (Vercel Serverless Function)
    ↕  Anthropic API (claude-sonnet-4-20250514)
    ↑  System prompt con programa de gobierno
```

- **Frontend:** HTML + CSS + JavaScript vanilla — sin dependencias, sin frameworks
- **Backend:** Vercel API Route (serverless) en Node.js — protege la API Key
- **Modelo:** `claude-sonnet-4-20250514` de Anthropic
- **Deploy:** Vercel (gratis para proyectos pequeños)

---

## 📁 Estructura del proyecto

```
kast-asistente/
├── api/
│   └── chat.js          # Serverless function: proxy hacia Anthropic API
├── public/
│   └── index.html       # Frontend completo del chat
├── .env.example         # Variables de entorno necesarias
├── .gitignore
├── package.json
├── vercel.json          # Configuración de routing en Vercel
└── README.md
```

---

## 🚀 Deploy en Vercel (paso a paso)

### Prerequisitos
- Cuenta en [GitHub](https://github.com)
- Cuenta en [Vercel](https://vercel.com) (gratis)
- API Key de [Anthropic Console](https://console.anthropic.com)

### 1. Sube el proyecto a GitHub

```bash
# Inicializa el repositorio
git init
git add .
git commit -m "feat: asistente de campaña Kast con IA"

# Crea un nuevo repo en github.com y luego:
git remote add origin https://github.com/TU_USUARIO/kast-asistente.git
git push -u origin main
```

### 2. Conecta con Vercel

1. Ve a [vercel.com](https://vercel.com) → **Add New Project**
2. Importa tu repositorio de GitHub
3. Vercel detecta automáticamente la configuración

### 3. Agrega la variable de entorno

En Vercel → tu proyecto → **Settings → Environment Variables**:

| Name | Value |
|------|-------|
| `ANTHROPIC_API_KEY` | `sk-ant-api03-...` |

### 4. Deploy

Haz click en **Deploy** — en 1-2 minutos tu asistente estará en vivo.

Cada `git push` al branch `main` hace un nuevo deploy automático.

---

## 💻 Desarrollo local

```bash
# Clona el repo
git clone https://github.com/TU_USUARIO/kast-asistente.git
cd kast-asistente

# Instala Vercel CLI
npm install

# Crea tu archivo de variables locales
cp .env.example .env.local
# Edita .env.local y agrega tu API Key real

# Inicia el servidor de desarrollo
npx vercel dev
# → Abre http://localhost:3000
```

---

## 🔧 Personalización

### Cambiar el candidato

Todo el conocimiento del candidato vive en el **system prompt** al inicio de `/api/chat.js`. Para adaptar a otro candidato:

1. Reemplaza el contenido de la variable `SYSTEM` en `api/chat.js`
2. Actualiza el nombre y los colores en `public/index.html`
3. Modifica las preguntas sugeridas en el `<div class="suggestions">`

### Cambiar colores

En `public/index.html`, edita las variables CSS en `:root`:

```css
:root {
  --red: #CC1818;       /* Color principal */
  --red-dark: #aa1010;  /* Hover */
  --red-light: #fbeaea; /* Fondos suaves */
}
```

### Agregar más contexto

Para documentos más largos (programa completo, declaraciones, entrevistas), considera implementar **RAG** (Retrieval Augmented Generation):
1. Divide el documento en chunks de ~500 palabras
2. Almacena embeddings en una base vectorial (Supabase pgvector, Pinecone)
3. Antes de llamar a Claude, busca los chunks más relevantes a la pregunta
4. Inyéctalos dinámicamente en el system prompt

---

## 🔐 Seguridad

- La API Key **nunca** se expone en el frontend — siempre viaja desde el servidor
- El archivo `.env.local` está en `.gitignore` — nunca se sube a GitHub
- Vercel almacena las variables de entorno de forma cifrada
- Para producción, considera agregar rate limiting en `/api/chat.js`

---

## 📊 Analíticas opcionales (con Supabase)

Para registrar las preguntas y analizar qué consulta la gente, agrega esto en `api/chat.js`:

```js
import { createClient } from '@supabase/supabase-js';
const supabase = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_KEY);

// Dentro del handler, después de obtener la respuesta:
await supabase.from('conversaciones').insert({
  pregunta: messages[messages.length - 1].content,
  respuesta: data.content[0].text,
  timestamp: new Date().toISOString(),
});
```

---

## 🧩 Tecnologías usadas

| Capa | Tecnología |
|------|-----------|
| Frontend | HTML5 + CSS3 + JavaScript vanilla |
| Backend | Node.js (Vercel Serverless) |
| IA | Claude Sonnet 4 (Anthropic) |
| Hosting | Vercel |
| Fuente tipográfica | Google Fonts (Inter) |

---

## 🤝 Sobre Aidtogrow

Este proyecto es una demostración del servicio **Asistente de Candidato IA** de [Aidtogrow SPA](https://www.aidtogrow.com/es/), empresa chilena especializada en agentes conversacionales para América Latina.

**¿Quieres un asistente similar para tu candidato o campaña?**
Contáctanos en [aidtogrow.com](https://www.aidtogrow.com/es/)

---

## 📄 Licencia

MIT — libre para usar, modificar y distribuir con atribución.
