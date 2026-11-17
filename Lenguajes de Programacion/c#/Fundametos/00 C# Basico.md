
## Diferencia entre .net y .net framework 

| .Net                                  | .Net Framework                                         |
| ------------------------------------- | ------------------------------------------------------ |
| Runs in linux, macOs y windows        | Only rus on widows                                     |
| Open-source and accepts contributions | Source code asvalable but does not accept contributios |
| Innovation happents here              | Security and stable                                    |
| ot shipped with operating system      | includet with windows                                  |
| enw developers                        |                                                        |

## Tipos de variable

Texto
Numeros
Booleanos
Listas
Objetos

Se declaran varibles tanto de forma implicita como explicita, sin embargo esto implioca que haya tipado diamico, es decir :

```c#
int edad = 2
var edad = 2
```

## 3. Métodos

**Java**

```c#
static int sumar(int a, int b) {
    return a + b;
}
```

**C#**

```c#
static int Sumar(int a, int b)
{
    return a + b;
}
```

En ambos lenguajes se especifican el tipo de retorno y los tipos de los parámetros. Una convención habitual de C# es escribir los nombres de métodos en **PascalCase**: `Sumar`, `ObtenerUsuario`.

## 4. Clases, campos y propiedades

En Java, a menudo escribías campos privados y getters. En C# existe una sintaxis especial llamada **propiedad**:

```c#
class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }
}
```

Puedes usarla así:

```c#
var persona = new Persona();
persona.Nombre = "Manuel";
persona.Edad = 25;

Console.WriteLine(persona.Nombre);
```

`{ get; set; }` define automáticamente almacenamiento y acceso. Es una de las razones por las que normalmente no necesitas Lombok para generar getters y setters sencillos.

Para una propiedad de solo lectura después de construir el objeto:

```c#
class Persona
{
    public string Nombre { get; }

    public Persona(string nombre)
    {
        Nombre = nombre;
    }
}
```

## 3. Métodos

**Java**

```c#
static int sumar(int a, int b) {
    return a + b;
}
```

**C#**

```c#
static int Sumar(int a, int b)
{
    return a + b;
}
```

En ambos lenguajes se especifican el tipo de retorno y los tipos de los parámetros. Una convención habitual de C# es escribir los nombres de métodos en **PascalCase**: `Sumar`, `ObtenerUsuario`.

## 4. Clases, campos y propiedades

En Java, a menudo escribías campos privados y getters. En C# existe una sintaxis especial llamada **propiedad**:

```c#
class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }
}
```

Puedes usarla así:

```c#
var persona = new Persona();
persona.Nombre = "Manuel";
persona.Edad = 25;

Console.WriteLine(persona.Nombre);
```

`{ get; set; }` define automáticamente almacenamiento y acceso. Es una de las razones por las que normalmente no necesitas Lombok para generar getters y setters sencillos.

Para una propiedad de solo lectura después de construir el objeto:

```c#
class Persona
{
    public string Nombre { get; }

    public Persona(string nombre)
    {
        Nombre = nombre;
    }
}
```

## 5. Condicionales y ciclos

Son muy similares a Java:

```
if (edad >= 18)
{
    Console.WriteLine("Mayor de edad");
}
else
{
    Console.WriteLine("Menor de edad");
}

for (int i = 0; i < 3; i++)
{
    Console.WriteLine(i);
}

foreach (var numero in numeros)
{
    Console.WriteLine(numero);
}
```

Una diferencia visible: C# usa `foreach` igual que Java, aunque en C# también se escribe así para colecciones.

## Ejercicio breve

¿Qué imprime este programa y qué tipo deduce C# para `mensaje`?

```
var nombre = "Ana";
var edad = 20;
var mensaje = $"{nombre} tiene {edad} años";

Console.WriteLine(mensaje);
```

La expresión `$"..."` es una cadena interpolada, equivalente a una forma más cómoda de construir cadenas con variables. En el siguiente paso podemos practicar con ejercicios y luego pasar a métodos, clases y propiedades con un ejemplo un poco más completo.