# Control de muestras multiresiduo – Cerezas T2026-27 (Frutea)

App web (PWA) para llevar el estado de cada muestra de análisis multiresiduo a partir de la hoja **FLUJO DIARIO** de la estimación de cerezas. Los datos se comparten entre todo el equipo mediante Firestore.

## Estados
No tomada → Tomada → Enviada al laboratorio → Resultado entregado

## Base de datos (Firestore, mismo proyecto del Cuaderno de Dardos)
| Colección | Documento | Contenido |
|---|---|---|
| `multiresiduo_flujo` | `T2026-27` | Flujo diario importado, quién y cuándo lo cargó |
| `multiresiduo_config` | `T2026-27` | Días entre variedades, vigencia, sábado del laboratorio |
| `multiresiduo_muestras` | `productor\|predio\|CSG\|variedad` | Estado actual de cada muestra (toma, envío, resultado) |
| `multiresiduo_muestras/{id}/historial` | automático | Cada cambio: qué se hizo, quién y cuándo (no se puede borrar) |

### Configuración (una sola vez)
1. En `index.html`, busca `const firebaseConfig` y pega la configuración del proyecto del Cuaderno de Dardos (Consola Firebase → ⚙ Configuración del proyecto → Tus apps → App web).
2. En Firestore → Reglas, **agrega** los bloques de `firestore.rules.multiresiduo` dentro de tus reglas actuales (no las reemplaces) y publica.
3. La primera vez que alguien abra la app conectada, se sube el flujo inicial y los estados que esa persona tenga guardados en su equipo.

Cada usuario indica su nombre al entrar (se puede cambiar tocando el indicador de conexión). Funciona sin señal: los cambios se guardan en el teléfono y se sincronizan al volver la conexión.

## Cómo arma el plan de muestreo
- **Campo** = productor + predio + CSG. Se muestrea la variedad que parte primero según el flujo.
- Si otra variedad del mismo campo parte **más de 15 días** después, lleva su propia muestra (configurable).
- **Tomar a más tardar**: el laboratorio retira en la mañana y entrega al día hábil siguiente en la tarde; la muestra debe salir el último día hábil cuyo resultado llega antes del inicio de cosecha.
- **Vigencia**: 15 días desde la toma (configurable); se avisa si una variedad cubierta queda fuera.

## Publicar en GitHub Pages
Subir `index.html`, `manifest.json`, `sw.js`, `icon.svg`, `README.md` y `firestore.rules.multiresiduo` al repo → Settings → Pages → rama `main` / root.

## Pendiente
Inicio de sesión (mismo sistema de cuentas del Cuaderno de Dardos) y reglas que exijan usuario autorizado.
