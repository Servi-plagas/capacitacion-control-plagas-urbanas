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
| 1. ¿Qué es el control integrado? | «Solo fumigar»: el rocío mata la cucaracha, pero al mes siguiente entra otra por la grieta (el calendario cambia de mes); «control integrado»: rueda de 5 pasos que se encienden en orden |
| 2. Alimento, agua y refugio | Tres columnas sostienen el bloque «la plaga se queda»; se tachan una a una y quedan tachadas; sin las tres, el bloque se hunde y aparece «sin plaga» |
| 3. Cucarachas y hormigas | Cocina que pasa de día a noche y solo de noche salen cucarachas; crías que van naciendo de la cápsula. Hormigas que siguen su camino por la grieta de la pared; una recoge el cebo y lo lleva al nido |
| 4. Moscas y mosquitos | Caneca destapada: la mosca va de la basura al plato y lo contamina; al bajar la tapa, el plato queda limpio. Agua destapada: el mosquito baja a poner huevos y aparecen larvas; con la tapa puesta, vuelve y no tiene dónde criar |
| 5. Roedores | Corte de un edificio: rata de tejados en el entretecho, ratón en el mueble, rata noruega en la alcantarilla; las señales (roeduras, excrementos, mancha de grasa) aparecen en orden y se quedan |
| 6. Pulgas, chinches y termitas | Pulga que salta de la mascota y cae en la alfombra, donde quedan huevos; chinche que sale de la costura, deja manchas y vuelve a esconderse; túnel de barro que sube hasta la viga, que queda hueca |
| 7. Inspección y puntos de control | Una lupa recorre el plano y se detiene en cada punto; los puntos revisados quedan marcados, aparecen hallazgos y el nivel de riesgo sube de bajo a moderado y a alto |
| 8. Métodos y seguridad | Cinco fichas de métodos; técnico que se pone el EPP pieza por pieza hasta quedar completo (con lista de chequeo); señal de no reingreso con reloj |
| 9. Responsabilidades | Tres engranajes que giran engranados: empresa, cliente y usuarios. Cuando uno se detiene, los otros dos también |

Las plagas se dibujan una sola vez (`<symbol id="ic-roach">`, `ic-ant`, `ic-fly`, `ic-mosq`, `ic-rat`,
`ic-bug`, al principio del `<body>`) y se reutilizan con `<use>`. Cada figura cuenta su historia en un
ciclo de 8 a 12 s que **empieza desde cero cada vez que se abre el módulo**. Los cambios de opacidad y
tamaño son CSS (clases `anim-*`); lo que se mueve por un camino (hormigas, mosca, mosquito, lupa) usa
`<animateMotion>` nativo de SVG. Si el dispositivo pide menos movimiento (`prefers-reduced-motion`),
las animaciones se detienen y cada figura muestra un solo estado coherente.

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
