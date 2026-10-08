# Contexto del proyecto (para Claude Code)

PWA para iPad con los vídeos esquemáticos de las jugadas de ataque del entrenador (balonmano, Juvenil de Fundación Agustinos Alicante). Se usa en los **tiempos muertos (<1 min)**: encontrar la jugada en 2 toques, reproducirla y dibujar encima con el Apple Pencil.

## Decisiones tomadas
- **PWA y no app nativa:** sin Xcode ni cuenta de desarrollador de Apple. Se publica en **GitHub Pages**.
- **Los vídeos nunca se suben al repo.** Se importan en el iPad desde Archivos/Google Drive y se guardan en **IndexedDB** (`jugadas-db`, stores `videos` y `kv`). La app funciona 100 % offline.
- **El catálogo (`datos/catalogo_jugadas.json`) NO va al repo público:** contiene el playbook del entrenador, sacado de su PDF "Procedimientos ofensivos Juvenil". Se importa en la app junto a los vídeos.
- **Clasificación automática por nombre de fichero:** `norm()` pasa a minúsculas, quita tildes, convierte `+` en " mas " y elimina `_`/`-`. Luego se compara con el id, el nombre y los alias del catálogo. Si no hay coincidencia, el vídeo va a "Otras", y se puede reasignar a mano en ⚙︎.
- **Pencil dibuja, el dedo pausa o reanuda** (`pointerType`). Apoyar el lápiz pausa el vídeo. Hay un modo "dibujar con el dedo" opcional.
- **Herramientas de dibujo:** trazo libre, desplazamiento (continua + flecha), pase (discontinua + flecha), goma, deshacer y borrar. Los trazos se guardan en coordenadas normalizadas 0–1.
- **"Pista vacía":** media pista dibujada en canvas (`drawCourt`) con el mismo estilo que los vídeos.
- **Formato de los vídeos:** H.264, 1620×2160 (3:4 vertical), 10–30 s, generados con una app de pizarra táctica.

## Estructura (GitHub Pages publica la raíz del repo)
- `index.html` (en la raíz del repo): toda la app (HTML + CSS + JS sin dependencias ni build).
- `sw.js`: cachea el app shell. **Sube `CACHE` en cada versión.** La navegación va por red primero y, si no hay red, usa la caché.
- `manifest.webmanifest` e iconos.
- `datos/catalogo_jugadas.json`: familias, jugadas, alias, etiquetas (p. ej. `oleadas`) y descripción.

## Flujo de trabajo
- Prueba local en el navegador: `npx serve .` o `python -m http.server 8080`.
- Prueba real en el iPad: push a GitHub, esperar ~1 min a Pages y abrir la app instalada con conexión para que se actualice.
- Mantener `APP_VERSION` (index.html) y `CACHE` (sw.js) sincronizados.
- El usuario se comunica en español; la UI está en español.

## Pendiente
- Ajustes tras la primera prueba en el iPad (el usuario los irá indicando).
