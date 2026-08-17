# leychile

La legislación chilena como repositorio git: **cada acto legislativo es un
commit**, con su fecha de vigencia real, la ley que lo produjo y quienes la
firmaron.

- `git log codigos/codigo-penal.md` es la historia legislativa del Código Penal.
- `git diff <commit>~1 <commit> -- codigos/codigo-penal.md` muestra exactamente
  qué palabras cambió esa reforma.
- La rama `main` contiene el texto vigente de cada norma.

El repositorio se organiza en tres carpetas:

| Carpeta | Qué contiene |
|---|---|
| `constitucion/` | La Constitución, con sus cuatro textos históricos encadenados. |
| `codigos/` | Los 15 códigos oficiales. |
| `normas/<tipo>/` | Las 714 normas que los modificaron, en subcarpetas por tipo: ley, decreto ley, auto acordado, sentencia, etc. |

Cada commit contiene **la causa y el efecto juntos**, y cubre el acto
legislativo completo: si una ley reformó ocho códigos el mismo día, eso es un
commit que toca los ocho archivos, no ocho commits repitiendo el mismo título.
Así se ve el alcance real de una reforma de un vistazo —el commit
`[LEY-20720] 2014-10-10: SUSTITUYE EL RÉGIMEN CONCURSAL VIGENTE...`:

    codigos/codigo-civil.md                       +25     -24
    codigos/codigo-de-comercio.md                 +358    -800
    codigos/codigo-de-mineria.md                  +4      -4
    codigos/codigo-de-procedimiento-civil.md      +10     -7
    codigos/codigo-del-trabajo.md                 +25     -7
    codigos/codigo-organico-de-tribunales.md      +7      -7
    codigos/codigo-penal.md                       +39     -11
    codigos/codigo-tributario.md                  +5      -5
    normas/ley/LEY-20720.md                       +2      -2

El golpe fuerte fue al Código de Comercio —800 líneas eliminadas, el Libro IV de
Quiebras que la ley derogó—, mientras que a Minería apenas le tocó cuatro
líneas. El cuerpo del commit cita, para cada documento y para la ley, tanto el
archivo local como la URL de BCN.

Un commit agrupa varios documentos sólo cuando **las mismas normas** los
modificaron **el mismo día**: cada norma citada en un commit modificó cada uno
de sus documentos. Cuando una ley entra a regir sobre distintos códigos en
fechas distintas, cada fecha es su propio commit, en su lugar de la cronología.

