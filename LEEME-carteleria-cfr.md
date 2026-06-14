# Cartelería dixital CFR de Vigo — Guía de uso

Sistema de cartelería para a Smart TV do centro. Funciona como unha páxina web (`index.html`) publicada en GitHub Pages, que se conecta automaticamente a varias follas de Google Sheets para mostrar a información actualizada cada día.

URL en produción: `https://caixadocorreo.github.io/carteleria-cfr/`

---

## 1. Que mostra e cando

A pantalla cambia automaticamente segundo o día e a hora, sen intervención manual:

| Momento | Que se mostra |
|---|---|
| **Domingo** | "O centro está pechado hoxe" |
| **Sábado sen sesións programadas** | "O centro está pechado hoxe" |
| **Sábado con sesións programadas** | Vista de sesións (igual que un día de tarde) |
| **Luns a venres, 08:00–13:45** | Vista de **Directorio** (persoal e aulas de mañá) |
| **Luns a venres, a partir das 13:45** | Vista de **Sesións de hoxe** (actividades de tarde) |

A pantalla recárgase automaticamente cada noite á 00:05 para actualizar a data e os datos.

O horario oficial do centro é de 09:00 a 14:00. O cambio á vista de tarde adiántase a **13:45** para que o persoal poida comprobar que a información está correctamente configurada antes de marchar.

---

## 2. Vista "Sesións de hoxe"

Mostra as actividades formativas que teñen sesión **esa tarde** (hora de inicio ≥ 14:00), en grupos de varias por pantalla con avance automático cada 15 segundos.

Cada actividade amosa:
- Código (en negrita)
- Título
- Relator/a(s)
- Horario (hora inicio – hora fin)
- Icona e nome da aula/espazo

### Fonte de datos
Folla de Google Sheets "Sesións", publicada como CSV. Columnas (por orde):

| Columna | Contido |
|---|---|
| A | Data (formato DD/MM/AAAA) |
| B | Aula/espazo (debe coincidir cos nomes de `DIRECTORIO_SLIDE2` no código) |
| C | Código da actividade |
| D | Título |
| E | Relator/es, separados por `;` |
| F | Hora de inicio (HH:MM) |
| G | Hora de fin (HH:MM) |

**Importante**: só se mostran na vista de sesións as filas con `data` = hoxe e `horaInicio` ≥ 14:00. As de mañá (antes das 14:00) aparecen no Directorio.

---

## 3. Vista "Directorio" (luns–venres, 08:00–14:00)

Ten dous "slides" que rotan:

### Slide 1 — Persoal
Lista fixa de espazos administrativos (Administración/Préstamos, Dirección/Secretaría, Despachos 1-4), cos seus responsables.

### Slide 2 — Aulas con actividade hoxe
Só se mostran as aulas que **teñen algunha sesión programada para hoxe** (mañá ou tarde). Se unha aula ten sesión de mañá e de tarde, móstranse as dúas (título + horario de cada unha). Se non hai ningunha aula con actividade, este slide non aparece.

### Fonte de datos
Folla de Google Sheets "Directorio", publicada como CSV. Columnas:

| Columna | Contido |
|---|---|
| A | Espazo (debe coincidir exactamente co nome usado en `DIRECTORIO_SLIDE1`) |
| B | Responsables, separados por `;` |

Os responsables das aulas (slide 2) **non** veñen desta folla — só se usa para o slide 1 (persoal). Os datos das aulas no slide 2 veñen da folla de Sesións.

---

## 4. Pósteres e promocións

Sistema para inserir slides especiais de imaxe a pantalla completa, intercalados nos ciclos normais (tanto en Sesións como en Directorio).

### Tipos
| Tipo | Comportamento | Cabeceira que se mostra |
|---|---|---|
| `poster` | Imaxe a pantalla completa, intercalada **ao final** do ciclo | "Sesións de hoxe" (sempre, mesmo se está no ciclo do Directorio) |
| `promo` | Imaxe a pantalla completa, intercalada en **posición aleatoria** (recalculada cada vez que se carga a páxina) | "+info" |

