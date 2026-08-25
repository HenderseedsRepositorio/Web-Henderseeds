# Informe de Research — Plan Nutricional por Cultivo (Maíz, Pampa Arenosa)

> Sostiene los coeficientes y umbrales de `plan-nutricional.html`. Complementa a
> `FERTILIZANTES-INVESTIGACION.md` (no lo reemplaza): reusa N y P ya validados ahí,
> y suma la investigación nueva de **fósforo objetivo por cultivo** y **zinc**.
> Zona de calibración: Pampa Arenosa central — Henderson, Bolívar, Daireaux y
> partidos de influencia (General Villegas). Última actualización: 2026-08-25.

---

## 0. Resumen de constantes usadas en el código

| Nutriente | Constante | Valor | Confianza | Fuente principal |
|---|---|---|---|---|
| Nitrógeno | `N_PER_TON` | 22 kg N/t rinde | ALTA | INTA / Fertilizar AC; Ciampitti & García (reusado) |
| Nitrógeno | `UREA_N_PCT` | 46% N | ALTA | Estándar industrial 46-0-0 (reusado) |
| Fósforo | `P_FACTORS` (MAP 15 / DAP 17 / SPT 17 / SPS 39) | kg producto/ppm déficit | ALTA | García, Ciampitti, Rubio & Picone, CREA Oeste 2009 (reusado) |
| Fósforo | P objetivo default | 13 ppm (editable) | MEDIA (ver §2) | decisión de Alvaro; umbral de respuesta zona = 15-16 |
| Zinc | `ZN_UMBRAL` | 1 ppm Zn-DTPA | ALTA | Fertilizar AC (alta respuesta 0,9-1,3 ppm) |
| Zinc | `ZN_DOSIS_KG` | 1,5 kg Zn/ha | ALTA | Fertilizar / Profertil / INTA (rango 1-2) |
| Zinc | `ZN_SULFATO_PCT` | 22% Zn | ALTA | Sulfato de zinc heptahidratado ZnSO₄·7H₂O |
| Bolsas | `BOLSA_KG` | 50 kg | SUPUESTO | Sin confirmar por Alvaro (igual que fertilizantes) |

**Fuera de v1 (con dato, pero el dato juega en contra de incluirlos):**
- **Azufre:** en la Pampa Arenosa la respuesta de maíz a S es marginal (10% de sitios,
  +3,6% de rinde — Barraco et al., INTA Gral. Villegas 2016). No se incluye para no
  sugerir una palanca que en la zona casi no mueve la aguja.
- **Potasio:** Molisoles de la zona ricos en K; sin fuente citable que justifique
  respuesta en maíz. Afuera.

---

## 1. Nitrógeno — REUSADO (sin cambios)

Se reusa tal cual el requerimiento validado en `FERTILIZANTES-INVESTIGACION.md`:
**22 kg N por tonelada de rinde objetivo**, estándar fisiológico consolidado
(INTA, Fertilizar AC; Ciampitti & García; rango 20-25, 22 central). Urea 46% N.

Es el **requerimiento del cultivo, no la dosis final**: no descuenta la
mineralización del suelo ni las pérdidas por volatilización (15-40% si la urea
queda en superficie sin lluvia), ni ajusta por eficiencia de uso real (EUN
40-70%). El plan calcula `déficit = rinde×22 − N disponible` y `urea = déficit/0,46`.

**Confirmación zonal (nueva):** el ensayo de INTA General Villegas (Barraco, Díaz
Zorita, Álvarez, 2016) reporta para la Pampa Arenosa un umbral de **157 kg N/ha
disponible** (suelo + fertilizante) y respuesta significativa a N en el 87% de los
sitios (+23% de producción promedio) — consistente con el modelo de 22 kg N/t.
Confianza: **ALTA**.

---

## 2. Fósforo — objetivo por cultivo (investigación nueva) + factores reusados

### 2.1. Factores kg-producto/ppm — REUSADOS
MAP=15, DAP=17, SPT=17, SPS=39 kg de producto comercial por cada ppm de déficit de
P-Bray, calibrados para suelos franco-arenosos (Bray-1, 0-20 cm, Da 1,2 t/m³),
requerimiento ~3,42 kg P elemental/ha por ppm. Validados en el informe de
fertilizantes contra García/Ciampitti/Rubio/Picone (CREA Oeste 2009). Confianza: ALTA.

### 2.2. P objetivo sugerido por cultivo — LO QUE FALTABA
La calculadora de fertilizantes pide el "P objetivo" a mano porque en la sesión
previa no se había encontrado un ppm-objetivo citable para la Pampa Arenosa
(Berardo et al. 2003 era sudeste bonaerense, se descartó). **Esta vuelta sí apareció
la fuente de zona:**

- **Umbral crítico de respuesta del maíz: 15-16 ppm P-Bray.** García et al. 2007,
  **confirmado específicamente en la Pampa Arenosa por Barraco et al. (INTA General
  Villegas, 2016)** — General Villegas es la misma subregión que Henderson/Bolívar/
  Daireaux, no una extrapolación. En ese ensayo el 75% de los lotes estaba por
  debajo de 16,8 ppm y hubo respuesta a P en el 75% de los sitios (+17% de rinde,
  eficiencia 80 kg grano/kg P). Confianza sobre la banda: **ALTA**.
