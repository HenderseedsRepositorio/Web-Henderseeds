# Informe de Research — Plan Nutricional por Cultivo (Maíz, Pampa Arenosa)

> Sostiene los coeficientes y umbrales de `plan-nutricional.html`. Complementa a
> `FERTILIZANTES-INVESTIGACION.md` (no lo reemplaza): reusa N y P ya validados ahí,
> y suma la investigación nueva de **fósforo objetivo por cultivo** y **zinc**.
> Zona de calibración: Pampa Arenosa central — Henderson, Bolívar, Daireaux y
> partidos de influencia (General Villegas). Última actualización: 2026-08-31 (auditoría técnica: ver §2.1, §2.1.b, §2.3, §3.4.b).

---

## 0. Resumen de constantes usadas en el código

| Nutriente | Constante | Valor | Confianza | Fuente principal |
|---|---|---|---|---|
| Nitrógeno | `N_PER_TON` | 22 kg N/t rinde | ALTA | INTA / Fertilizar AC; Ciampitti & García (reusado) |
| Nitrógeno | `UREA_N_PCT` | 46% N | ALTA | Estándar industrial 46-0-0 (reusado) |
| Fósforo | `P_REQ_KG_PER_PPM` | 3,42 kg P elemental/ha por ppm | ALTA | Rubio et al. (FAUBA 2007/2008): 3,1-4,0 para Pampa Arenosa (ver §2.1) |
| Fósforo | `pFactor(f)` | derivado del grado | ALTA | `3,42 ÷ (P₂O₅% ÷ 2,291 ÷ 100)` — ya no hay factores a mano |
| Fósforo | `P_EXTRACT_P_PER_TON` | 2,6 kg P/t grano (= 5,96 kg P₂O₅/t) | ALTA | Ciampitti y García (2007), IPNI IA N°33 |
| Fósforo | P objetivo default | **18 ppm** (editable) | MEDIA (ver §2.2) | criterio de mantenimiento; umbral de respuesta zona 15-16 |
| Línea siembra | `LINEA_OK` / `LINEA_RIESGO` | 60 / 130 kg producto/ha | ALTA | Ciampitti et al. (2006) s/ Barraco 2002 + Fontanetto (ver §2.3) |
| Zinc | `ZN_UMBRAL` | 1 ppm Zn-DTPA | ALTA | Fertilizar AC (alta respuesta 0,9-1,3 ppm) |
| Zinc | `ZN_DOSIS_KG` | 1,5 kg Zn/ha | ALTA | Fertilizar / Profertil / INTA (rango 1-2) |
| Zinc | `ZN_SULFATO_PCT` | 22% Zn | ALTA | Sulfato de zinc heptahidratado ZnSO₄·7H₂O |

> `BOLSA_KG` se eliminó: la herramienta trabaja en kg/ha y no convierte a bolsas,
> así que el supuesto sin confirmar dejó de existir.

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

### 2.1. Factores kg-producto/ppm — AHORA DERIVADOS DEL GRADO (2026-08-31)

Antes había cinco números escritos a mano (MAP 15, DAP 17, NPS 34, SPT 17, SPS 39).
Se reemplazaron por un solo coeficiente + una fórmula, para que agregar un producto
no exija recalcular nada:

```
factor (kg producto/ppm) = 3,42 ÷ (P₂O₅% ÷ 2,291 ÷ 100)
```

**3,42 kg de P elemental/ha por ppm de P-Bray.** Contrastado esta vuelta contra
**Rubio et al. (FAUBA, 2007/2008)**, que es el modelo que responde exactamente esta
pregunta y contempla textura:

```
Dosis P (kg P/ha) = 0,1 × (Da (t/m³) × Prof (cm)) ÷ b
b = 0,45369 + 0,00356·P_Bray + 0,16245·Z − 0,00344·Arcilla     (Z: 1 Norte, 2 Sur)
```

