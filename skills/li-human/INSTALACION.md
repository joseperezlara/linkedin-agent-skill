# li-human en español — instalación

## Qué trae esta carpeta

| Archivo | Estado |
|---|---|
| `SKILL.md` | Reescrito: léxico por idioma, huellas vs. avisos, puntaje como referencia |
| `detect.py` | Adaptado: lee la sección `detector` del léxico. Sin ella, funciona igual que el original |
| `slop-es.json` | Nuevo: léxico en español de México (v1.3) |
| `humanize.py` | Original, sin cambios |
| `slop.json` | Original, sin cambios (inglés) |

## Pasos

1. Haz un fork de `Jakeschincariol/linkedin-agent-skill` en tu GitHub.
2. Reemplaza la carpeta `skills/li-human/` de tu fork por esta carpeta `li-human/`.
3. Haz commit y push.
4. Instala desde tu fork, no desde el original. Si actualizas desde el
   original, te sobrescribe `detect.py` y `SKILL.md`.

## Prueba rápida

```bash
cd skills/li-human
printf 'Cabe destacar que en virtud de que la regla cambió, hace sentido revisar.\n' > prueba.txt
python3 humanize.py prueba.txt --report --lexicon slop-es.json
python3 detect.py prueba.txt --lexicon slop-es.json
```

Debe cambiar "en virtud de que" a "porque" y "hace sentido" a "tiene sentido",
marcar "cabe destacar que" como huella y mostrar el puntaje como REFERENCIA.

## Calibración (septiembre 2026)

- 4 posts propios, 4 mixtos (IA editada) y 4 de IA pura.
- El panel original del repo no distinguía ningún grupo (promedios de 45 a 53).
- El adaptado separaba los grupos, pero falló en la prueba a ciegas: por eso el
  puntaje quedó como referencia y el reporte de huellas como herramienta principal.
- Única señal consistente en los 12 textos: comillas curvas (0 en posts propios,
  3 de 4 textos de IA).
