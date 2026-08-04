---
description: Crea un git worktree en .worktrees/<slug>
---

Vas a crear un git worktree en `.worktrees/<slug>` a partir del argumento del usuario: `$1`.

Sigue EXACTAMENTE estos pasos, sin omitir ninguno y en este orden:

1. Normaliza el argumento `$1` a un slug:
   - Convierte a minúsculas.
   - Quita acentos y símbolos (deja solo letras a-z y espacios).
   - Elimina stop-words en español: `del, de, la, el, los, las, en, y`.
   - Reemplaza espacios por guiones `-`.
   - Colapsa guiones múltiples en uno solo.

2. Si el slug resultante tiene 4 palabras o menos (contando por `-`), úsalo directamente.

3. Si el slug tiene MÁS de 4 palabras, el nombre es demasiado largo. Antes de ejecutar nada, propón al usuario 3 nombres cortos alternativos (derivados del argumento original, máx 4 palabras cada uno) y pregúntale cuál elegir usando la tool `question`. Usa el elegido como slug.

4. Verifica si ya existe un worktree con ese slug (revisa `git worktree list`). Si existe, díselo al usuario y termina sin crear nada.

5. Verifica que el directorio padre `.worktrees/` existe; si no, créalo.

6. Ejecuta el comando:
   git worktree add --detach ".worktrees/<slug-elegido>"

7. Reporta el resultado en una sola línea: la ruta del worktree creado.