Para la Pampa Arenosa (Da 1,2 t/m³, 0-20 cm, ~14% de arcilla, P-Bray 10) da
**3,1 kg P/ppm (Z=Sur) a 4,0 (Z=Norte)**. El 3,42 cae adentro → confianza **ALTA**.

**Distinción importante que antes no estaba escrita:** Rubio mide el incremento a los
45 días, o sea la corrección **para el cultivo de esta campaña**. Una construcción
*durable* del nivel de P pide bastante más: Berardo y col. (25 kg P/ha → +4,5 ppm el
año 1 = **5,6 kg P/ppm**) y la Red CREA Sur de Santa Fe (**5,8 kg P/ppm** el primer
año, subiendo a 25 kg al octavo). La página aclara cuál de los dos hace.

### 2.1.b. Catálogo de fuentes de P (ampliado 2026-08-31)

Grados verificados contra el catálogo argentino vigente (Bunge AR, Profertil, Mosaic AR):

| Clave | Producto | Grado | Factor kg/ppm | Aporta además |
|---|---|---|---|---|
| `map` | Monoamonio Fosfato | 11-52-0 | 15,1 | 11% N |
| `dap` | Diamonio Fosfato | 18-46-0 | 17,0 | 18% N |
| `spt` | Superfosfato Triple | 0-46-0 | 17,0 | ~14% Ca |
| `sps` | Superfosfato Simple | **0-21-0-12S** | 37,3 | 12% S, ~20% Ca |
| `mesz` | MicroEssentials SZ | 12-40-0-10S-1Zn | 19,6 | 12% N, 10% S, **1% Zn** |
| `mes9` | MicroEssentials S9 | 10-46-0-9S | 17,0 | 10% N, 9% S |
| `nps740` | Mezcla Startmix | 7-40-0-5S | 19,6 | 7% N, 5% S |
| `nps636` | Mezcla Startmix | 6-36-0-3S | 21,8 | 6% N, 3% S |
| `nps431` | Mezcla Startmix | 4-31-0-8S | 25,3 | 4% N, 8% S |
| `solfos` | SolFOS líquido | 11-37-0 | 21,2 | 11% N; ~1,39 kg/l |

**Correcciones respecto de v1:**
- El **SPS** figuraba como 0-20-0+11S. El grado comercial argentino es **0-21-0 con 12% S**
  (Profertil; Bunge "SPS Ramallo" 0-18/21-0 12S; COFCO). El factor pasa de 39 a 37,3.
- Se sacó el **NPS 23-23-0+7S**: es una mezcla construible (MAP + sulfato de amonio + urea)
  pero no se pudo verificar como grado comercial corriente en el país. Lo reemplazan las
  mezclas Startmix, que sí están en catálogo.

**Deliberadamente afuera:** **Microstar PZ** (Rizobacter) y los microgranulados en general.
Se usan a 30-40 kg/ha en la línea como arrancador, no como corrector de suelo — el modelo
de déficit en ppm les daría dosis sin sentido físico.

**Por qué entró MicroEssentials SZ:** Mosaic lo posiciona para suelos arenosos de pH alto
con Zn deficiente del norte de Buenos Aires, sur de Córdoba y Santa Fe — la misma
subregión. En 22 sitios/año en BA, SF y ER rindió **+862 kg/ha (+8,8%)** contra N+P.
Trae 1% de Zn: a 150 kg/ha aporta 1,5 kg Zn/ha, exactamente la dosis de corrección, así
que **la página descuenta ese aporte del sulfato de zinc** en vez de sumar los dos.

### 2.3. Fitotoxicidad en línea de siembra (investigación nueva 2026-08-31)

El fosforado en contacto con la semilla quema plántulas y la herramienta no decía nada,
mientras tiraba dosis de 120 a 312 kg/ha.

- **Ciampitti et al. (2006)**, sintetizando ensayos de **Barraco et al. (2002, INTA EEA
  General Villegas)** y Fontanetto: **60-80 kg/ha de fosfato diamónico** en la línea
  producen ~**20% de pérdida de plantas**; **130-170 kg/ha**, hasta **50%**.
