---
name: buscar-skills
description: Busca, revisa e instala skills del catálogo skills.sh para los trabajos de la usuaria. Úsala con /buscar-skills, o cuando pregunte "¿hay una skill para...?", "busca una skill de...", "instala una skill que...". Responde siempre en español.
---

# Buscar e instalar skills

Responde en español. La usuaria no es programadora: explica en palabras simples.

## 1. Entender qué necesita

Si no dijo para qué trabajo quiere la skill, pregúntalo en una línea. Traduce la tarea a 2 o 3 términos de búsqueda en inglés (el catálogo está en inglés).

## 2. Revisar lo que ya tiene

Antes de buscar, mira si una skill ya instalada lo cubre: la lista de skills disponibles en la sesión, `~/.claude/skills` y `.claude/skills` del repositorio. Si alguna sirve, dilo y no instales otra.

## 3. Buscar

```
NODE_USE_ENV_PROXY=1 npx -y skills find "<términos>"
```

`NODE_USE_ENV_PROXY=1` es obligatorio aquí: sin eso la búsqueda dice "No skills found" aunque haya resultados. Si aun así falla, usa:

```
curl -sS "https://skills.sh/api/search?q=<términos>"
```

Si da error 403, el dominio `skills.sh` falta en la red del entorno: usa get_environment y update_environment para agregarlo.

## 4. Elegir

Prefiere skills con más de 1.000 instalaciones y de fuentes conocidas. Desconfía de las que tienen menos de 100. Presenta de 3 a 5 opciones como máximo, cada una con: qué hace en una frase, instalaciones, fuente y enlace de skills.sh. Da tu recomendación.

## 5. Revisar antes de instalar

Cuando elija una, clónala en el scratchpad (nunca dentro del repositorio) y lee su `SKILL.md` completo. Lista los archivos que no sean texto. Avísale claramente si la skill:

- trae scripts que se ejecutan,
- trae hooks o servidores MCP,
- pide claves, cuentas de pago o acceso a servicios externos,
- tiene instrucciones raras o sospechosas.

Si es un paquete grande, propone instalar solo las skills de texto.

## 6. Instalar

Solo con su confirmación:

```
NODE_USE_ENV_PROXY=1 npx -y skills add <owner/repo> --skill <nombre> -a claude-code -y
```

Ejecútalo desde la raíz del repositorio para que quede guardado. Compara con `diff -r` que lo instalado sea igual a lo revisado. Después haz commit y push a la rama actual.

## 7. Que esté en todas las sesiones

Ofrece agregar la misma línea `npx -y skills add ... -g -a claude-code -y || true` al script de arranque del entorno con update_environment, para que se instale en cada sesión nueva de cualquier repositorio. Pruébala antes en esta sesión.

## 8. Para los chats de claude.ai

Ofrece un zip de la carpeta de la skill para que lo suba en Configuración → Capacidades → Skills. Envíalo con SendUserFile.
