## ¿Cómo se documenta?

Se utiliza una cadena entre triple comilla (`"""`) llamada *docstring* ubicada justo después de la definición de una función, clase o módulo.

## ¿Qué estándar se sigue?

- Se recomienda seguir **PEP 257** (convención de docstrings).
- Los estilos más comunes para estructurar los docstrings son:
  - **Sphinx / reStructuredText** (más usado)
  - **Google Style**
  - **NumPy Style**

## Ejemplo de docstring con formato Sphinx (reStructuredText)

```python
def mi_funcion(nombre: str, edad: int) -> bool:
    """
    Valida si un usuario es mayor de edad.

    :param nombre: Nombre del usuario.
    :param edad: Edad del usuario.
    :return: True si es mayor de edad, False si no.
    """
```


##  Ejemplo de docstring en clase y método (Python / Sphinx)

```python
class PeriodService:
    """
    Servicio para gestionar periodos académicos en la base de datos.

    Este servicio encapsula la lógica de negocio relacionada con la creación de
    periodos, utilizando SQLAlchemy y un modelo de datos validado con Pydantic.

    Métodos:
        create_period(data, session):
            Crea un nuevo registro de periodo a partir de un PeriodInput.
    """

    @staticmethod
    def create_period(data: PeriodInput, session: Session) -> Period:
        """
        Crea un nuevo periodo en la base de datos.

        :param data: Objeto PeriodInput con los datos del periodo.
        :param session: Sesión de SQLAlchemy para operar con la base de datos.
        :return: Objeto Period creado y persistido.
        """
        repo = PeriodRepository(session)
        period_data = data.dict(exclude={"cod_period"}, exclude_unset=True)
        period = Period(cod_period=data.cod_period, **period_data)
        return repo.create(period)