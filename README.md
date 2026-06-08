# Mi Dinero — South Beach (v27)

Gestor de gastos e ingresos para trabajo con propinas. Maneja dos billeteras (**Banco** y **Efectivo**), registro de gastos por categoría, ingresos (comisión/horas/tips + cheques con estimación de impuestos), gráficas de tendencia y metas con fecha.

App estática de un solo archivo + soporte **PWA** (instalable en el teléfono y funciona sin internet). No necesita build ni servidor.

---

## Cómo funciona el dinero

- 💵 **Tips cash** (registro diario) → suman al **Efectivo**.
- 🏦 **Cheques cobrados** → suman al **Banco**.
- 🧾 **Gastos** → restan de la billetera que elijas (Efectivo o Banco).
- ✏️ Puedes **editar Banco/Efectivo a mano** cuando quieras; la app ajusta la base sin perder lo que entró por tips o cheques.
- **Total disponible = Banco + Efectivo.** Es el número que llena tus metas.

La parte de comisión/horas y la estimación de impuestos del cheque es solo **análisis/predicción** y NO se suma al balance (para no contar doble con el cheque real).

---

## Archivos del repo

```
index.html            ← la app completa
manifest.webmanifest  ← datos de la PWA (nombre, íconos, colores)
sw.js                 ← service worker (cache + offline)
icon.svg              ← ícono vectorial
apple-touch-icon.png  ← ícono para "Agregar a inicio" en iPhone (180px)
icon-192.png          ← ícono PWA 192px
icon-512.png          ← ícono PWA 512px
package.json          ← metadata (opcional)
README.md             ← este archivo
```

> ⚠️ Subí **todos** estos archivos al repo. Si falta `sw.js` o los íconos, la app igual funciona pero no se podrá instalar bien en el teléfono.

---

## Subirlo a GitHub y desplegar en Vercel

### 1. Crear el repo en GitHub
1. Andá a https://github.com/new
2. Nombre: `mi-dinero` (o el que quieras) → **Create repository**

### 2. Subir los archivos
1. En el repo nuevo, clic en **"uploading an existing file"**
2. Arrastrá **todos** los archivos de esta carpeta
3. **Commit changes**

### 3. Conectar a Vercel
1. https://vercel.com → entrá con GitHub
2. **Add New → Project** → importá el repo
3. Framework Preset: **Other** → **Deploy**
4. En ~30s tenés tu URL: `https://mi-dinero-xxx.vercel.app`

Cada cambio en GitHub redeploya solo.

> Si actualizás la app, subí los archivos nuevos a GitHub. En el teléfono, cerrá y reabrí la app (o recargá en Safari) para que el service worker tome la versión nueva.

---

## Usarla en el iPhone (sin App Store)

1. Abrí tu URL de Vercel en **Safari**.
2. Tocá el botón **Compartir** (el cuadrito con la flecha hacia arriba).
3. Elegí **"Agregar a inicio"** / "Add to Home Screen".
4. Listo: queda un ícono en tu pantalla, abre a pantalla completa y funciona sin internet.

---

## ⚠️ Sobre tus datos (importante)

- Los datos se guardan en **este dispositivo/navegador** (localStorage).
- En iPhone, Safari **puede borrar** esos datos si no abrís la app por ~7 días o si limpiás el historial/datos del sitio.
- Por eso: usá **Resumen → Exportar respaldo** cada tanto. Guardá ese archivo `.json` en Notas, Mail o Drive.
- Si se borra algo o cambiás de teléfono: **Importar respaldo** y vuelve todo.
- Para una solución a prueba de balas (datos en la nube con login, sincronizados entre dispositivos) hace falta un paso extra de backend — se puede agregar después.

---

## Claves de almacenamiento

| Clave | Contenido |
|-------|-----------|
| `tip-savings-data` | Registro diario `{ "YYYY-MM-DD": {com,hrs,tip} }` |
| `tip-cheques-data` | Cheques cobrados `[{id,date,label,amount}]` |
| `tip-expenses` | Gastos `[{id,date,amount,cat,wallet}]` |
| `tip-wallet-adjust` | Ajustes manuales de billeteras `{banco,efectivo}` |
| `tip-tarifa` | Tarifa por hora (para estimar cheque) |
| `tip-goals` | Metas `[{id,name,target,date,created}]` |

## Parámetros del modelo de impuestos (Florida)

Social Security 6.2% + Medicare 1.45% + Federal fijo $4.57. Editable en el código (`SS_RATE`, `MED_RATE`, `FED_FIXED`). Tarifa por hora editable desde la interfaz.
