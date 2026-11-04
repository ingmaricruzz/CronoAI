# CronoAI · Generador de cronograma

Herramienta web para armar el cronograma de desarrollo de un proyecto a partir de sus historias de usuario (HU), sus puntos de historia y sus dependencias. Calcula las fechas día a día según los desarrolladores disponibles, excluye fines de semana, festivos de Colombia y días no laborables, y exporta el cronograma a Excel.

Es un único archivo `index.html`: no requiere instalación, servidor ni base de datos. Todo el cálculo se hace en el navegador y los datos no salen del equipo de quien la usa.

## Cómo se usa

La primera vez, la herramienta abre en blanco y arranca un **tutorial interactivo** que recorre cada sección con un proyecto de ejemplo. Se puede saltar en cualquier momento (también con *Esc*); al final se elige entre empezar en blanco o quedarse con el ejemplo. Para repetirlo, usa el botón **«Tutorial»** del encabezado.

La página se organiza de arriba abajo: el resumen (inicio, fin, días hábiles…), la línea de tiempo a todo el ancho, los datos del proyecto (parámetros e historias de usuario, lado a lado) y el detalle (cronograma diario y resumen por historia, en pestañas). Los botones **«Abrir Excel»** y **«Descargar Excel»** están en el encabezado.

1. **Parámetros:** nombre del proyecto, fecha de inicio (por defecto, hoy), número de desarrolladores, holgura en semanas y días no laborables adicionales.
2. **Días no laborables y medios días:** elige la fecha (o un rango con *Hasta*), el tipo —**día completo** o **medio día**—, un motivo opcional y pulsa **«+ Agregar»**. También puedes hacer clic en la fecha de un día en la línea de tiempo: el primer clic lo marca como medio día, el segundo como no laborable y el tercero lo deja hábil otra vez.
3. **Historias de usuario:** escríbelas en la tabla, pégalas desde Excel (columnas *Código, Nombre, Puntos de historia, Dependencias*) o ábrelas desde un archivo `.xlsx`. Con **«Cargar ejemplo»** puedes ver un proyecto de muestra.
4. **Resultado:** la página muestra al instante las fechas de inicio y fin, la línea de tiempo (por historia o por desarrollador), el cronograma diario y el resumen por historia. Con **−** y **+** cambias el tamaño de la línea de tiempo.
5. **Descargar:** **«Descargar Excel»** genera el archivo con el cronograma para compartirlo con el equipo y **«Descargar PNG»** guarda la línea de tiempo completa como imagen (aunque en pantalla tenga scroll), lista para presentaciones.

## Reglas de cálculo

- **1 punto de historia = 4 horas =** dedicación diaria máxima de un desarrollador.
- **Calendario:** lunes a viernes. Se pueden excluir los festivos de Colombia (Ley Emiliani, calculados para cualquier año) y agregar otros días no laborables (una fecha o un rango), de día completo o de medio día.
- **Medio día:** cada desarrollador aporta 2 horas (0,5 puntos de historia). Cuenta como día hábil y, si cae en holgura, cuenta como un día de holgura. Si un día figura a la vez como medio día y como día completo, prevalece el día completo; un festivo prevalece sobre ambos.
- **Dependencias en flujo continuo:** una HU dependiente arranca el mismo día en que termina su predecesora. Si a una HU le queda menos de un día de trabajo, un desarrollador la termina y los demás empiezan la siguiente, para no perder capacidad.
- **Último día del proyecto:** las horas que quedan de la última HU se reparten por igual entre los desarrolladores. Se redondea hacia abajo si la fracción es menor o igual a 0,5 y hacia arriba si es mayor.
- **Holgura:** 1 semana = 5 días hábiles; los festivos y días no laborables no cuentan. Los días se reparten por igual entre las HU y el sobrante va a las primeras en orden de desarrollo. Cada HU recibe su holgura justo después de terminar; en esos días todo el equipo queda asignado a esa HU y no se consumen puntos planeados.

## El Excel exportado

| Hoja | Contenido |
|---|---|
| Historias de usuario | Puntos, horas, dependencias, inicio, fin, días con trabajo y días de holgura de cada HU. |
| Cronograma | Una fila por día hábil: horas por desarrollador, HU, puntos planeados y restantes. Los medios días se resaltan y traen la nota en *Observación*. *Puntos ejecutados*, *Restantes reales* y *Observación* quedan en blanco para que el equipo los diligencie durante la ejecución. |
| Supuestos y notas | Parámetros, calendario, orden de desarrollo y reglas aplicadas. |
| Festivos Colombia | Festivos de los años que abarca el proyecto (si se excluyen). |

**Para retomar un proyecto**, abre el Excel exportado con el botón **«Abrir Excel»**: la herramienta recupera todos los parámetros y las historias desde una hoja oculta (*Datos cronoAI*). Los cambios se hacen en la herramienta y se vuelve a exportar; si se editan las historias directamente en el Excel, se ignoran al abrirlo.

## Dónde quedan los datos

Lo que se escribe en la herramienta se guarda automáticamente en el navegador (`localStorage`): un proyecto por navegador. Para guardar varios proyectos, compartirlos o cambiar de equipo, usa el Excel exportado.

Exportar e importar Excel usa la librería [ExcelJS](https://github.com/exceljs/exceljs) desde cdnjs, por lo que esas dos funciones requieren conexión a internet.

## Publicación

El sitio se publica con GitHub Pages desde la rama `main`, carpeta raíz (*Settings → Pages → Deploy from a branch*). Cada cambio que se sube a `main` se publica automáticamente en unos minutos.

Para usarla sin publicar, basta con abrir `index.html` en Edge o Chrome.
