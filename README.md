# Mi Dinero — South Beach (v28)

Gestor de gastos e ingresos para trabajo con propinas. App estática de un solo archivo (`index.html`) + **PWA** (instalable en iPhone/Android y funciona sin internet). No necesita build ni servidor.

## Pestañas (4)
- **Resumen:** total disponible, billeteras (Banco / Efectivo / Ahorros), deuda y patrimonio neto, ingresos/gastos/ahorro del mes, y **métricas** (por semana, por mes, ahorro acumulado) + respaldo.
- **Gastos:** registro simple (monto · categoría · billetera) + gastos del mes por categoría + lista.
- **Ingresos:** registro diario (comisión/horas/tips), **próximo cheque esperado** (sobre la quincena anterior, con sueldo/hora y monto esperado editables), cheques cobrados, botón **➕ préstamo o ingreso extra**, y calendario.
- **Metas:** **ahorro automático** (aparta un % del sobrante diario a Ahorros) + metas con fecha (el progreso se mide con tus Ahorros).

## Cómo funciona el dinero
- 💵 **Tips cash** → Efectivo. 🏦 **Cheques cobrados** → Banco.
- 🧾 **Gastos** restan de la billetera elegida (5 categorías).
- 🤝 **Préstamos** entran a una billetera pero quedan como **deuda** (se aclara en Resumen y en Banco; botón "Pagado" para saldar).
- ✨ **Ingresos extra** (regalo, venta, trabajo aparte) entran a una billetera y son tuyos.
- 🐷 **Ahorros:** apartás dinero de Banco/Efectivo a Ahorros (manual o con la sugerencia de la regla %).
- ✏️ Banco/Efectivo/Ahorros se pueden **editar a mano** (ajustan una base sin perder los flujos).
- **Total disponible = Banco + Efectivo.** **Patrimonio neto = disponible + Ahorros − deuda.**
- La comisión/horas + estimación de impuestos es análisis/predicción del cheque, NO entra al balance.

## Archivos del repo
```
index.html  manifest.webmanifest  sw.js
icon.svg  apple-touch-icon.png  icon-192.png  icon-512.png
package.json  README.md
```
> Subí **todos** al repo de GitHub (incluí `sw.js`, `manifest.webmanifest` e íconos para que se pueda instalar en el teléfono).

## Desplegar (GitHub → Vercel)
1. GitHub → tu repo → **Add file → Upload files** → arrastrá todos los archivos → **Commit**.
2. Vercel redeploya solo en ~30s y te da el link.

## Usarla en iPhone (sin App Store)
Abrí el link en **Safari** → **Compartir** → **"Agregar a inicio"**. Queda como app a pantalla completa.

## ⚠️ Tus datos
Se guardan en este dispositivo (localStorage). En iPhone pueden borrarse a los ~7 días sin uso o al limpiar Safari. Usá **Resumen → Exportar respaldo** seguido (guardalo en Notas/Mail/Drive) y **Importar respaldo** para recuperarlo.

## Claves de almacenamiento
| Clave | Contenido |
|-------|-----------|
| `tip-savings-data` | Registro diario `{ "YYYY-MM-DD": {com,hrs,tip} }` |
| `tip-cheques-data` | Cheques cobrados |
| `tip-expenses` | Gastos `{id,date,amount,cat,wallet}` |
| `tip-loans` | Préstamos / deuda `{id,date,label,amount,wallet}` |
| `tip-extra` | Ingresos extra `{id,date,label,amount,wallet}` |
| `tip-savings-transfers` | Movimientos a Ahorros `{id,date,amount,from}` |
| `tip-wallet-adjust` | Ajustes manuales `{banco,efectivo,ahorros}` |
| `tip-save-rule` | % del sobrante diario a ahorrar (decimal) |
| `tip-cheque-overrides` | Montos de cheque esperado puestos a mano `{periodo: monto}` |
| `tip-tarifa` | Sueldo por hora (estimación del cheque) |
| `tip-goals` | Metas `{id,name,target,date}` |

Impuestos (Florida): Social Security 6.2% + Medicare 1.45% + federal fijo $4.57 (editables en el código).
