# AGENTS.md · assetto-corsa-nos-purge

Guía para agentes de código. Qué es, cómo se instala y cómo se configura está en `README.md` (y `README.es.md`).

## Qué es

Un único script Lua (`nos_purge.lua`, 23 líneas) que añade humo tipo NOS Purge a un coche de Assetto Corsa. Usa el sistema de partículas de CSP (Custom Shaders Patch) y se activa con el botón `ExtraE` de Content Manager. No hay build, dependencias ni tests.

## Estructura

- `nos_purge.lua`: el script. Define las propiedades de `ac.Particles.Smoke` y, en `script.update`, emite dos columnas de humo cuando `car.extraE` es `true`.
- `README.md` / `README.es.md`: instalación, configuración de `ext_config.ini`, tabla de propiedades y el script copiado.
- `assets/`: imagen de la cabecera del README.
- `LICENSE` (MIT).

## Comandos

No hay scripts de build, test ni lint. Las únicas comprobaciones posibles son:

- Revisar la sintaxis Lua a mano o con un linter de Lua, si el entorno lo tiene. No se ha configurado ninguno en el repo.
- Probarlo en el juego: copiar `nos_purge.lua` a la carpeta `extension` de un coche, añadir la entrada `[SCRIPT_0]` a su `ext_config.ini`, activar `ExtraE` en Content Manager y conducir. Esta es la única verificación real.

## Cómo probar un cambio

- No hay tests automáticos. Documenta en el PR o commit qué se probó en el juego: coche, versión de CSP y si se vio el humo.
- Un cambio que solo toca valores (`color`, `thickness`, `life`, `size`, `spreadK`, `growK`, `targetYVelocity`) debe comprobarse visualmente; no se puede verificar desde el repo.

## Convenciones

- Lua con el API de CSP (`ac.Particles.Smoke`, `script.update`, `car.extraE`, `rgbm`, `vec3`). Ese API depende de la versión de CSP; no lo asumas sin consultar su documentación.
- Comentarios y nombres de propiedades, en inglés, como están en el script. La documentación del README va en inglés y español (`README.md` y `README.es.md`); cambia los dos a la vez.
- Sin versión ni CHANGELOG en el repo.

## Decisiones y trampas

- **Coordenadas fijas.** Las dos columnas de humo se emiten desde `vec3(-0.68, 0.95, 0.7)` y `vec3(0.68, 0.95, 0.7)`, con velocidades fijas. Están codificadas para la carrocería de un modelo concreto [inferencia: ajustadas a un coche de ejemplo]. Cambiar de coche exige recalcularlas.
- **Valores del README sincronizados con el script.** La tabla de propiedades del README refleja los valores de `nos_purge.lua`. Si cambias uno, cambia la tabla y el bloque de código de los dos README.
- **Activación.** El script solo emite si `car.extraE` es `true`. Sin la tecla `ExtraE` activa en Content Manager no aparece nada, aunque el script esté bien.
- **`ext_config.ini`.** La entrada `[SCRIPT_0]` tiene `ACTIVE_FOR_NEAREST=4` y `ACTIVE_FOR_UNFOCUSED=1` según el README; cambiarlos afecta a qué coches ejecutan el script.
- **Rendimiento.** El README lo describe como ligero. No hay medida en el repo: no lo afirmes sin probarlo.

## Mantenimiento

- Este fichero y el `README.md` se actualizan en el mismo cambio que los deja obsoletos.
- Aquí solo va lo que puede ser público: nada de rutas locales, puertos de la máquina, IPs, perfiles de secretos ni datos personales.
