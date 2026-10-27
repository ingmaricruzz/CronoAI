# CronoAI · Generador de cronograma

Herramienta web para armar el cronograma de desarrollo de un proyecto a partir de sus historias de usuario (HU), sus puntos de historia y sus dependencias. Calcula las fechas día a día según los desarrolladores disponibles, excluye fines de semana, festivos de Colombia y días no laborables, y exporta el cronograma a Excel.

Es un único archivo `index.html`: no requiere instalación, servidor ni base de datos. Todo el cálculo se hace en el navegador y los datos no salen del equipo de quien la usa.

## Cómo se usa

La primera vez, la herramienta abre en blanco y arranca un **tutorial interactivo** que recorre cada sección con un proyecto de ejemplo. Se puede saltar en cualquier momento (también con *Esc*); al final se elige entre empezar en blanco o quedarse con el ejemplo. Para repetirlo, usa el botón **«Tutorial»** del encabezado.

1. **Parámetros:** nombre del proyecto, fecha de inicio (por defecto, hoy), número de desarrolladores, holgura en semanas y días no laborables adicionales.
2. **Historias de usuario:** escríbelas en la tabla, pégalas desde Excel (columnas *Código, Nombre, Puntos de historia, Dependencias*) o ábrelas desde un archivo `.xlsx`. Con **«Cargar ejemplo»** puedes ver un proyecto de muestra.
3. **Resultado:** la página muestra al instante las fechas de inicio y fin, la línea de tiempo (por historia o por desarrollador), el cronograma diario y el resumen por historia.
4. **Descargar Excel:** genera el archivo con el cronograma para compartirlo con el equipo.

## Reglas de cálculo

- **1 punto de historia = 4 horas =** dedicación diaria máxima de un desarrollador.
- **Calendario:** lunes a viernes. Se pueden excluir los festivos de Colombia (Ley Emiliani, calculados para cualquier año) y agregar otros días no laborables, una fecha o un rango por línea (`2026-12-24`, `24/12/2026`, `2026-12-28 a 2027-01-08`).
- **Dependencias en flujo continuo:** una HU dependiente arranca el mismo día en que termina su predecesora. Si a una HU le queda menos de un día de trabajo, un desarrollador la termina y los demás empiezan la siguiente, para no perder capacidad.
- **Último día del proyecto:** las horas que quedan de la última HU se reparten por igual entre los desarrolladores. Se redondea hacia abajo si la fracción es menor o igual a 0,5 y hacia arriba si es mayor.
- **Holgura:** 1 semana = 5 días hábiles; los festivos y días no laborables no cuentan. Los días se reparten por igual entre las HU y el sobrante va a las primeras en orden de desarrollo. Cada HU recibe su holgura justo después de terminar; en esos días todo el equipo queda asignado a esa HU y no se consumen puntos planeados.

## El Excel exportado

| Hoja | Contenido |
|---|---|
| Historias de usuario | Puntos, horas, dependencias, inicio, fin, días con trabajo y días de holgura de cada HU. |
| Cronograma | Una fila por día hábil: horas por desarrollador, HU, puntos planeados y restantes. *Puntos ejecutados*, *Restantes reales* y *Observación* quedan en blanco para que el equipo los diligencie durante la ejecución. |
| Supuestos y notas | Parámetros, calendario, orden de desarrollo y reglas aplicadas. |
| Festivos Colombia | Festivos de los años que abarca el proyecto (si se excluyen). |

**Para retomar un proyecto**, abre el Excel exportado con el botón **«Abrir Excel»**: la herramienta recupera todos los parámetros y las historias desde una hoja oculta (*Datos cronoAI*). Los cambios se hacen en la herramienta y se vuelve a exportar; si se editan las historias directamente en el Excel, se ignoran al abrirlo.

## Dónde quedan los datos

Lo que se escribe en la herramienta se guarda automáticamente en el navegador (`localStorage`): un proyecto por navegador. Para guardar varios proyectos, compartirlos o cambiar de equipo, usa el Excel exportado.

Exportar e importar Excel usa la librería [ExcelJS](https://github.com/exceljs/exceljs) desde cdnjs, por lo que esas dos funciones requieren conexión a internet.

## Publicación

El sitio se publica con GitHub Pages desde la rama `main`, carpeta raíz (*Settings → Pages → Deploy from a branch*). Cada cambio que se sube a `main` se publica automáticamente en unos minutos.

Para usarla sin publicar, basta con abrir `index.html` en Edge o Chrome.
