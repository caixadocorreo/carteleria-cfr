# Cartelería dixital CFR de Vigo — Guía de uso

Sistema de cartelería para a Smart TV do centro. Funciona como unha páxina web (`index.html`) publicada en GitHub Pages, que se conecta automaticamente a varias follas de Google Sheets para mostrar a información actualizada cada día.

URL en produción: `https://caixadocorreo.github.io/carteleria-cfr/`

**Modo probas** (para ver pola mañá o que se vai ver pola tarde): engadir `?modo=sesions` á URL —
`https://caixadocorreo.github.io/carteleria-cfr/?modo=sesions`. Ver detalles na §1.

---

## 1. Que mostra e cando

A pantalla cambia automaticamente segundo o día e a hora, sen intervención manual:

| Momento | Que se mostra |
|---|---|
| **Domingo** | "O centro está pechado hoxe" |
| **Sábado sen ningunha sesión programada** | "O centro está pechado hoxe" |
| **Sábado con sesións programadas** | Vista de **Sesións de hoxe** con **todas** as sesións do día (mañá e tarde) |
| **Luns a venres, antes das 08:00** | Vista de **Sesións de hoxe** (actividades de tarde) |
| **Luns a venres, 08:00–13:45** | Vista de **Directorio** (persoal e aulas con actividade) |
| **Luns a venres, a partir das 13:45** | Vista de **Sesións de hoxe** (actividades de tarde) |

**Cambio automático de vista:** a páxina comproba a hora cada 30 segundos e, cando o modo que corresponde cambia (por exemplo ás 08:00 ou ás 13:45), recárgase soa e amosa a vista nova. Non fai falta tocar nada.

Ademais, a pantalla recárgase automaticamente cada noite ás **00:00:05** para actualizar a data e os datos.

**Actualización dos datos:** cada **5 minutos** a páxina descarga de novo as catro follas (Sesións, Directorio, Promos e Lecturas) e compara o contido co que ten. Se algunha cambiou, recárgase soa e amosa os datos novos; se non hai cambios, non fai nada. Ao recargar, a rotación volve ao primeiro slide e as posicións aleatorias de promos e lecturas resortéanse. Ten en conta que Google Sheets tarda un tempo en actualizar o CSV publicado despois de editar a folla, así que o cambio pode tardar uns minutos máis en verse na TV. O intervalo axústase coa constante `REFRESCO_DATOS_MIN` do código.

O horario oficial do centro é de 09:00 a 14:00. O cambio á vista de tarde adiántase a **13:45** para que o persoal poida comprobar que a información está correctamente configurada antes de marchar.

En calquera vista, as **recomendacións de lectura** e os **pósteres/promos** intercálanse nos ciclos de slides, nas datas configuradas para cada un.

**Vista de Sesións e slides especiais:** para que o profesorado vexa axiña a súa aula, as pantallas de sesións **repítense despois de cada slide especial**. O ciclo é: sesións → especial → sesións → especial → … Así, cunha ou dúas pantallas de sesións, unha pantalla volve aparecer pouco despois de pasar (con 2 pantallas de sesións, ata uns 17 s; con 1, uns 7 s; cálculo aproximado a partir dos tempos da §8). A orde dos especiais é: promos e lecturas en orde aleatoria e, ao final, os pósteres. O Directorio non se modificou: alí seguen intercalados como se describe nas §4 e §5.

**Modo probas:** engadindo `?modo=sesions` ao final da URL fórzase a vista de Sesións de hoxe en calquera momento do día (por exemplo pola mañá, para comprobar como se verá pola tarde), cos datos reais de hoxe. Mentres está activo:
- Aparece unha etiqueta vermella "MODO PROBAS" na esquina superior esquerda, para non confundilo coa vista real.
- Desactívase o vixilante de cambio de hora (08:00/13:45), para que a proba non se interrompa só.
- O refresco de datos cada 5 minutos segue activo, así que a proba mostra os datos actuais.

Para saír do modo probas, basta quitar `?modo=sesions` da URL e recargar (ou abrir `https://caixadocorreo.github.io/carteleria-cfr/` sen parámetros).

---

## 2. Vista "Sesións de hoxe"