### Fonte de datos
Folla de Google Sheets "Promos", publicada como CSV. Columnas:

| Columna | Contido |
|---|---|
| A | Tipo (`poster` ou `promo`) |
| B | URL da imaxe |
| C | Data de inicio de visualización (DD/MM/AAAA) |
| D | Data de fin de visualización (DD/MM/AAAA) — inclusive, o póster/promo desaparece o día **seguinte** a esta data |

### Imaxes
- **Proporción recomendada**: 1080 × 1746 px (ou proporcional) para que ocupe toda a pantalla sen marxes brancos, xa que o espazo dispoñible é o que queda entre a cabeceira (130px) e o rodapé (44px) nunha TV vertical 1080×1920.
- **URLs de Google Drive**: pégase o enlace normal de "Compartir" (`https://drive.google.com/file/d/ID/view...`) e o sistema convérteo automaticamente ao formato `lh3.googleusercontent.com` que funciona sen problemas de CORS.

### Como gardar un póster/promo para reutilizar máis tarde
Non fai falta borrar a fila. Pon a `DataFin` en pasado (deixa de mostrarse) e, cando o queiras reactivar, actualiza `DataInicioVisual`/`DataFin` ás novas datas. Podes ter varias filas de tipo `portada`/`poster`/`promo` con datas correlativas para ir rotando contidos ao longo do curso.

---

## 5. Espazos e iconas configurados

Os nomes seguintes están escritos **exactamente** como aparecen no código (`DIRECTORIO_SLIDE1` e `DIRECTORIO_SLIDE2`). O nome do espazo nas follas de Sesións (columna B) e Directorio (columna A) ten que coincidir **letra a letra, incluíndo maiúsculas, minúsculas, acentos e espazos** co valor desta lista, ou o sistema non recoñecerá a aula/espazo e non amosará a icona correcta.

### Slide 1 do Directorio (persoal) — `DIRECTORIO_SLIDE1`
| Espazo (nome exacto) | Andar |
|---|---|
| `Administración / Préstamos` | Planta baixa |
| `Dirección / Secretaría` | 2º andar |
| `Despacho 1` | 2º andar |
| `Despacho 2` | 2º andar |
| `Despacho 3` | 2º andar |
| `Despacho 4` | 2º andar |

### Slide 2 do Directorio / Vista de Sesións (aulas) — `DIRECTORIO_SLIDE2`
| Espazo (nome exacto) | Andar |
|---|---|
| `Aula 0` | Planta baixa |
| `Salón de actos` | Planta baixa |
| `Polo A` | Planta baixa |
| `Polo B` | Planta baixa |
| `Aula 1` | 1º andar |
| `Aula 2` | 1º andar |
| `Aula 3` | 1º andar |
| `Aula 4` | 1º andar |
| `Aula 5` | 1º andar |
| `Aula 6` | 1º andar |
| `Aula 7` | 2º andar |
| `Aula 8` | 2º andar |

---

## 6. Resumo de ligazóns CSV configuradas

No código (`index.html`), preto do inicio, atópanse tres constantes coas URLs dos CSV publicados:

- `CSV_URL` → folla de Sesións
- `DIRECTORIO_URL` → folla de Directorio (persoal)
- `CSV_PROMOS_URL` → folla de Pósteres/Promos

Se algunha folla de Sheets cambia de URL de publicación (por exemplo, ao crear unha folla nova desde cero), hai que actualizar a constante correspondente no código e volver subir o ficheiro a GitHub.

---

## 7. Actualización do sistema

Para cambios no deseño, lóxica ou estrutura (non só datos):

1. Editar o ficheiro `index.html`
2. Subir a GitHub no repositorio `caixadocorreo/carteleria-cfr`, substituíndo o ficheiro existente (Add file → Upload files)
3. A páxina publicada en GitHub Pages actualízase automaticamente uns minutos despois

Para cambios de **contido diario** (sesións, responsables, pósteres) **non hai que tocar o código** — só editar as follas de Google Sheets correspondentes.
