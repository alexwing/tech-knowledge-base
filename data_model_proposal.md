### Data Model Documentation

#### Introduction

This document describes the naming convention and structure of the fields in the database tables. Each table has a descriptive name and an alias used as a prefix for the fields. The column names follow the convention `[alias]_[type]_[name]`, making it easier to identify the context and data type of each field.

#### Alias Conventions

Table aliases should consist of three unique letters directly related to the table name. Examples:

- **Table**: `example` → **Alias**: `exa`
- **Table**: `other` → **Alias**: `oth`
- **Table**: `usuarios` → **Alias**: `usr`
- **Table**: `productos` → **Alias**: `prd`

> Aliases must be unique and consistent across tables to avoid ambiguities.

#### Field Naming Conventions

Each table field should follow this structure:

- `[alias]`: Table alias (three letters). For example, `exa` for `example` or `oth` for `other`.

- `[type]`: Identifier for the data type of the field, as defined below:

  - **ia**: Auto-incrementing Integer (used for primary keys).
  - **i**: Integer (non-auto-incrementing).
  - **c**: Character (short text string).
  - **cl**: Character Large (long text string).
  - **d**: Date.
  - **dt**: DateTime (full timestamp with date and time).
  - **b**: Boolean (true/false).
  - **f**: Float (decimal).
  - **fk**: Foreign Key (reference to another table).
  - **bl**: Binary (binary data).
  - **o**: Other uncategorized types.

- `[name]`: Descriptive identifier for the field.

#### Example of Conventions

If we have two tables: `example` and `other`, the convention would be as follows:

##### Table: `example`

- **Alias**: `exa`

| **Field** | **Type** | **Description** |
|-----------|----------|------------------|
| `exa_ia_id` | `INTEGER AUTONUMERIC` | Unique identifier for the table, primary key. |
| `exa_c_nombre` | `VARCHAR(255)` | Name of the example (short text string). |
| `exa_d_fecha` | `DATE` | Date associated with the example. |
| `exa_dt_created` | `DATETIME` | Timestamp of record creation. |
| `oth_fk_id` | `INTEGER` | Foreign key referencing the table `other`. |

##### Table: `other`

- **Alias**: `oth`

| **Field** | **Type** | **Description** |
|-----------|----------|------------------|
| `oth_ia_id` | `INTEGER AUTONUMERIC` | Unique identifier for the table `other`. |
| `oth_c_descripcion` | `VARCHAR(255)` | Description of the element in `other`. |
| `oth_d_fechacreacion` | `DATE` | Creation date. |
| `oth_b_activo` | `BOOLEAN` | Indicator of whether the element is active (1 or 0). |

### Example of Table Relationships

The relationship between the table `example` and `other` is established through the field `oth_fk_id`, which is a foreign key in the table `example` pointing to `oth_ia_id` in `other`.

- **Table `example`:**

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

- **Table `other`:**

```sql
CREATE TABLE other (
    oth_ia_id INTEGER AUTONUMERIC PRIMARY KEY,
    oth_c_descripcion VARCHAR(255),
    oth_d_fechacreacion DATE,
    oth_b_activo BOOLEAN
);
```

### General Notes

1. **Use of Foreign Keys**: Foreign keys should be clearly documented, indicating their relationship with the source table. The prefix `fk` is used to indicate this relationship (`oth_fk_id`).

2. **Consistent Aliases**: Aliases must be consistent across different tables and must be unique to avoid ambiguities.

3. **Descriptive Names**: Field names should be sufficiently descriptive to clearly indicate the content of the field.

4. **Documentation**: Each field should be documented with its data type, size, and a brief description of its purpose.

This structure will provide a clear and uniform guide for documenting and designing database tables, facilitating data maintenance and understanding.

### Disadvantages

1. **Complexity**: The naming convention may be complex for users unfamiliar with the structure.

2. **Extensibility**: The convention may not be sufficient to cover all use cases or data types.

3. **Maintenance**: Requires additional effort; if there is a change in data type, the project's source code using the database must also be updated.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/20833930/f9b04c49-7cc5-4385-836d-798c2f6f6dbd/propuesta_modelos_de_datos.md
