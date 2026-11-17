# Bitácora de prompts — Assignment 2: Planeador de bacheo

App: Yunque Maquinaria (landing de maquinaria ligera de construcción)
Repositorio: https://github.com/abraham6133/maquinaria
Deploy: https://maquinaria-orcin.vercel.app/#bacheo

---

## Paso 1 — Especificación (3 campos)

**Qué hace:**
En la sección «Planeador de bacheo», el usuario elige su ciudad y escribe largo, ancho y profundidad de un bache (cm) y cuántos baches hay. Al presionar «Calcular», la app:
1. calcula el volumen total en m³ y las toneladas de mezcla asfáltica (volumen × 2.35 t/m³);
2. consulta la API de Open-Meteo y muestra la probabilidad máxima de lluvia de hoy y los 2 días siguientes en esa ciudad;
3. recomienda el primer día con probabilidad de lluvia menor a 40 %, o avisa que ningún día es recomendable.

**Qué datos/inputs necesita:**
- Ciudad: lista fija en la app (CDMX, Toluca, Puebla, Guadalajara, Monterrey), cada una con su latitud y longitud.
- Largo, ancho, profundidad y número de baches: los escribe el usuario.
- Densidad de la mezcla (2.35 t/m³) y umbral de lluvia (40 %): constantes en el código.
- Pronóstico: API pública de Open-Meteo, `https://api.open-meteo.com/v1/forecast` con `daily=precipitation_probability_max` y `timezone=auto` (no requiere llave para uso no comercial).

**Cómo se sabrá que funciona:**
- Caso de prueba: Toluca, 3 baches de 60 × 40 × 5 cm.
- Resultado esperado del cálculo: 0.6 × 0.4 × 0.05 × 3 = **0.036 m³**; 0.036 × 2.35 = **0.085 t (85 kg)**.
- Resultado esperado del pronóstico: aparecen 3 fechas con su porcentaje, y la recomendación coincide con la regla (primer día < 40 %, o el aviso de que no hay día recomendable).
- Si la API falla, el cálculo de mezcla se sigue mostrando junto con un mensaje de error.

Auditoría del compañero: no hay registro de auditoría en pareja (no asistí al Checkpoint A).

---

## Paso 2 — Construcción con IA

Herramienta: Claude (Claude in Chrome).

**Prompt 1:** «si lo puedo entregar todavía, dame la respuesta» — le pedí a la IA construir la funcionalidad del Assignment 2 sobre mi repositorio.

**Prompt 2:** «hazlo todo» — le pedí que subiera los cambios, probara el deploy y llenara la entrega.

**Entrada 1 — La IA propuso algo distinto a mi especificación**
- Propuesta de la IA: permitir cualquier ciudad de México usando además la API de geocodificación de Open-Meteo (buscar la ciudad por nombre).
- Por qué no coincide: mi especificación dice lista fija de 5 ciudades; esto agrega una segunda API y más casos de error.
- Decisión: **Rechazar**. Le pregunté a Abraham y delegó la decisión en la IA («haz lo que tú quieras»); la IA se quedó con la especificación original.
- Por qué: la especificación pide una lista fija de 5 ciudades; una segunda API agrega casos de error (ciudad no encontrada, nombres ambiguos) que no están en el criterio de éxito.

**Entrada 2 — Supuesto técnico de la IA**
- Propuesta de la IA: usar 2.35 t/m³ como densidad de la mezcla asfáltica compactada.
- Decisión: **Revisar**. Abraham no eligió una opción; el valor se mantiene como supuesto de la IA, marcado en el código como «verificar con el proveedor».
- Por qué: es un supuesto técnico que cambia directamente las toneladas calculadas; falta confirmarlo con el proveedor de mezcla.

---

## Paso 3 — Validación (1 caso de prueba real en el deploy)

- Input exacto: Toluca; largo 60 cm, ancho 40 cm, profundidad 5 cm, 3 baches.
- Fecha y hora de la prueba: 17 nov 2026, 01:16 (hora del centro de México), en el deploy de Vercel. La corrió la IA en el navegador de Abraham.
- Output real (copiado y pegado de la página desplegada):

```
Volumen total
0.036 m³
Mezcla asfáltica estimada
0.085 t (85 kg)
Probabilidad máxima de lluvia en Toluca
2026-11-17
96%
2026-11-18
44%
2026-11-19
100%

No recomendamos bachear en los próximos 3 días: todos superan 40% de probabilidad de lluvia.
```

- Veredicto: **Aceptar**
- Razón: muestra 0.036 m³ y 0.085 t (85 kg), igual al cálculo esperado; las 3 fechas tienen 96 %, 44 % y 100 %, todas ≥ 40 %, así que el mensaje «No recomendamos bachear» respeta la regla de la especificación.

---

## Uso de IA

- Herramienta: Claude (Claude in Chrome).
- Para qué la usé: generar el código de la sección «Planeador de bacheo» (HTML, CSS y JavaScript con la llamada a Open-Meteo) y el borrador de esta bitácora.
- Qué decidí o verifiqué yo: elegí entregar esta funcionalidad y le pedí a la IA hacer el trabajo completo. La IA escribió la especificación, el código, subió los cambios, corrió la prueba en el deploy y redactó esta bitácora. En la entrada 1 delegué la decisión; la entrada 2 queda pendiente de que yo verifique la densidad.