Mostra as actividades formativas do día que comezan **a partir das 13:45** (luns a venres) ou **todas as do día** (sábados), en grupos de **catro** por pantalla, con avance automático cada **10 segundos**.

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

**Importante**: de luns a venres só se mostran na vista de sesións as filas con `data` = hoxe e `horaInicio` ≥ 13:45. As que comezan antes aparecen só no Directorio (mentres dura a vista de mañá). Os sábados móstranse todas as filas de hoxe.

O sistema tolera datas sen cero inicial (`1/9/2026` interprétase como `01/09/2026`), pero recoméndase manter o formato `DD/MM/AAAA`.

Se non hai ningunha sesión que mostrar, a pantalla amosa "Non hai sesións programadas para hoxe" (agás que haxa pósteres, promos ou lecturas activos: nese caso móstranse só eses slides).

---

## 3. Vista "Directorio" (luns–venres, 08:00–13:45)

Ten dous "slides" que rotan:

### Slide 1 — Persoal
Lista fixa de espazos administrativos (Administración/Préstamos, Dirección/Secretaría, Despachos 1-4), cos seus responsables.

### Slide 2 — Aulas con actividade hoxe
Só se mostran as aulas que **teñen algunha sesión programada para hoxe** (mañá ou tarde). Se unha aula ten sesión de mañá e de tarde, móstranse as dúas (título + horario de cada unha). Se non hai ningunha aula con actividade, este slide non aparece. Todas as aulas con actividade móstranse nunha **única pantalla** (ata 12 filas), sen paxinar.

O Directorio rota cada **10 segundos**. Nesta vista non se mostran iconas, só nome, andar e responsables ou sesións.

### Fonte de datos
Folla de Google Sheets "Directorio", publicada como CSV. Columnas:

| Columna | Contido |
|---|---|
| A | Espazo (debe coincidir exactamente co nome usado en `DIRECTORIO_SLIDE1`) |
| B | Responsables, separados por `;` |

Os responsables das aulas (slide 2) **non** veñen desta folla — só se usa para o slide 1 (persoal). Os datos das aulas no slide 2 veñen da folla de Sesións.

---

## 4. Recomendacións de lectura

Slides especiais que mostran unha recomendación bibliográfica, intercalados en posición aleatoria no ciclo do Directorio; no de Sesións seguen a regra da §1 (as sesións repítense despois de cada un). A cabeceira mostra "+info" mentres dura o slide. Cada lectura dura **7 segundos**.

O slide mostra:
- Portada do libro (imaxe con proporción real, sen deformar)
- Título, autor/a e texto de recomendación

### Fonte de datos
Folla de Google Sheets "Lecturas", publicada como CSV (`CSV_LECTURAS_URL` no código). Columnas:

| Columna | Contido |
|---|---|
| A | Título do libro |
| B | Autor/a |
| C | Texto de recomendación |
| D | URL da imaxe da portada |
| E | Data de inicio de visualización (DD/MM/AAAA) |
| F | Data de fin de visualización (DD/MM/AAAA) — inclusive |

### Imaxes
- **URLs de Google Drive**: pégase o enlace normal de "Compartir" (`https://drive.google.com/file/d/ID/view...`) e o sistema convérteo automaticamente ao formato `lh3.googleusercontent.com`.
- Calquera proporción de portada funciona correctamente — a imaxe non se deforma.

### Xestión de datas
Se a `DataInicio` está baleira, a lectura non se mostra nunca. Se a `DataFin` está baleira, móstrase indefinidamente desde a data de inicio. Igual que as promos: pon a `DataFin` en pasado para desactivar sen borrar a fila, e actualiza as datas para reactivar. Podes ter varias recomendacións con datas correlativas para ir rotando ao longo do curso.

---

## 5. Pósteres e promocións

Sistema para inserir slides especiais de imaxe a pantalla completa, intercalados nos ciclos normais (tanto en Sesións como en Directorio).

### Tipos
Cada póster ou promo dura **7 segundos**. Se a `DataFin` está baleira, móstrase indefinidamente desde a data de inicio; se a `DataInicio` está baleira, non se mostra nunca.

