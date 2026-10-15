# flowguard (repositorio combinado)

Este repositorio junta los seis repos independientes de FlowGuard en uno solo, cada uno como una carpeta normal, usando `git subtree` — conservando el historial de cada uno y sin que un repo "truene" al otro.

Mientras el equipo trabaja, cada quien sigue empujando a su propio repo (`flowguard-docs`, `flowguard-supabase`, `flowguard-src`, `flowguard-app`, `flowguard-mocks`, `flowguard-tests`). Este repositorio se actualiza cuando se decida congelar una integración (por ejemplo, para la demo final).

## Por qué `git subtree` y no `git submodule`

- Un **submódulo** solo guarda un puntero a un commit de cada repo; clonar el repo combinado requiere un paso extra (`git submodule update --init`) y el código de los submódulos no aparece realmente dentro del repo combinado.
- Un **subtree** copia el contenido y el historial de cada repo directamente dentro de una carpeta de este repositorio. El resultado se ve y se usa como un monorepo normal (`npm install` en la raíz ve todo), sin pasos extra para quien lo clona, y sigue siendo posible actualizar cada carpeta por separado desde su repo original.

## Primera integración (cuando el equipo esté listo)

Ejecutar una vez, en este repositorio ya clonado:

```bash
git remote add docs-origin      https://github.com/FlowGuard-Hackathon/flowguard-docs.git
git remote add supabase-origin  https://github.com/FlowGuard-Hackathon/flowguard-supabase.git
git remote add src-origin       https://github.com/FlowGuard-Hackathon/flowguard-src.git
git remote add app-origin       https://github.com/FlowGuard-Hackathon/flowguard-app.git
git remote add mocks-origin     https://github.com/FlowGuard-Hackathon/flowguard-mocks.git
git remote add tests-origin     https://github.com/FlowGuard-Hackathon/flowguard-tests.git

git subtree add --prefix=docs     docs-origin     main --squash
git subtree add --prefix=supabase supabase-origin main --squash
git subtree add --prefix=src      src-origin      main --squash
git subtree add --prefix=app      app-origin      main --squash
git subtree add --prefix=mocks    mocks-origin    main --squash
git subtree add --prefix=tests    tests-origin    main --squash

git push origin main
```

`--squash` trae el contenido de cada repo como un solo commit aquí (en vez de todo su historial completo), para mantener el log de este repositorio legible — cada repo individual conserva su propio historial completo en su propia URL, nada se pierde.

## Actualizaciones posteriores

Para traer cambios nuevos de un repo individual hacia este combinado:

```bash
git subtree pull --prefix=docs docs-origin main --squash
```

Para empujar un cambio hecho *dentro* de este repo combinado de vuelta a un repo individual (menos frecuente, pero posible):

```bash
git subtree push --prefix=docs docs-origin main
```

## Estado actual

Este repositorio todavía no tiene ninguna carpeta integrada — está preparado para la primera integración descrita arriba, cuando el equipo decida hacerla (recomendado: no antes del hito del día 6 de [flowguard-docs/implementation-checklist.md](https://github.com/FlowGuard-Hackathon/flowguard-docs/blob/main/implementation-checklist.md), para no congelar algo que todavía está cambiando mucho).
