[REFERENCIA.md](https://github.com/user-attachments/files/29721477/REFERENCIA.md)
# MiniPy — Referencia técnica

## Tipos

| Hint | Java |
|---|---|
| `int` | `int` |
| `float` | `double` |
| `bool` | `boolean` |
| `str` | `String` |
| `list[int\|float\|bool\|str]` | `ArrayList<Integer\|Double\|Boolean\|String>` |

Sin listas anidadas, dict, tuple, `None`.

## Literales

```
42          int
3.14        float (requiere parte entera y decimal)
True False  bool
"x" 'x'     str (sin escapes)
```

## Variables

```
x = expr                # tipo inferido de la 1ra asignación
x: tipo = expr           # tipo explícito
```
Scope de función completa (no de bloque), salvo la variable de `for x in lista:`.

## Operadores (precedencia, mayor a menor)

| # | Op | Asoc |
|---|---|---|
| 1 | `not` `-`(unario) | — |
| 2 | `**` | izq* |
| 3 | `*` `/` `//` `%` | izq |
| 4 | `+` `-` | izq |
| 5 | `<` `>` `<=` `>=` | izq |
| 6 | `==` `!=` | izq |
| 7 | `and` | izq |
| 8 | `or` | izq |

`+=` `-=` `*=` `/=` soportados; `//=` `%=` `**=` no.

\* Python real: `**` es asoc. derecha. Acá es izquierda.

### Semántica de tipos

```
/            siempre double
//           floorDiv (int,int)->int ; si no, Math.floor
**           int**int->int ; si no, double
str + str    concat
==/!= str    .equals()
==/!= list   referencia, no contenido
str/list mezclados en + / comparación   no soportado
comparación encadenada a<b<c              no soportado
```

## Control de flujo

```
if expr: bloque
(elif expr: bloque)*
(else: bloque)?

while expr: bloque

for x in range(n): bloque
for x in range(a, b): bloque
for x in range(a, b, paso): bloque
for x in lista: bloque
```
Sin `break` `continue` `pass` `while/for...else`.

## Funciones

```
def nombre(p1: tipo, p2: tipo) -> tipo_ret:
    ...
    return expr

def sin_retorno(x: tipo):
    ...
```
- Type hints obligatorios en parámetros.
- Sin defaults, kwargs, `*args`/`**kwargs`, anidadas, lambdas.
- Recursión y orden de declaración libres.
- Requiere `def main():` sin parámetros como entry point.

## Listas

```
lst = []                  # asume list[int] si no se anota
lst: list[float] = []
lst = [1, 2, 3]            # tipo del primer elemento
lst.append(x)
lst[i]                     # get
lst[i] = x                 # set
len(lst)
```
Único método soportado: `.append()`. Sin slicing, comprehensions, `.pop()`, `.sort()`, etc.

## Built-ins

| Función | Traducción |
|---|---|
| `print(a, b, ...)` | `System.out.println` (join con espacio) |
| `len(x)` | `.size()` (lista) / `.length()` (str) |
| `range(n\|a,b\|a,b,paso)` | solo dentro de `for...in`, for clásico |

Cualquier otro nombre → llamada a función de usuario.

## Gramática (EBNF)

```
programa      := (defFuncion)*
defFuncion    := 'def' ID '(' params? ')' ('->' tipo)? ':' bloque
params        := param (',' param)*
param         := ID ':' tipo
tipo          := ID ('[' ID ']')?
bloque        := INDENT sentencia+ DEDENT

sentencia     := (asign | asignAum | retorno | sentExpr) NEWLINE | compuesta
asign         := ID (':' tipo)? '=' expr | ID '[' expr ']' '=' expr
asignAum      := ID ('+='|'-='|'*='|'/=') expr
retorno       := 'return' expr?
sentExpr      := ID '(' args? ')' | ID '.' ID '(' args? ')'
compuesta     := sentIf | sentWhile | sentFor
sentIf        := 'if' expr ':' bloque ('elif' expr ':' bloque)* ('else' ':' bloque)?
sentWhile     := 'while' expr ':' bloque
sentFor       := 'for' ID 'in' expr ':' bloque

expr := 'not' expr | '-' expr
      | expr '**' expr
      | expr ('*'|'/'|'//'|'%') expr
      | expr ('+'|'-') expr
      | expr ('<'|'>'|'<='|'>=') expr
      | expr ('=='|'!=') expr
      | expr 'and' expr | expr 'or' expr
      | ID '(' args? ')' | ID '.' ID '(' args? ')'
      | ID '[' expr ']' | '[' args? ']'
      | ID | INT | FLOAT | 'True' | 'False' | STRING | '(' expr ')'
```

## No soportado

Clases/herencia, dict/tuple/set, try/except, break/continue/pass, import,
f-strings/`.format()`, slicing, comprehensions, asignación múltiple,
desempaquetado, default/kwargs/`*args`/`**kwargs`, funciones anidadas,
lambdas, decoradores, generadores, `None`, `is`, `in` fuera de `for`,
comparación encadenada, hilos, I/O de archivos, librería estándar (salvo
`print`/`len`/`range`/`.append()`).
