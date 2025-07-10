
## ¿Qué es la PEP 8?

La **PEP 8** es la guía oficial de estilo para escribir código en Python. Su objetivo es mejorar la **legibilidad**, **consistencia** y **mantenibilidad** del código, especialmente en proyectos colaborativos.

---
## Principales convenciones

### 1. Longitud de línea

- Máximo recomendado: **79 caracteres**

- Para docstrings o comentarios: **72 caracteres**

- Black (formateador) usa por defecto hasta **88 caracteres**

---

### 2. Indentación

- Usa **4 espacios por nivel**

- No se recomienda usar tabs

```python

def funcion():

    if True:

        print("Indentación correcta")

```

  
---

### 3. Espacios en blanco

Evita espacios innecesarios:


```python

# Correcto

x = 1

def funcion(a, b=2): return a + b

  

# Incorrecto

x= 1

def funcion( a , b = 2 ): return a+b

```

---


### 4. Nombrado de variables

  

| Elemento     | Convención      | Ejemplo             |

|--------------|------------------|----------------------|

| Variables    | snake_case        | `nombre_usuario`     |

| Funciones    | snake_case        | `obtener_datos()`    |

| Clases       | PascalCase        | `UsuarioActivo`      |

| Constantes   | MAYÚSCULAS        | `MAX_INTENTOS`       |

---

### 5. Importaciones

  
- Cada importación en línea separada

- Orden recomendado:

  1. Librerías estándar

  2. Librerías de terceros

  3. Módulos locales


```python

import os

import sys

  

import requests

  

from app.models import Period

```


---

### 6. Comentarios y docstrings

#### Comentarios

- Empiezan con `#` y un espacio

- Evita comentarios obvios


```python

# Calcula el total con impuesto

total = precio * 1.19

```

#### Docstrings

- Se escriben con triple comilla ("""texto""")

- Aplican a módulos, clases y funciones


```python

def sumar(a, b):

    """

    Suma dos números enteros.

    :param a: Primer número

    :param b: Segundo número

    :return: Resultado de la suma

    """

    return a + b

```

  

---

  

### 7. Estructura recomendada de archivos

  

- Deja **2 líneas en blanco** entre funciones o clases a nivel de módulo

- Usa **1 línea en blanco** dentro de clases entre métodos

  

---

  

### 8. Herramientas útiles

  

- `flake8`: Linter que detecta errores de estilo

- `black`: Formateador automático de código

- `pylint`: Análisis profundo de estilo y errores potenciales

  

---

  

## Recursos

  

- [PEP 8 en python.org](https://peps.python.org/pep-0008/)

- [Guía de estilo de Google para Python](https://google.github.io/styleguide/pyguide.html)