# 🌊 Shocks de El Niño Costero y Mercado Bursátil Peruano: Evidencia Econométrica sobre Retornos, Volatilidad Condicional y Heterogeneidad Sectorial en la BVL (2015–2026)

![Python](https://img.shields.io/badge/Python-3.10%2B-3776ab?style=flat-square&logo=python&logoColor=white)
![Stata](https://img.shields.io/badge/Stata-16.0%2B-1f5b8c?style=flat-square&logo=stata&logoColor=white)
![BCRP API](https://img.shields.io/badge/Datos-BCRP_API-003366?style=flat-square)
![IGP ENFEN](https://img.shields.io/badge/Clima-ICEN_IGP-e63946?style=flat-square)
![UNSA](https://img.shields.io/badge/UNSA-Econometría_II-2b2d42?style=flat-square)
![Series de Tiempo](https://img.shields.io/badge/Método-Local_Projections_%26_GARCH--X-008080?style=flat-square)

> **Trabajo de Investigación Formativa (TIF) — Econometría II**  
> **Universidad Nacional de San Agustín de Arequipa (UNSA)** — *Facultad de Economía*  
> **Área:** Macroeconometría Financiera y Riesgo Climático (*Climate Finance*)  
> **Período Muestral:** Enero de 2015 – Mayo de 2026 ($T = 137$ observaciones mensuales)

---

## 📌 1. Resumen y Pregunta de Investigación

El Fenómeno El Niño Costero representa uno de los eventos hidrometeorológicos y climáticos de mayor disrupción física y macroeconómica en el Perú, generando daños severos en infraestructura vial, agricultura y pesquería. A pesar de su recurrencia (notablemente en los eventos extraordinarios de 2017 y 2023), la literatura financiera emergente ha prestado escasa atención al mecanismo de transmisión hacia el mercado de capitales doméstico mediante indicadores climáticos locales y desagregación sectorial.

Este estudio analiza empíricamente la **respuesta dinámica de los retornos accionarios y la volatilidad condicional de la Bolsa de Valores de Lima (BVL)** ante los shocks exógenos del **Índice Costero El Niño (ICEN)**, evaluando la existencia de **heterogeneidad sectorial** (Financiero, Industrial, Minería y Servicios Públicos) e incorporando el canal mediador de las **expectativas macroeconómicas y empresariales**.

### Pregunta General de Investigación:
> *¿Los shocks del Índice Costero El Niño (ICEN) generan una respuesta dinámica y estadísticamente identificable en los retornos y la volatilidad condicional del mercado bursátil peruano, y es dicha respuesta heterogénea entre los sectores económicos de la Bolsa de Valores de Lima durante el período 2015–2026?*

### Preguntas Específicas:
1. **Dinámica e Impulso-Respuesta:** ¿Existe una respuesta estadísticamente significativa en los retornos del índice general (S&P/BVL Peru General) ante innovaciones del ICEN y en qué horizonte temporal ($h$ meses) se disipa?
2. **Volatilidad e Incertidumbre:** ¿Aumentan las anomalías térmicas del mar la volatilidad condicional del mercado bursátil, y existe asimetría entre shocks cálidos ($ICEN > 0$) y fases frías ($ICEN < 0$)?
3. **Heterogeneidad Sectorial:** ¿Difiere la magnitud y signo de la respuesta entre sectores con exposición física directa (Industria, Servicios Públicos) frente a sectores con fijación global de precios (Minería) o intermediación financiera?
4. **Canal de Transmisión de Expectativas:** ¿Actúan las expectativas de crecimiento del PBI y la confianza empresarial del BCRP como eslabón transmisor del shock climático?
5. **Robustez a Quiebres:** ¿Se mantiene la relación estimada tras controlar por el quiebre estructural exógeno de la pandemia COVID-19 (2020)?

---

## 🔄 2. Mecanismo Teórico de Transmisión

Siguiendo el marco conceptual desarrollado en el documento metodológico (*Book/El Niño Costero y la BVL.pdf*), la cadena causal postulada se estructura de la siguiente manera:

```text
       ┌────────────────────────────────────────────────────────┐
       │   Shock Climático Costero: Anomalía TSM Niño 1+2       │
       │           (Índice Costero El Niño - ICEN)              │
       └───────────────────────────┬────────────────────────────┘
                                   │
                                   ▼
       ┌────────────────────────────────────────────────────────┐
       │      Impactos en Actividad Real y Oferta Sectorial     │
       │    (Daños en transporte/vías, caída pesca y agro)      │
       └───────────────────────────┬────────────────────────────┘
                                   │
                                   ▼
       ┌────────────────────────────────────────────────────────┐
       │       Canal de Expectativas y Sentimiento de Mercado   │
       │ (Encuesta BCRP: Caída en confianza empresarial y PBI)  │
       └───────────────────────────┬────────────────────────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    ▼                             ▼
       ┌────────────────────────┐    ┌────────────────────────┐
       │     Flujos Futuros     │    │   Incertidumbre /      │
       │   y Tasa de Descuento  │    │   Dispersión de Pronóstico │
       └────────────┬───────────┘    └────────────┬───────────┘
                    │                             │
                    ▼                             ▼
       ┌────────────────────────┐    ┌────────────────────────┐
       │  Retornos Bursátiles   │    │  Volatilidad           │
       │  BVL (Nivel)           │    │  Condicional (GARCH)   │
       └────────────────────────┘    └────────────────────────┘
```

---

## 📊 3. Base de Datos y Operacionalización de Variables

El dataset maestro (`datos_bvl_icen_master.csv` y `.dta`) comprende **137 observaciones mensuales continuas** (Enero 2015 – Mayo 2026), combinando tres fuentes oficiales:

### A. Variable Explicativa Climática (Exógena) — IGP / ENFEN
* **ICEN (Índice Costero El Niño):** Medido en °C como la media móvil trimestral de las anomalías de la Temperatura Superficial del Mar en la región Niño 1+2 frente al litoral peruano.
* **Fuente:** Repositorio oficial del Instituto Geofísico del Perú (`http://met.igp.gob.pe/datos/ICEN.txt`).
* **Variables derivadas:**
  - $ICEN\_calido_t = \max(ICEN_t, 0)$: Captura choques de calentamiento (fase El Niño).
  - $ICEN\_frio_t = \min(ICEN_t, 0)$: Captura fases frías (La Niña).
  - $Dummy\_Nino_t = \mathbb{I}(ICEN_t \ge 1.0)$: Umbral oficial ENFEN para eventos de magnitud moderada/fuerte.

### B. Variables Bursátiles (Bolsa de Valores de Lima - BVL) — API BCRP
* **Índice General BVL (`PN01142MM`):** S&P/BVL Peru General.
* **Índice Selectivo BVL (`PN01143MM`):** S&P/BVL Peru Select.
* **Índices Sectoriales BVL:**
  - *BVL Financiero* (`PN01148MM`): Sector bancario y seguros.
  - *BVL Industrial* (`PN01149MM`): Manufactura y bienes intermedios.
  - *BVL Minería* (`PN01150MM`): Extracción metálica (control "placebo" con precios internacionales).
  - *BVL Servicios Públicos* (`PN01151MM`): Electricidad, infraestructura y servicios regulados.
* **Transformación:** Retornos logarítmicos continuos: $r_{i,t} = 100 \times \ln(P_{i,t} / P_{i,t-1})$.

### C. Variables Macroeconómicas y Precios de Commodities — API BCRP
* **Tipo de Cambio PEN/USD (`PN01234PM`):** Promedio mensual interbancario ($r_{TC} = 100 \times \Delta \ln TC_t$).
* **Tasa de Referencia de Política Monetaria (`PD04722MM`):** Tasa del BCRP en primera diferencia ($\Delta i_t = i_t - i_{t-1}$).
* **Cobre LME (`PN01652XM`):** Cotización mensual internacional en ¢US$/lb ($r_{Cu} = 100 \times \Delta \ln Cu_t$).
* **Petróleo WTI (`PN01660XM`):** Cotización mensual en US$/barril ($r_{WTI} = 100 \times \Delta \ln WTI_t$).
* **Oro LME (`PN01654XM`):** Activo refugio en US$/oz troy ($r_{Au} = 100 \times \Delta \ln Au_t$).
* **Actividad Económica / PBI Mensual (`PN01770AM`):** Índice mensual de producción desestacionalizado ($2007=100$).

### D. Canal de Expectativas Macroeconómicas y Empresariales — API BCRP
* **Expectativa de Inflación a 12 meses (`PD12912AM`):** Encuesta mensual BCRP (nivel y $\Delta$).
* **Expectativa de Crecimiento del PBI a 12 meses (`PD38048AM`):** Crecimiento esperado a un año (nivel y $\Delta$).
* **Expectativa de Tipo de Cambio a 12 meses (`PD38049AM`):** Presión cambiaria proyectada (nivel y $\Delta$).
* **Expectativa de la Economía a 3 meses (`PD38045AM`):** **Índice de Confianza Empresarial** (>50 optimismo, <50 pesimismo).
* **Expectativa del Sector a 3 meses (`PD38046AM`):** Confianza empresarial sectorial a corto plazo.

### E. Mercados Financieros Globales — Yahoo Finance
* **S&P 500 (`^GSPC`):** Co-movimiento global de renta variable ($r_{SP500} = 100 \times \Delta \ln SP500_t$).
* **VIX Index (`^VIX`):** Proxy de incertidumbre y aversión al riesgo internacional (*fear gauge*).

---

## 📐 4. Estrategia Econométrica

Dado el tamaño de muestra mensual ($T \approx 137$), la investigación evita modelos sobrerestringidos y adopta el diseño metodológico recomendado en la auditoría del proyecto:

### 1. Modelo Principal de Retornos: Proyecciones Locales (Jordà, 2005)
Para cada sector y para el índice agregado, estimamos la respuesta dinámica al horizonte $h \in \{0, 1, 2, \dots, 12\}$ meses:

$$r_{i, t+h} = \alpha^{(h)} + \beta^{(h)} ICEN_{t} + \sum_{p=1}^{P} \boldsymbol{\Gamma}_{p}^{(h)} \mathbf{X}_{t-p} + \sum_{k=1}^{K} \phi_{k}^{(h)} r_{i, t-k} + \delta^{(h)} \text{COVID}_{t} + \varepsilon_{t+h}^{(h)}$$

* **Ventajas frente a VAR:** Robusto a errores de especificación dinámica, permite incorporar efectos acumulativos no lineales y no impone restricciones autoregresivas recursivas en muestras intermedias.
* **Inferencia:** Errores estándar robustos a autocorrelación y heterocedasticidad mediante corrección Newey-West con ancho de banda $L = h + 1$.

### 2. Modelado de la Volatilidad Condicional: GARCH-X Univariado
Para evaluar el impacto de la incertidumbre climática sobre la dispersión de retornos:

$$\text{Ecuación de Media:} \quad r_{t} = \mu + \sum_{j=1}^{p} \rho_j r_{t-j} + \varepsilon_t, \quad \varepsilon_t = \sigma_t z_t, \quad z_t \sim \text{iid}(0,1)$$

$$\text{Ecuación de Varianza (GARCH-X):} \quad \sigma_t^2 = \omega + \alpha \varepsilon_{t-1}^2 + \beta \sigma_{t-1}^2 + \gamma_1 |ICEN_t| + \gamma_2 ICEN\_calido_t$$

* Permite testear formalmente si $\gamma_1 > 0$ (el shock climático eleva la volatilidad) y si $\gamma_2 \neq 0$ (asimetría entre fases cálidas y neutras).

### 3. Contraste de Heterogeneidad Sectorial
Estimación mediante un sistema de regresiones aparentemente no relacionadas (**SUR - Seemingly Unrelated Regressions**) para contrastar formalmente la hipótesis nula de coeficientes homogéneos entre sectores:

$$H_0: \beta_{\text{Financiero}}^{(h)} = \beta_{\text{Industrial}}^{(h)} = \beta_{\text{Minería}}^{(h)} = \beta_{\text{Servicios}}^{(h)}$$

### 4. Pruebas de Estacionariedad y Quiebres Estructurales
* Pruebas de raíz unitaria: Dickey-Fuller Aumentado (ADF) y Phillips-Perron (PP) sobre niveles y retornos logarítmicos.
* Prueba de quiebre endógeno de **Zivot y Andrews (1992)** para validar la estabilidad paramétrica frente a las anomalías de 2017 (Niño Costero) y 2020 (COVID-19).

---

## 🗂️ 5. Estructura del Repositorio

```text
📦 TIF Econometría II/
 ├── 📂 Book/
 │    └── 📄 El Niño Costero y la BVL.pdf        # Guía metodológica, auditoría y diseño integral del TIF
 ├── 📂 Code/
 │    ├── 📂 Py/
 │    │    ├── 📄 index.ipynb                     # Notebook de extracción BCRP, ICEN, Yahoo Finance y transformaciones
 │    │    ├── 📄 datos_bvl_icen_master.csv       # Base de datos consolidada (137 obs × 46 variables)
 │    │    └── 📄 datos_bvl_icen_master.dta       # Base en formato Stata (tsset con %tm)
 │    ├── 📂 Paper/                              # Código fuente en LaTeX del artículo científico
 │    │    ├── 📄 paper.tex                       # Manuscrito estructurado en formato journal
 │    │    └── 📄 paper.pdf                       # Documento compilado final
 │    ├── 📂 Beamer/                             # Presentación académica para la sustentación
 │    ├── 📄 .gitignore                          # Exclusión de binarios y temporales
 │    └── 📄 README.md                           # Documentación técnica del proyecto
 ├── 📂 Antecedentes/                            # Literatura y artículos indexados de referencia
 └── 📂 Base/                                    # Respaldos de series crudas descargadas
```

---

## 🚀 6. Guía de Replicación Rápida

### Requisitos Previos
* **Python 3.10+**: `pandas`, `numpy`, `requests`, `yfinance`, `matplotlib`.
* **Stata 16+**: Paquetes recomendados: `jorda` o comandos de proyecciones locales (`lpirfs`), `arch`, `zandrews`.

### Paso 1: Extracción y Consolidación de Datos en Python
Ejecutar las celdas del notebook [Code/Py/index.ipynb](file:///c:/Users/atili/OneDrive%20-%20unsa.edu.pe/TIF%20Econometr%C3%ADa%20II/Code/Py/index.ipynb) o correr el pipeline por consola:

```bash
cd "Code/Py"
python -c "
import os; os.system('python ../../scratch/run_full_pipeline.py')
"
```
Esto consultará en tiempo real la API del BCRP, el servidor del IGP y Yahoo Finance, generando los archivos `datos_bvl_icen_master.csv` y `datos_bvl_icen_master.dta`.

### Paso 2: Declaración Temporal y Estimaciones en Stata
En Stata, importar la base generada y configurar la estructura de serie temporal mensual:

```stata
* Cargar base procesada
use "Code/Py/datos_bvl_icen_master.dta", clear

* Declarar estructura de series de tiempo
tsset stata_tm, monthly

* Inspección gráfica de retornos e ICEN
tsline ret_bvl_general icen, title("Retornos BVL e ICEN (2015-2026)")

* Estimación de Proyección Local al horizonte h=1 con controles
newey F1.ret_bvl_general icen L(1/2).ret_bvl_general L(1/2).ret_cobre_lme L(1/2).d_tasa_ref dummy_covid, lag(2)

* Modelo de Volatilidad GARCH(1,1)-X
arch ret_bvl_general L1.ret_bvl_general, arch(1) garch(1) het(icen_calido icen_frio)
```

---

## 📚 7. Referencias Bibliográficas Clave (APA 7)

* **Banco Central de Reserva del Perú [BCRP].** (2023). *El Fenómeno El Niño y su impacto en la economía peruana* [Recuadro 1]. En *Reporte de Inflación: Panorama actual y proyecciones macroeconómicas 2023–2024* (junio). BCRP.
* **Banco Central de Reserva del Perú [BCRP].** (2026). *Impacto de El Niño Costero 2026 en la actividad económica* [Recuadro 2]. En *Reporte de Inflación* (marzo). BCRP.
* **Bollerslev, T.** (1986). Generalized autoregressive conditional heteroskedasticity. *Journal of Econometrics*, 31(3), 307–327. https://doi.org/10.1016/0304-4076(86)90063-1
* **Comité Multisectorial encargado del Estudio Nacional del Fenómeno El Niño [ENFEN].** (2024). *Definición operacional de los eventos El Niño Costero y La Niña Costera en el Perú* (Nota Técnica ENFEN 01-2024). IMARPE / IGP.
* **Jordà, Ò.** (2005). Estimation and inference of impulse responses by local projections. *American Economic Review*, 95(1), 161–182. https://doi.org/10.1257/0002828053854583
* **Zivot, E., & Andrews, D. W. K.** (1992). Further evidence on the great crash, the oil-price shock, and the unit-root hypothesis. *Journal of Business & Economic Statistics*, 10(3), 251–270. https://doi.org/10.1080/07350015.1992.10509904
