*Este proyecto ha sido creado como parte del currículo de 42 por acandrea.*

# Libft

## Descripción

**Libft** es mi primera librería en C. Su objetivo es reimplementar desde cero un
conjunto de funciones de uso general —muchas de ellas copias de funciones de la
librería estándar de C (`libc`)— para entender en profundidad cómo funcionan por
dentro y disponer de una caja de herramientas reutilizable en los siguientes
proyectos del cursus.

El resultado de compilar el proyecto es un archivo de librería estática llamado
`libft.a`, que se puede enlazar con cualquier otro programa en C.

La librería se divide en tres partes:

1. **Funciones de la libc** (`ft_isalpha`, `ft_strlen`, `ft_memcpy`, `ft_atoi`,
   `ft_calloc`, `ft_strdup`, …): réplicas fieles de funciones estándar, con el
   prefijo `ft_`.
2. **Funciones adicionales** (`ft_substr`, `ft_strjoin`, `ft_strtrim`,
   `ft_split`, `ft_itoa`, `ft_strmapi`, `ft_striteri`, `ft_putstr_fd`, …):
   utilidades que no existen en la libc o que se comportan de forma distinta.
3. **Listas enlazadas** (bonus) (`ft_lstnew`, `ft_lstadd_back`, `ft_lstmap`, …):
   funciones para crear y manipular listas enlazadas simples.

## Instrucciones

### Compilación

```sh
make          # compila la parte obligatoria  -> genera libft.a
make bonus    # compila además las funciones de listas (bonus)
make clean    # borra los archivos objeto (.o)
make fclean   # borra los .o y libft.a
make re       # equivale a fclean + all
```

### Uso en otro programa

```c
#include "libft.h"

int	main(void)
{
	char	*saludo;

	saludo = ft_strjoin("Hola, ", "mundo!");
	ft_putendl_fd(saludo, 1);
	free(saludo);
	return (0);
}
```

Compilando y enlazando contra la librería:

```sh
cc main.c -L. -lft -I. -o programa
./programa
```

- `-L.` indica que busque librerías en el directorio actual.
- `-lft` enlaza con `libft.a` (el prefijo `lib` y el sufijo `.a` se omiten).
- `-I.` indica dónde encontrar `libft.h`.

## Características (lista de funciones)

| Parte | Funciones |
|-------|-----------|
| 1 — libc | `ft_isalpha` `ft_isdigit` `ft_isalnum` `ft_isascii` `ft_isprint` `ft_strlen` `ft_memset` `ft_bzero` `ft_memcpy` `ft_memmove` `ft_strlcpy` `ft_strlcat` `ft_toupper` `ft_tolower` `ft_strchr` `ft_strrchr` `ft_strncmp` `ft_memchr` `ft_memcmp` `ft_strnstr` `ft_atoi` `ft_calloc` `ft_strdup` |
| 2 — adicionales | `ft_substr` `ft_strjoin` `ft_strtrim` `ft_split` `ft_itoa` `ft_strmapi` `ft_striteri` `ft_putchar_fd` `ft_putstr_fd` `ft_putendl_fd` `ft_putnbr_fd` |
| 3 — listas (bonus) | `ft_lstnew` `ft_lstadd_front` `ft_lstsize` `ft_lstlast` `ft_lstadd_back` `ft_lstdelone` `ft_lstclear` `ft_lstiter` `ft_lstmap` |

## Decisiones técnicas

- Todas las funciones siguen la **Norma** de 42: máximo 25 líneas por función,
  80 columnas por línea, sin `for`, sin ternarios, sin variables globales, y las
  funciones auxiliares se declaran `static`.
- Se compila con `cc -Wall -Wextra -Werror` y la librería se genera con `ar rcs`.
- `ft_itoa` y `ft_putnbr_fd` usan un `long` interno para poder manejar
  `INT_MIN` (-2147483648) sin desbordamiento.
- `ft_calloc` protege contra el desbordamiento de `count * size`.
- El `Makefile` no hace *relink*: ejecutar `make` dos veces seguidas no recompila.

## Recursos

- Páginas del manual de Linux/BSD de cada función (`man 3 strlen`, `man 3 atoi`,
  `man 3 memcpy`, etc.).
- *The C Programming Language* (Kernighan & Ritchie).
- [cppreference.com](https://en.cppreference.com/w/c) para el comportamiento
  estándar de las funciones de cadena y memoria.
- La Norma de 42 (`norminette`).

### Uso de IA

Se documenta aquí el uso de inteligencia artificial durante el proyecto, según
exige la hoja del enunciado:

- Se usó IA como apoyo para **explicar** el comportamiento exacto de algunas
  funciones del `man` (por ejemplo, las diferencias de valor de retorno de
  `strlcat` cuando el buffer es más corto que el destino) y para **revisar**
  casos límite (manejo de `INT_MIN`, `calloc(0, 0)`, cadenas vacías en `split`).
- La lógica, la implementación final y las pruebas fueron comprendidas y
  validadas manualmente compilando con `-Wall -Wextra -Werror` y comparando la
  salida contra las funciones originales de la libc.
