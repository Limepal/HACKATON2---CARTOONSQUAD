# TropelCare Control Room


## Integrantes

| Nombre | Código |
|--------|--------|
| Burgos Ochoa Jeseph Imanol | 2020510395 |
| Morales Portella Santiago Cesar | 202510403 |
| Diaz Carrion Gabriel Marcelo | 202510404 |

## Instalación

```bash
npm install
```

## Variables de entorno

Crea un `.env` basado en `.env.example`:

```env
VITE_API_BASE_URL=https://<backend-url>/api/v1
VITE_TEAM_CODE=TEAM-087
VITE_EMAIL=operator@tuckersoft.com
Pizza-TEAM-087
```

## Comandos

```bash
npm run dev        # Dev server en http://localhost:5173
npm run build      # Build de producción
npm run typecheck  # Verificación de tipos sin emitir
npm run preview    # Preview del build
```


## Decisiones técnicas

### Anti-race condition en Tropeles
Se usa un contador de versión (`reqVersion`) por ref para descartar respuestas de requests obsoletas cuando el usuario cambia filtros rápidamente.

### Infinite scroll sin librerías
`IntersectionObserver` con un sentinel al final de la lista. Un ref booleano (`inFlightRef`) garantiza que sólo una request adicional esté en vuelo. En reseteos (cambio de filtros), se cancela la request anterior con `AbortController`.

### Deduplicación de señales
Se mantiene un `Set<string>` de IDs vistos. Cada página nueva filtra los IDs ya presentes antes de agregarlos al estado.

### Preservar posición en el feed
Al navegar a un detalle de señal, se guarda `window.scrollY` en el `state` de React Router. Al volver, se restaura con `window.scrollTo`.

### Scrollytelling (Checkpoint 5)
- `IntersectionObserver` con `rootMargin: '-30% 0px -40% 0px'` activa la etapa visible.
- `CSS Scroll-driven Animations` via `animation-timeline: view()` si el navegador lo soporta (progresive enhancement).
- `View Transition API` declarada en CSS global como progressive enhancement.
- `prefers-reduced-motion`: deshabilita todas las animaciones y transiciones.
- Navegación por teclado: ↑↓ ←→ hacen scroll a la etapa anterior/siguiente.
- El panel visual sticky (desktop) y compacto (mobile) muestran la etapa activa en todo momento.

### URL state
`useUrlState` sincroniza filtros y paginación con `useSearchParams` de React Router. Compartir o recargar la URL restaura el mismo estado exacto.

## Link del deploy

_Pendiente de completar_
