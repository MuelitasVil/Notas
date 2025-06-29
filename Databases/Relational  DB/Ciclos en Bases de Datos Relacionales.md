
## ¿Qué es un ciclo?

  
Un **ciclo en una base de datos relacional** ocurre cuando existe una **cadena cerrada de dependencias entre claves foráneas**, es decir, una tabla A necesita a B para existir, B necesita a C, y C necesita a A. Esto **rompe la lógica de inserción y borrado** y puede causar errores como:

```

Cannot add or update a child row: a foreign key constraint fails

```

---

##  Ciclo real 


Aquí sí se forma una **ruta cerrada de dependencias**, lo cual puede causar errores al insertar o borrar datos.

### Ejemplo:

  
  

```plaintext

         +----------------+

         |   empleado     |

         +----------------+

         | id (PK)        |

         | nombre         |

         | jefe_id (FK) ─────┐

         | departamento_id ─┐│

         +----------------+ ││

                            ▼▼

                      +----------------+

                      | departamento   |

                      +----------------+

                      | id (PK)        |

                      | nombre         |

                      | director_id ───────┐

                      +----------------+  │

                            ▲              │

                            └──────────────┘

```

  
  

### Explicación:

- `empleado.departamento_id → departamento.id`

- `departamento.director_id → empleado.id`

- Ambas tablas dependen mutuamente para existir.

- No se puede insertar ninguna sin que la otra ya exista → **Ciclo real**.

  

---

## Ciclo no real

email_sender
  ↓
email_sender_unit
  ↓
unit_unal
  ↓
unit_school_associate
  ↓
school
  ↓
email_sender_school_associate
  ↓
email_sender_school
  ↓
email_sender (¿regresa?)


### Clave para determinar si hay un **ciclo real**:

Un **ciclo relacional real** ocurre **solo si las claves foráneas entre estas tablas forman una dependencia circular**, es decir:

- Cada tabla **depende de otra para poder insertarse** (por claves foráneas).
    
- **Y alguna de esas tablas depende a su vez de la primera**.
    

En tu caso:

1. `email_sender_unit.sender_id → email_sender.id`
    
2. `email_sender_school_associate.email_sender_school → email_sender_school.email_sender_school`
    

🔍 Pero: **`email_sender_school` no depende de `email_sender` ni por clave foránea ni lógica.**

---

![[Dned modelo de datos.png]]
### Por qué **no es un ciclo real**:

- Aunque en el _modelo conceptual_ parezca que recorres el grafo y vuelves al punto de origen, **no hay ninguna clave foránea que cree una dependencia inversa directa** hacia `email_sender` desde el extremo opuesto.
    
- Las tablas `email_sender_school` y `email_sender` **son independientes entre sí**. Aunque ambas contienen correos, no se referencian mutuamente por clave foránea.
- 
##  Definiciones


### Ciclo real


> Una cadena de claves foráneas que se cierra sobre sí misma, generando dependencia circular y **rompiendo la lógica relacional**. No se pueden insertar registros sin violar las restricciones. ❌


### Ciclo no real


> Una relación lineal (aunque larga) entre tablas, en la que no se vuelve a una tabla anterior. Es segura, se puede insertar y eliminar con orden lógico. ✅

  

---