| Tipo | Comportamento | Cabeceira que se mostra |
|---|---|---|
| `poster` | Imaxe a pantalla completa, intercalada **ao final** do ciclo (dos especiais) | "Sesións de hoxe" |
| `promo` | Imaxe a pantalla completa, en **posición aleatoria** entre os especiais | "+info" |

Na vista de Sesións, despois de cada póster ou promo repítense as pantallas de sesións (ver §1).

### Fonte de datos
Folla de Google Sheets "Promos", publicada como CSV. Columnas:

| Columna | Contido |
|---|---|
| A | Tipo (`poster` ou `promo`) |
| B | URL da imaxe |
| C | Data de inicio de visualización (DD/MM/AAAA) |
| D | Data de fin de visualización (DD/MM/AAAA) — inclusive, o póster/promo desaparece o día **seguinte** a esta data |

### Imaxes
- **Proporción recomendada**: 1080 × 1746 px (ou proporcional) para que ocupe toda a pantalla sen marxes brancos (a imaxe axústase con recorte, non se deforma, polo que se a proporción é distinta cortaranse os bordos), xa que o espazo dispoñible é o que queda entre a cabeceira (130px) e o rodapé (44px) nunha TV vertical 1080×1920.
- **URLs de Google Drive**: pégase o enlace normal de "Compartir" (`https://drive.google.com/file/d/ID/view...`) e o sistema convérteo automaticamente ao formato `lh3.googleusercontent.com` que funciona sen problemas de CORS.

### Como gardar un póster/promo para reutilizar máis tarde
Non fai falta borrar a fila. Pon a `DataFin` en pasado (deixa de mostrarse) e, cando o queiras reactivar, actualiza `DataInicioVisual`/`DataFin` ás novas datas. Podes ter varias filas con datas correlativas para ir rotando contidos ao longo do curso.

---

## 6. Espazos e iconas configurados

Os nomes seguintes están escritos **exactamente** como aparecen no código (`DIRECTORIO_SLIDE1` e `DIRECTORIO_SLIDE2`). O nome do espazo nas follas de Sesións (columna B) e Directorio (columna A) ten que coincidir **letra a letra, incluíndo maiúsculas, minúsculas, acentos e espazos** co valor desta lista, ou o sistema non recoñecerá a aula/espazo (na vista de Sesións non amosará a icona correcta e no Directorio a aula non aparecerá).

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

## 7. Resumo de ligazóns CSV configuradas

No código (`index.html`), no bloque `DATOS` do script (despois das iconas SVG), atópanse catro constantes coas URLs dos CSV publicados:

- `CSV_URL` → folla de Sesións
- `DIRECTORIO_URL` → folla de Directorio (persoal)
- `CSV_PROMOS_URL` → folla de Pósteres/Promos
- `CSV_LECTURAS_URL` → folla de Recomendacións de lectura

Se algunha folla de Sheets cambia de URL de publicación (por exemplo, ao crear unha folla nova desde cero), hai que actualizar a constante correspondente no código e volver subir o ficheiro a GitHub.

---

## 8. Tempos de rotación

Configurados na constante `INTERVALOS` do código (segundos por slide):

| Slide | Segundos |
|---|---|
| Sesións | 10 |
| Directorio | 10 |
| Recomendación de lectura | 7 |
| Promo | 7 |
| Póster | 7 |

Intervalo de comprobación de cambios nas follas: `REFRESCO_DATOS_MIN = 5` (minutos).

O número de actividades por pantalla na vista de Sesións é `POR_SLIDE = 4`.

---

## 9. Actualización do sistema

Para cambios no deseño, lóxica ou estrutura (non só datos):

1. Editar o ficheiro `index.html`
2. Subir a GitHub no repositorio `caixadocorreo/carteleria-cfr`, substituíndo o ficheiro existente (Add file → Upload files)
3. A páxina publicada en GitHub Pages actualízase automaticamente uns minutos despois

Para cambios de **contido diario** (sesións, responsables, pósteres, lecturas) **non hai que tocar o código** — só editar as follas de Google Sheets correspondentes. A pantalla detecta o cambio soa nuns minutos (ver §1).
