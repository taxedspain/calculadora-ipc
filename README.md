# Calculadora IRPF 2025

Calculadora web que responde a una pregunta concreta: cuánto tiene que subir tu salario **bruto** para que tu salario **neto** crezca exactamente lo mismo que el IPC.

Como el IRPF es progresivo, cada euro adicional tributa a un tipo marginal más alto que el tipo medio que ya pagabas. Una subida bruta igual a la inflación deja siempre un neto por debajo de la inflación. A esa diferencia se la llama progresividad en frío o rémora fiscal, y es lo que la herramienta cuantifica.

**Ejercicio fiscal:** 2025 (declaración presentada en 2026).

👉 [Ver la calculadora](https://taxedspain.github.io/calculadora-ipc/)


## Qué hace

- Calcula la subida bruta necesaria para cada nivel de renta, con el IPC que elijas.
- Compara ese resultado con el IPC de referencia y con el 2,5% pactado para los empleados públicos en 2025.
- Calcula tu caso concreto: neto, tipo efectivo, tipo marginal real y el bruto que tendrías que pedir.
- Compara la misma nómina en las 17 comunidades y territorios forales.
- Muestra una tabla con cotización, IRPF, neto, tipo efectivo y tipo marginal por tramo de renta.

## Un resultado que conviene mirar

El esfuerzo no crece de forma continua con el sueldo. El pico está entre los 18.000 y los 21.000 € brutos, donde el tipo marginal real supera el 55% porque se retiran a la vez la reducción por rendimientos del trabajo (que cae 1,75 € por cada euro adicional) y la deducción de 340 € de la Ley 5/2025. Por encima de ese tramo, la subida necesaria se estabiliza en torno al 3–3,5% para un IPC del 2,7%.

## Cómo calcula

Régimen común, contribuyente soltero, sin hijos ni discapacidad, con rentas exclusivamente del trabajo:

1. Cotización del trabajador sobre el bruto, con el tope de la base máxima y la cuota de solidaridad por encima de ella.
2. Rendimiento neto = bruto − cotización − 2.000 € de gastos deducibles generales.
3. Reducción por obtención de rendimientos del trabajo (art. 20 LIRPF) si procede.
4. Cuota íntegra = escala estatal + escala autonómica sobre la base liquidable, restando la cuota correspondiente al mínimo personal por el método legal.
5. Cuota líquida = cuota íntegra − deducción por rendimientos del trabajo (Ley 5/2025).
6. Neto = bruto − cotización − cuota líquida.

Navarra y País Vasco usan su tarifa foral única, que sustituye por completo a la escala estatal, con la deducción por mínimo personal y la deducción o bonificación del trabajo correspondientes.

La subida necesaria se resuelve por bisección sobre la función de neto, no por aproximación lineal, así que es exacta también en los saltos de tramo.

## Datos del ejercicio 2025

| Concepto | Valor |
|---|---|
| Escala estatal | 9,5% / 12% / 15% / 18,5% / 22,5% / 24,5% |
| Cotización del trabajador | 6,48% (4,70 + 1,55 + 0,10 + 0,13 de MEI) |
| Base máxima de cotización | 4.909,50 €/mes · 58.914 €/año |
| Cuota de solidaridad (trabajador) | 0,15% / 0,17% / 0,19% sobre el exceso |
| Gastos deducibles generales | 2.000 € |
| Reducción art. 20 LIRPF | 7.302 € hasta 14.852 €, decreciente hasta 19.747,50 € |
| Mínimo personal | 5.550 € |
| Deducción rentas bajas (Ley 5/2025) | hasta 340 €, hasta 18.276 € de rendimiento íntegro |
| SMI | 16.576 € anuales en 14 pagas |
| IPC | media anual 2,7%; interanual de diciembre 2,9% |
| Subida sector público | 2,5% retroactivo a 1 de enero (RDL 14/2025) |

Escalas autonómicas de las 15 comunidades de régimen común vigentes en 2025, incluidas las deflactaciones de Madrid, Canarias, Aragón y Galicia. Tarifas forales de Navarra (13%–52%) y País Vasco (23%–49%).

**Fuentes:** Ley 35/2006 del IRPF y normativa autonómica de tributos cedidos, Orden de cotización a la Seguridad Social para 2025, Ley 5/2025, Real Decreto-ley 14/2025 e Índice de Precios de Consumo del INE.

## Limitaciones

- No contempla situación familiar, hijos, discapacidad, movilidad geográfica ni edad.
- No aplica deducciones autonómicas, aportaciones a planes de pensiones ni rentas del ahorro.
- Los resultados de Navarra y País Vasco son orientativos: sus sistemas forales incluyen deducciones que no están recogidas, y los tres territorios vascos tienen normativa propia entre sí.
- El mutualismo administrativo cotiza sobre haberes reguladores por grupo, no sobre el salario real; aquí se aproxima como un porcentaje del bruto.
- Es una herramienta divulgativa. No sustituye al asesoramiento fiscal profesional ni al simulador de la Agencia Tributaria.

## Estructura

```
index.html    Todo: marcado, estilos y lógica de cálculo
README.md
```

Un solo archivo, sin dependencias que instalar y sin paso de compilación. La única dependencia externa es Chart.js, cargado desde jsDelivr con versión fijada. Si falla la carga, la calculadora y la tabla siguen funcionando y los gráficos muestran un aviso.

No usa cookies, `localStorage` ni analítica: no hace falta banner de consentimiento.

## Publicar en GitHub Pages

1. Sube `index.html` a la raíz del repositorio.
2. Ve a **Settings → Pages**.
3. En **Source**, elige *Deploy from a branch*.
4. Selecciona la rama `main` y la carpeta `/ (root)`.
5. Guarda y espera un minuto a que se despliegue.

Funciona igual publicada en la raíz del dominio que en un subdirectorio, porque no hay ninguna ruta relativa a recursos locales.

## Ejecutar en local

Basta con abrir `index.html` en el navegador. Si prefieres servirlo:

```bash
python3 -m http.server 8000
```

## Actualizar a un ejercicio posterior

Casi todo está en el bloque `DATOS FISCALES` del principio del script:

- `ESCALA_ESTATAL`: la escala estatal, estable desde hace varios ejercicios.
- `TERRITORIOS`: las escalas autonómicas y forales. Es lo que más cambia cada año, porque varias comunidades deflactan sus tramos.
- `SS`: tipos, base máxima y tramos de la cuota de solidaridad, que suben cada año.
- `GASTOS_GENERALES`, `MINIMO_PERSONAL`, `SMI`, `SUBIDA_PACTADA`.
- Las funciones `reduccionTrabajo` y `deduccionRentasBajas` si cambian los umbrales.

Acuérdate de actualizar también los presets de IPC, el bloque «Datos del ejercicio» del HTML y el año del encabezado.

## Licencia

MIT. Los datos fiscales proceden de normativa pública.
