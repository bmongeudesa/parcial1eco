---
{"dg-publish":true,"permalink":"/categories/paradigmas-de-la-programacion/","tags":["gardenEntry"],"dg-note-properties":{"categories":["[[Subjects]]"],"year":2026,"semestre":"1ro","status":["[[Active]]"],"type":["Hub"],"Profesores":["Javier Godoy"],"tags":null}}
---

## Magistrales 

```dataview
TABLE WITHOUT ID date AS "Dia", file.link AS "Title", status, type, topics
WHERE contains(subject, [[Categories/Paradigmas de la Programación\|Paradigmas de la Programación]])
WHERE !contains(type, "Concept")
SORT date, file.name ASC

