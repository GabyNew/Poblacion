Selecion de 7 datos 

-Años: 1990, 2000, 2010, 2015, 2020, 2022, 2024.

-Población correspondiente: se puede tomar de fuentes confiables como el INE

2. Métodos de interpolación
Los métodos de interpolación son herramientas para estimar valores desconocidos basándose en datos conocidos. Aquí emplearemos Newton y Lagrange.

2.1 Interpolación de Newton
El polinomio de Newton se construye utilizando diferencias divididas:

-Construye la tabla de diferencias divididas.
-Usa los coeficientes para construir el polinomio.

2.2 Interpolación de Lagrange
El polinomio de Lagrange está dado por:

2.2.1 Calcula los términos L(x) para cada punto.

2.2.2 Suma los términos ponderados por  𝑦𝑖


------codigo------

from scipy.interpolate import lagrange
import numpy as np
import sympy as sp
from sympy import symbols, expand

# Datos históricos de población de Bolivia
tiempo = np.array([1990, 2000, 2010, 2015, 2020, 2022])
poblacion = np.array([5.55, 7.10, 9.12, 10.40, 11.51, 11.90])
x = symbols('x')

# Interpolación de Newton
def newton_interpolation(x_values, y_values):
    n = len(x_values)
    diff_table = np.zeros((n, n))
    diff_table[:, 0] = y_values

    for j in range(1, n):
        for i in range(n - j):
            diff_table[i, j] = (diff_table[i + 1, j - 1] - diff_table[i, j - 1]) / (x_values[i + j] - x_values[i])

    # Construcción del polinomio de Newton
    polynomial = 0
    for i in range(n):
        term = diff_table[0, i]
        for j in range(i):
            term *= (x - x_values[j])
        polynomial += term

    return expand(polynomial)

# Obtener polinomio de Newton
newton_poly = newton_interpolation(tiempo, poblacion)

# Interpolación de Lagrange
lagrange_poly = lagrange(tiempo, poblacion)

# Convertir polinomio de Lagrange a forma simbólica
lagrange_poly_sym = sum(poblacion[i] * np.prod([(x - tiempo[j]) / (tiempo[i] - tiempo[j]) for j in range(len(tiempo)) if j != i]) for i in range(len(tiempo)))

# Proyección para 2024
tiempo_2024 = 2024
pop_newton_2024 = newton_poly.subs(x, tiempo_2024)
pop_lagrange_2024 = lagrange_poly_sym.subs(x, tiempo_2024)





Año	Población (millones)

1990  |	5.55

2000	| 7.10

2010  |	9.12

2015  |	10.40

2020  |	11.51

2022  |	11.90
