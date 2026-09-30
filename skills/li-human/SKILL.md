---
name: li-human
description: >-
  Limpia las huellas de máquina de un borrador (caracteres invisibles, comillas
  curvas, léxico de relleno, marcadores de plantilla sin llenar) y reporta las
  estructuras que conviene reescribir antes de publicar. Funciona en español de
  México (slop-es.json) y en inglés (slop.json). Úsala siempre antes de mostrar
  un post, comentario, respuesta o DM de LinkedIn, y cuando el usuario diga
  "humaniza esto", "¿suena a IA?", "límpialo", "revisa las huellas", humanize,
  "does this sound like AI" o "de-slop this".
---

# li-human (fork en español)

En esta carpeta hay dos herramientas que se ejecutan de verdad. Úsalas; no
revises a ojo.

## Elige el léxico por idioma

- Texto en español: `--lexicon slop-es.json`
- Texto en inglés: sin `--lexicon` (usa `slop.json`, el original del repo)

```bash
python3 humanize.py borrador.txt -o limpio.txt --report --lexicon slop-es.json   # limpia y reporta
python3 detect.py borrador.txt limpio.txt --lexicon slop-es.json                  # antes y después, como referencia
```

## Qué se corrige solo

1. **Caracteres invisibles.** Espacios de ancho cero, marcas de orden de bytes,
   caracteres de etiqueta Unicode y espacios duros. `humanize.py` los borra todos.
2. **Tipografía.** Comillas curvas a rectas, puntos suspensivos a tres puntos,
   raya corta a guion. **La raya (—) no se toca en español**: el inciso cerrado
   ( —así— ) y la raya de viñeta son ortografía correcta.
3. **Léxico.** Solo reemplazos sin riesgo de concordancia ni de sentido:
   "a efecto de" → "para", "en virtud de que" → "porque", "hace sentido" →
   "tiene sentido", "en base a" → "con base en", robusto/a/os/as →
   sólido/a/os/as, entre otros.

## Qué NO se corrige solo

El reporte trae dos tipos de marca. Nunca reescribas en automático ninguna de
las dos: cambiar la forma de una oración requiere criterio.

**Huellas** (restan en el puntaje). Propón una reescritura concreta para cada
una y espera la aprobación del usuario:
- Marcadores de plantilla sin llenar: `[INSERTAR…]`, `{{…}}`
- Flecha →, raya pegada sin espacios (convención del inglés)
- "La pregunta (no) es…", anuncios como "vale la pena aclarar" o "conviene entender"
- Remates en fragmento ("Eso cambió.", "deja de serlo")
- Muletillas de apertura, aperturas y cierres de plantilla de LinkedIn,
  carnada de interacción, relleno de crónica de evento
- Verbos de consultor, sustantivos inflados, calcos del inglés
- Muros de hashtags (5 o más)

**Avisos** (no restan; están marcados con `[aviso]`). Son recursos que el autor
también usa en sus propios textos:
- "No es X. Es Y." y sus variantes, series de tres, anáforas, cadenas de fragmentos
- "Con el objetivo de…" (puede ser conector o sustantivo)
- "Fundamental" (puede ser técnico: derechos o principios fundamentales)

Con los avisos, **pregunta en vez de corregir**: "¿esto es intención tuya o
inercia del borrador?". Solo sugiere cambio si se repiten varias veces en el
mismo texto.

## Limitación conocida: negritas Unicode

Las "negritas" de LinkedIn (𝗮𝘀𝗶́) son otro alfabeto. `humanize.py` no ve el
léxico dentro de ellas. `detect.py` sí las normaliza para medir. Si el borrador
trae negritas, revisa esos pasajes a mano contra el reporte. Nunca quites las
negritas: son estilo del autor.

## El puntaje es referencia, no veredicto

Con el léxico en español, `detect.py` muestra **REFERENCIA** en lugar de
PASS / REVIEW / FLAGGED. En la prueba a ciegas, un texto de IA sacó 74 y dos
posts escritos por el autor quedaron abajo. Por eso:

- **Nunca** presentes el puntaje como prueba de que un texto "suena humano" o
  "no será detectado".
- Úsalo solo para comparar el antes y el después del mismo texto.
- La herramienta principal es el reporte de `humanize.py`.

Son heurísticas locales; nada se sube a ningún servicio. No son GPTZero,
Originality, Copyleaks, Winston ni Turnitin, y no prometen sus resultados.

## Orden de trabajo

1. `humanize.py borrador.txt -o limpio.txt --report --lexicon slop-es.json`
2. Muestra al usuario el texto limpio y el reporte: primero huellas, después avisos.
3. Para cada huella, propón una reescritura. Para cada aviso, pregunta.
4. Aplica solo lo que el usuario apruebe.
5. Opcional: `detect.py borrador.txt limpio.txt --lexicon slop-es.json` como
   referencia del cambio. Nunca muestres el puntaje sin el texto.
6. Si hay datos faltantes, déjalos como marcador visible; nunca inventes cifras,
   fechas ni nombres.