Los datos provienen íntegramente de [LeyChile](https://www.leychile.cl), el
servicio público de la Biblioteca del Congreso Nacional (BCN). No hay texto
escrito a mano ni generado por IA: todo se descarga de los servicios de BCN y se
convierte a Markdown de forma automática.

## Qué contiene

Los 16 documentos fundamentales del ordenamiento jurídico chileno:

| Documento | Desde |
|---|---|
| Constitución Política de la República | 1833 |
| Código Civil | 1855 |
| Código de Comercio | 1865 |
| Código Penal | 1874 |
| Código de Procedimiento Civil | 1902 |
| Código de Procedimiento Penal | 1906 |
| Código Orgánico de Tribunales | 1943 |
| Código de Justicia Militar, Sanitario, de Aguas, de Minería, Aeronáutico, Tributario, del Trabajo, Procesal Penal, de Derecho Internacional Privado | varios |

Son cerca de mil commits que cubren desde la publicación original de cada cuerpo
legal hasta las modificaciones ya programadas para entrar en vigencia en los
próximos años.

### La Constitución abarca cuatro textos constitucionales

BCN no registra la historia constitucional como una sola norma, sino como una
cadena de cuerpos distintos. `constitucion/constitucion-politica-de-la-republica-de-chile.md`
los une en una sola línea de tiempo continua:

| Texto | Período con versiones en BCN |
|---|---|
| Constitución de 1833 | 1833-05-25 → 1888-08-10 |
| Constitución de 1925 (DTO 1333) | 1971-10-25 → 1977-03-12 |
| Constitución de 1980 (DL 3464) | 1980-08-11 → 2005-08-26 |
| Texto refundido de 2005 (DTO 100) | 2005-09-22 → presente |

Los commits que estrenan un texto nuevo lo dicen en su mensaje ("Inicia..."):
ahí el diff es el reemplazo completo del cuerpo anterior, no una reforma
puntual. BCN no tiene texto versionado para todo el período, así que quedan
huecos entre 1888 y 1971, y entre 1977 y 1980.

Como ejemplo de lo que permite ver: el commit de la Ley 18.825 (1989) muestra
en su diff la incorporación de los tratados internacionales de derechos humanos
al artículo 5° y la derogación del artículo 8°.

## Autoría de los commits

Cada commit se atribuye a quien corresponde, según los datos de BCN:

- Si la ley nació como **moción parlamentaria**, el autor principal es el primer
  parlamentario de la lista de BCN, y el resto aparece como `Co-authored-by:`.
- Si no, el autor es quien **firmó la promulgación**: el Presidente de la
  República de la época (o, entre 1973 y 1990, el miembro de la Junta de
  Gobierno que firmó). Los demás firmantes —ministros y el resto de la Junta—
  quedan como coautores.
- Si BCN no registra ninguno de los dos, se usa el organismo emisor. Es lo
  habitual en los autos acordados, que firma la Corte Suprema y no un
  presidente.
- Si BCN **no registra siquiera qué norma causó el cambio** (201 commits),
  no se atribuye a nadie: el autor es el marcador neutro "Norma modificatoria no
  registrada". Deducir quién encabezaba el Ejecutivo en esa fecha sería inventar
  un dato que la fuente no tiene.

Cada commit incluye además una sección **Participantes** con el cargo textual de
cada persona ("Presidenta de la República", "Ministro de Hacienda", "autor de la
moción"), tomado de la promulgación de la propia norma.

### Dos advertencias sobre cómo leer la autoría

**El orden de los autores parlamentarios es el que entrega BCN y no implica
jerarquía.** No es alfabético ni por identificador, y su criterio no está
documentado. Que alguien figure como autor principal del commit sólo significa
que aparecía primero en esa lista, no que haya sido el redactor principal ni el
promotor de la ley. Todos los demás quedan acreditados como coautores y en la
sección Participantes.

**El campo `Author` mezcla dos roles distintos**, según lo que la fuente
permita: en una ley de moción es quien la *escribió*, y en las demás quien la
*firmó*. Por eso conviene no comparar autores por cantidad de commits: un
Presidente acumula cientos por firmar, mientras que quien redactó una ley suma
uno. La sección Participantes de cada commit sí distingue ambos roles.

Los correos son sintéticos (`...@sourced-from-bcn.leychile.invalid`): no existe
un correo público real para autoridades históricas. El objetivo es la
atribución y la auditoría, no tener un buzón que funcione.

## Verificabilidad

Cada archivo empieza con un bloque de front-matter que declara la URL exacta de
BCN de la que salió, el `idNorma`, la fecha de la versión y el momento de la
descarga. El mensaje de cada commit repite esa fuente. Cualquiera puede volver a
pedir esa URL y comprobar que el contenido coincide.

## Dos advertencias sobre las fechas

- **Commits fechados el 1970-01-01**: git no acepta fechas anteriores a la
  época Unix, y buena parte de esta historia es del siglo XIX. Esos commits
  llevan una fecha git sintética que preserva el orden cronológico; la fecha de
  vigencia real siempre está en el mensaje del commit y en el front-matter del
  archivo.
- **Commits con fecha futura**: no son un error. BCN publica modificaciones ya
  aprobadas cuya entrada en vigencia está programada para más adelante.

## Cómo se genera

Con el pipeline de
[leychile](https://github.com/diegocaro/leychile), que descarga los
datos de BCN y construye esta historia de forma reproducible.

<!-- INDICE-GENERADO -->

## Índice

### Constitución

| Documento | Versiones en el repo |
|---|---|
| [constitucion-politica-de-la-republica-de-chile](constitucion/constitucion-politica-de-la-republica-de-chile.md) | ver `git log -- constitucion/constitucion-politica-de-la-republica-de-chile.md` |

### Códigos

| Código | Versiones en el repo |
|---|---|
| [codigo-aeronautico](codigos/codigo-aeronautico.md) | ver `git log -- codigos/codigo-aeronautico.md` |
| [codigo-civil](codigos/codigo-civil.md) | ver `git log -- codigos/codigo-civil.md` |
| [codigo-de-aguas](codigos/codigo-de-aguas.md) | ver `git log -- codigos/codigo-de-aguas.md` |
| [codigo-de-comercio](codigos/codigo-de-comercio.md) | ver `git log -- codigos/codigo-de-comercio.md` |
| [codigo-de-derecho-internacional-privado](codigos/codigo-de-derecho-internacional-privado.md) | ver `git log -- codigos/codigo-de-derecho-internacional-privado.md` |
| [codigo-de-justicia-militar](codigos/codigo-de-justicia-militar.md) | ver `git log -- codigos/codigo-de-justicia-militar.md` |
| [codigo-de-mineria](codigos/codigo-de-mineria.md) | ver `git log -- codigos/codigo-de-mineria.md` |
| [codigo-de-procedimiento-civil](codigos/codigo-de-procedimiento-civil.md) | ver `git log -- codigos/codigo-de-procedimiento-civil.md` |
| [codigo-de-procedimiento-penal](codigos/codigo-de-procedimiento-penal.md) | ver `git log -- codigos/codigo-de-procedimiento-penal.md` |
| [codigo-del-trabajo](codigos/codigo-del-trabajo.md) | ver `git log -- codigos/codigo-del-trabajo.md` |
| [codigo-organico-de-tribunales](codigos/codigo-organico-de-tribunales.md) | ver `git log -- codigos/codigo-organico-de-tribunales.md` |
| [codigo-penal](codigos/codigo-penal.md) | ver `git log -- codigos/codigo-penal.md` |
| [codigo-procesal-penal](codigos/codigo-procesal-penal.md) | ver `git log -- codigos/codigo-procesal-penal.md` |
| [codigo-sanitario](codigos/codigo-sanitario.md) | ver `git log -- codigos/codigo-sanitario.md` |
| [codigo-tributario](codigos/codigo-tributario.md) | ver `git log -- codigos/codigo-tributario.md` |

### Normas modificatorias

| Tipo | Cantidad | Carpeta |
|---|---|---|
| Auto Acordado | 19 | [`normas/auto-acordado/`](normas/auto-acordado/) |
| Aviso | 3 | [`normas/aviso/`](normas/aviso/) |
| Decreto | 4 | [`normas/decreto/`](normas/decreto/) |
| Decreto con Fuerza de Ley | 8 | [`normas/decreto-con-fuerza-de-ley/`](normas/decreto-con-fuerza-de-ley/) |
| Decreto Ley | 52 | [`normas/decreto-ley/`](normas/decreto-ley/) |
| Ley | 635 | [`normas/ley/`](normas/ley/) |
| Rectificación | 2 | [`normas/rectificacion/`](normas/rectificacion/) |
| Sentencia | 5 | [`normas/sentencia/`](normas/sentencia/) |
