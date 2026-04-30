---
{"dg-publish":true,"permalink":"/categories/paradigmas-de-la-programacion/","tags":["gardenEntry"],"dg-note-properties":{"categories":["[[Subjects]]"],"year":2026,"semestre":"1ro","status":["[[Active]]"],"type":["Hub"],"Profesores":["Javier Godoy"],"tags":null}}
---

# Magistrales
 
```base
filters:
  and:
    - subject == ["[[Paradigmas de la Programación]]"]
    - type != ["Concept"]
properties:
  file.name:
    displayName: Clase
  note.date:
    displayName: Dia
  note.topics:
    displayName: Temas
  note.clase:
    displayName: Tipo de Clase
  note.type:
    displayName: Tipo
  file.tags:
    displayName: Status
views:
  - type: table
    name: Table
    order:
      - date
      - file.name
      - file.tags
      - type
      - topics
    sort:
      - property: date
        direction: DESC
      - property: file.name
        direction: DESC
      - property: tags
        direction: ASC
      - property: type
        direction: ASC
    columnSize:
      note.date: 107
      file.name: 246
      file.tags: 135
      note.type: 111
      note.topics: 80
  - type: table
    name: Magistrales
    filters:
      and:
        - type == ["Magistral"]
    order:
      - date
      - file.name
      - type
      - topics
    sort:
      - property: date
        direction: DESC

```


# TPs
[[Notes/TP 1 - Paradigmas\|TP 1 - Paradigmas]]