- **Ferraris, Couretot y Magnone (Fertilizar, IAH 18, junio 2015)** — tres campañas con
  FMA (MAP) y MicroEssentials SZ a 50/100/150/200 kg/ha en la línea: con poca humedad a
  la siembra la población cayó ~1,4 pl/m² a 100 kg/ha y 3,9 pl/m² a 200 kg/ha, y el
  rendimiento hizo techo en 100 kg/ha. Con buena humedad no hubo diferencias.

Umbrales usados: **aviso desde 60 kg/ha** de producto comercial, **aviso fuerte desde 130**.
La referencia son ensayos con DAP, la fuente más agresiva; MAP y los superfosfatos son más
suaves, pero el criterio se mantiene por precaución. Confianza: **ALTA**.

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

**Decisión de producto (Alvaro, 2026-08-25; corregido contra el disco el 2026-08-31):**
el campo P objetivo viene precargado en **18 ppm** — criterio de mantenimiento, no piso de
suficiencia — y es **editable**. (Este doc decía 13 ppm; el código dice 18 desde hace
varias sesiones. Gana el disco.) El disclaimer
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

### 3.4.b. Descuento del Zn que trae el fosforado (2026-08-31)

Si el productor elige **MicroEssentials SZ (1% Zn)**, el propio fosforado aporta Zn y
recomendarle además el sulfato completo sería contarlo dos veces. La página calcula
`Zn aportado = kg de producto × 1%` y lo resta de los 1,5 kg Zn/ha de corrección; si lo
cubre entero, la fila de Zn pasa a "Cubierto por el MES SZ" y no recomienda sulfato.

**Nota de fuente alternativa:** el sulfato **monohidratado** (33-35% Zn) también se
consigue en el país y Pioneer Argentina lo toma como referencia (~35%). La página calcula
con el heptahidratado (22%) y muestra la equivalencia en monohidratado en el detalle.

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
6. Ferraris, Couretot y Magnone (Fertilizar AC, *Informaciones Agronómicas de Hispanoamérica*
   N°18, junio 2015) — *Fertilizantes en línea de siembra de maíz: efectos sobre la
   implantación y el rendimiento.* https://fertilizar.org.ar/wp-content/uploads/2015/06/5.pdf
7. García, F. (CREA Oeste, 5° Taller Ridzo, 2011) — *Manejo de la fertilización fosfatada.*
   Contiene el modelo de Rubio et al. (FAUBA) y los coeficientes Pi/P-Bray de la Red CREA
   Sur de Santa Fe. https://www.creaoeste.org.ar/wp-content/uploads/2015/01/5to-Taller-Ridzo-Fertilizacion-Fosforada-F-Garcia-2011.pdf
8. Bunge Argentina — catálogo de fertilizantes fosfatados (grados comerciales vigentes).
   https://www.bunge.ar/Agricultura/Fertilizantes/Fertilizantes-Fosfatados
9. Mosaic — *MicroEssentials SZ 12-40-0-10S-1Zn*, ficha técnica y resultados en maíz
   temprano en Argentina. https://es.microessentials.com/resource-library/microessentials-sz-y-sus-resultados-en-maiz-temprano-en-argentina/
10. Profertil — *Superfosfato Simple (SPS) 0-21-0 12S*, ficha de producto.
   https://www.profertil.com.ar/index.php/en/products/simple-superphosphate-sps
11. Disponibilidad de zinc y respuesta a la fertilización del maíz en el sur de Córdoba —
   Univ. Nac. de Río Cuarto. https://www.produccionvegetalunrc.org/images/fotos/181_DISPONIBILIDAD%20DE%20ZINC%20Y%20RESPUESTA%20A%20LA%20FERTILIZACION%20DEL%20MAIZ%20EN%20EL%20SUR%20DE%20CoRDOBA.pdf

## 5. Regla dura respetada

**Soja NUNCA en el selector de cultivo/nitrógeno** — fija su nitrógeno biológicamente,
casi no se fertiliza con N. v1 = Maíz activo; Girasol "próximamente" (deshabilitado).
