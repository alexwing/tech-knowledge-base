### Documentación del Modelo de Datos

#### Introducción
Este documento describe la convención de nombres y estructura de los campos de las tablas en la base de datos. Cada tabla tiene un nombre descriptivo y un alias que se usa como prefijo en los campos. Los nombres de las columnas siguen la convención `[alias]_[tipo]_[nombre]`, facilitando la identificación del contexto y el tipo de dato de cada campo.

#### Convenciones de Alias
Los alias de las tablas deben estar compuestos por tres letras únicas que se relacionen directamente con el nombre de la tabla. Ejemplos:

- **Tabla**: `example` → **Alias**: `exa`
- **Tabla**: `other` → **Alias**: `oth`
- **Tabla**: `usuarios` → **Alias**: `usr`
- **Tabla**: `productos` → **Alias**: `prd`

> Los alias deben ser únicos y mantenerse consistentes entre tablas para evitar ambigüedades.


#### Convenciones de Nombres para Campos

Cada campo de la tabla debe seguir esta estructura:

- `[alias]`: Alias de la tabla (tres letras). Por ejemplo, `exa` para `example` o `oth` para `other`.
- `[tipo]`: Identificador del tipo de dato del campo, como se define a continuación:
  - **ia**: Integer Autonumérico (utilizado para claves primarias).
  - **i**: Integer (entero no autonumérico).
  - **c**: Character (cadena de texto corta).
  - **cl**: Character Large (cadena de texto larga).
  - **d**: Date (fecha).
  - **dt**: DateTime (marca de tiempo completa con fecha y hora).
  - **b**: Boolean (verdadero/falso).
  - **f**: Float (decimal de coma flotante).
  - **fk**: Foreign Key (clave foránea, referencia a otra tabla).
  - **bl**: Binary (datos binarios).
  - **o**: Otros tipos no categorizados.
- `[nombre]`: Identificador descriptivo del campo.

#### Ejemplo de Convenciones

Si tenemos dos tablas: `example` y `other`, la convención sería la siguiente:

##### Tabla: `example`  
- **Alias**: `exa`

| **Campo**       | **Tipo**            | **Descripción**                                      |
|-----------------|---------------------|------------------------------------------------------|
| `exa_ia_id`     | `INTEGER AUTONUMERIC`| Identificador único de la tabla, clave primaria.      |
| `exa_c_nombre`  | `VARCHAR(255)`       | Nombre del ejemplo (cadena de texto corta).           |
| `exa_d_fecha`   | `DATE`               | Fecha asociada al ejemplo.                            |
| `exa_dt_created`| `DATETIME`           | Marca de tiempo de creación del registro.             |
| `oth_fk_id`     | `INTEGER`            | Clave foránea que referencia la tabla `other`.        |

##### Tabla: `other`
- **Alias**: `oth`

| **Campo**       | **Tipo**             | **Descripción**                                     |
|-----------------|----------------------|-----------------------------------------------------|
| `oth_ia_id`     | `INTEGER AUTONUMERIC` | Identificador único de la tabla `other`.             |
| `oth_c_descripcion`| `VARCHAR(255)`      | Descripción del elemento en `other`.                 |
| `oth_d_fechacreacion`| `DATE`            | Fecha de creación.                                   |
| `oth_b_activo`  | `BOOLEAN`             | Indicador de si el elemento está activo (1 o 0).      |


### Ejemplo de Relación entre Tablas
La relación entre la tabla `example` y `other` se establece a través del campo `oth_fk_id`, que es una clave foránea en la tabla `example` que apunta a `oth_ia_id` en `other`.

- **Tabla `example`:**
```sql
CREATE TABLE example (
  exa_ia_id INTEGER AUTONUMERIC PRIMARY KEY,
  exa_c_nombre VARCHAR(255),
  exa_d_fecha DATE,
  exa_dt_created DATETIME,
  oth_fk_id INTEGER,
  FOREIGN KEY (oth_fk_id) REFERENCES other(oth_ia_id)
);
```

- **Tabla `other`:**
```sql
CREATE TABLE other (
  oth_ia_id INTEGER AUTONUMERIC PRIMARY KEY,
  oth_c_descripcion VARCHAR(255),
  oth_d_fechacreacion DATE,
  oth_b_activo BOOLEAN
);
```

### Notas Generales

1. **Uso de Claves Foráneas**: Las claves foráneas deben estar claramente documentadas y señalar la relación con la tabla de origen. Se usa el prefijo `fk` para indicar esta relación (`oth_fk_id`).

2. **Alias Consistentes**: Los alias deben mantenerse consistentes entre diferentes tablas y deben ser únicos para evitar ambigüedades.

3. **Nombres Descriptivos**: Los nombres de los campos deben ser suficientemente descriptivos para indicar claramente el contenido del campo.

4. **Documentación**: Cada campo debe estar documentado con su tipo de dato, tamaño, y una breve descripción de su propósito.

Esta estructura te permitirá tener una guía clara y uniforme para documentar y diseñar las tablas de la base de datos, facilitando el mantenimiento y comprensión de los datos.

### Inconvenientes

1. **Complejidad**: La convención de nombres puede resultar compleja para usuarios no familiarizados con la estructura.
   
2. **Extensibilidad**: La convención puede no ser suficiente para cubrir todos los casos de uso o tipos de datos.
   
3. **Mantenimiento**: Requiere un esfuerzo adicional, en caso de un cambio en el tipo de dato, se debe actualizar también el código fuente del proyecto que hace uso de la base de datos.