- **García/IPNI-Profertil 2005:** rango crítico 9-12 ppm (varía por textura) y
  recomienda mantener 20-30 ppm para producción óptima (criterio de reposición).

**Decisión de producto (Alvaro, 2026-08-25):** el campo P objetivo viene
precargado en **13 ppm** como piso de suficiencia, y es **editable**. El disclaimer
de la página aclara explícitamente que el umbral de *respuesta* en la zona es 15-16
ppm y el criterio de *reposición/mantenimiento* sube a 18-20 ppm, para que el
productor que trabaje con ese criterio lo suba a mano. Confianza del default como
número único: **MEDIA** (es una elección de criterio, no un valor forzado por una
única fuente — de ahí que quede editable y documentado).

---

## 3. Zinc — investigación nueva completa

No había nada construido para Zn. Se investigó de cero.

### 3.1. Método de extracción
El Zn se diagnostica por **DTPA** (no Bray-1, que es para P). El input de la página
va rotulado "Zn (DTPA), ppm". Método identificado como el más confiable para umbrales
de respuesta (Martens & Lindsay 1990; Sims & Johnson 1991).

### 3.2. Umbral de suficiencia
**~1 ppm Zn-DTPA.** Fertilizar AC reporta alta frecuencia de respuesta en maíz con
Zn-DTPA **por debajo de 0,9-1,3 ppm** (0-20 cm), con respuestas de rinde de 5-10%.
El clásico nivel crítico de Lindsay & Norvell (~1 ppm) coincide. En sitios sospechosos
de deficiencia de la región pampeana (centro de Córdoba, sur de Santa Fe, norte de
Buenos Aires, oeste de Entre Ríos) se midieron valores de 0,19-0,80 ppm. Confianza: **ALTA**.

### 3.3. Dosis de corrección
**1-2 kg Zn/ha (se usa 1,5)** como **sulfato de zinc**. Coinciden Fertilizar,
Profertil e INTA (aplicación de 1 a 1,5 kg/ha promedio; hasta 2 kg/ha como sulfato
cuando aparecen síntomas en plántulas). Confianza: **ALTA**.

### 3.4. Producto y conversión
- Sulfato de zinc **heptahidratado** (ZnSO₄·7H₂O) = **22% Zn** (y ~11% S).
- Sulfato de zinc **monohidratado** (ZnSO₄·H₂O) = **33-35% Zn**.
- La página usa el heptahidratado (22%): **1,5 kg Zn/ha ÷ 0,22 ≈ 6,8 kg/ha de producto**.

### 3.5. Lógica de cálculo (honesta)
El Zn **no** se calcula por déficit lineal de ppm como el P — la literatura no
sostiene una relación ppm→dosis lineal para Zn. Es **umbral + dosis fija de
corrección**: si Zn ≥ 1 ppm → "suficiente, no requiere"; si Zn < 1 ppm → recomendar
~1,5 kg Zn/ha. Mismo espíritu que la pestaña Microgranulados de fertilizantes.html.

---

## 4. Fuentes

1. García / IPNI-Profertil (2005) — *Criterios para el manejo de la fertilización del
   cultivo de maíz.* https://www.profertil.com.ar/wp-content/uploads/2020/08/nutricion-en-el-cultivo-de-maiz-ipni-f-garcia-2005.pdf
2. Barraco, Díaz Zorita, Álvarez (INTA EEA General Villegas / INTA Anguil, 2016) —
   *Contribución de la fertilización con nitrógeno, fósforo y azufre a la productividad
   de maíz en la Pampa Arenosa.* https://www.engormix.com/agricultura/fertilizacion-maiz/contribucion-fertilizacion-nitrogeno-fosforo_a32391/
3. Fertilizar AC — *Las claves de la fertilización en maíz.* https://fertilizar.org.ar/las-claves-de-la-fertilizacion-en-maiz/
4. Profertil — *La importancia del zinc en nuestros cultivos* (Boletín Técnico N°21, 2015).
   https://www.profertil.com.ar/wp-content/uploads/2020/08/bt-n-21-la-importancia-del-zinc-en-nuestros-cultivos-2015.pdf
5. García, Ciampitti, Rubio & Picone (CREA Oeste, 2009) — *Fósforo en cultivos extensivos*
   (factores P, reusados de FERTILIZANTES-INVESTIGACION.md).
   https://www.creaoeste.org.ar/wp-content/uploads/2015/02/Garcia-Ciampitti-Rubio-Picone-Fosforo-2009.pdf
6. Disponibilidad de zinc y respuesta a la fertilización del maíz en el sur de Córdoba —
   Univ. Nac. de Río Cuarto. https://www.produccionvegetalunrc.org/images/fotos/181_DISPONIBILIDAD%20DE%20ZINC%20Y%20RESPUESTA%20A%20LA%20FERTILIZACION%20DEL%20MAIZ%20EN%20EL%20SUR%20DE%20CoRDOBA.pdf

## 5. Regla dura respetada

**Soja NUNCA en el selector de cultivo/nitrógeno** — fija su nitrógeno biológicamente,
casi no se fertiliza con N. v1 = Maíz activo; Girasol "próximamente" (deshabilitado).
