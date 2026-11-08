# Control Integrado de Plagas Urbanas — Serviplagas

Capacitación web para técnicos de Serviplagas, personal administrativo y clientes.
Misma estructura que
[capacitacion-insectos-xilofagos](https://github.com/Servi-plagas/capacitacion-insectos-xilofagos):
un solo `index.html` sin dependencias de build, pensado para usarse desde el celular.

Contenido extraído de *Informe técnico de capacitación: Gestión Integral de Plagas Urbanas*
(Serviplagas S.A.S.), en español sencillo y sin nombres científicos.

## Cómo funciona

1. **9 módulos** con poco texto y frases cortas: qué es el control integrado, los tres recursos
   de toda plaga (alimento, agua, refugio), las plagas urbanas por grupos, inspección y puntos de
   control, métodos y seguridad, y responsabilidades de cada quien.
2. **Quiz de 8 preguntas** (una por módulo; la última cubre los módulos 8 y 9). Se aprueba con **6/8**.
   Antes de enviar, el técnico escribe su **nombre completo real**: se exigen al menos dos
   palabras, solo letras, y se convierte a formato título (`juan pablo pérez` → `Juan Pablo Pérez`).
   Las opciones se barajan en cada intento.
3. Al enviar el quiz se manda un correo automático a `informes.serviplagas@gmail.com` con el
   nombre, el puntaje y el detalle de cada respuesta (vía [FormSubmit](https://formsubmit.co),
   sin backend). La pantalla de resultados muestra si el envío salió bien y, si falla, ofrece un
   botón para reintentar.
4. Si aprueba, el técnico descarga un **certificado PDF** con logo, nombre, puntaje y fecha.
   Al generarlo se envía un segundo correo de confirmación. Si no aprueba, puede repasar los
   módulos y volver a presentar el quiz.

## Módulos e ilustraciones

Cada módulo abre con una **ilustración vectorial animada (SVG en línea)** que apoya el texto sin
repetirlo. Están escritas dentro de `index.html`, pesan pocos KB y funcionan sin conexión.

| Módulo | Qué muestra la animación |
|---|---|
| 1. ¿Qué es el control integrado? | «Solo fumigar»: la cucaracha vuelve; «control integrado»: rueda de 5 pasos que se encienden en orden |
| 2. Alimento, agua y refugio | Tres columnas sostienen el bloque «la plaga se queda»; al tacharlas una a una, la plaga se desvanece |
| 3. Cucarachas y hormigas | Cocina que pasa de día a noche y salen cucarachas; camino de hormigas al azúcar y cebo que vuelve al nido |
| 4. Moscas y mosquitos | Mosca que va de la caneca al plato y tapa que se cierra; larvas que suben a respirar y tapa que cubre el agua |
| 5. Roedores | Corte de un edificio: rata de tejados arriba, ratón en el mueble, rata noruega en la alcantarilla; señales que aparecen |
| 6. Pulgas, chinches y termitas | Pulga que salta de la mascota a la alfombra; chinche que sale de la costura del colchón; túnel de barro que sube por la pared |
| 7. Inspección y puntos de control | Plano con puntos de control que se encienden en orden de recorrido y semáforo de nivel de riesgo |
| 8. Métodos y seguridad | Cinco fichas de métodos; técnico que se pone el EPP pieza por pieza; señal de no reingreso con reloj |
| 9. Responsabilidades | Tres engranajes que giran juntos: empresa, cliente y usuarios |

Las plagas se dibujan una sola vez (`<symbol id="ic-roach">`, `ic-ant`, `ic-fly`, `ic-mosq`, `ic-rat`,
`ic-bug`, al principio del `<body>`) y se reutilizan con `<use>`. Las animaciones son CSS (clases
`anim-*`) y **se desactivan solas** si el dispositivo pide menos movimiento (`prefers-reduced-motion`).

`logo.png` viene del repositorio de xilófagos. El logo del certificado va **embebido en base64**
dentro de `index.html`, así que el PDF sale con logo incluso sin conexión.

## Publicar en GitHub Pages

> El documento fuente (`.pdf`) está en `.gitignore`: solo se publica el sitio.

```bash
git add index.html logo.png README.md .gitignore
git commit -m "Capacitación: Control Integrado de Plagas Urbanas"
git branch -M main
git remote add origin https://github.com/Servi-plagas/capacitacion-control-plagas-urbanas.git
git push -u origin main
```

Luego en GitHub: **Settings → Pages → Source: `main` / `root` → Save**.

La app queda en `https://servi-plagas.github.io/capacitacion-control-plagas-urbanas/`.

## Cambiar el correo de destino

Una sola línea en `index.html`:

```js
const NOTIFY_EMAIL = 'informes.serviplagas@gmail.com';
```

> Este correo ya está activado en FormSubmit por las otras capacitaciones.
> Si se cambia por uno nuevo, FormSubmit envía un correo de activación la
> primera vez que hay que confirmar.

## Ajustar el quiz

En `index.html`:

- `QUIZ_BANK` — las preguntas. `answer` es el índice de la opción correcta
  **antes** de barajar.
- `passingScore` — mínimo para aprobar (actualmente `6`).
- `totalQuestions` — debe coincidir con el número de preguntas.
