# 🌊 Shocks Asimétricos de El Niño Costero y Mercado Bursátil Peruano: Evidencia con Modelos NARDL y GARCH-X (2004–2026)

![Python](https://img.shields.io/badge/Python-3.10%2B-3776ab?style=flat-square&logo=python&logoColor=white)
![Stata](https://img.shields.io/badge/Stata-NARDL-1f5b8c?style=flat-square&logo=stata&logoColor=white)
![BCRP API](https://img.shields.io/badge/Datos-BCRP_API-003366?style=flat-square)
![IGP ENFEN](https://img.shields.io/badge/Clima-ICEN_IGP-e63946?style=flat-square)
![UNSA](https://img.shields.io/badge/UNSA-Econometría_II-2b2d42?style=flat-square)
![Metodología](https://img.shields.io/badge/Modelo-NARDL_%26_GARCH--X-008080?style=flat-square)

> **Trabajo de Investigación Formativa (TIF) — Econometría II**  
> **Universidad Nacional de San Agustín de Arequipa (UNSA)** — *Facultad de Economía*  
> **Área:** Macroeconometría Financiera, Cointegración Asimétrica y Finanzas del Clima (*Climate Finance*)  
> **Muestra Temporal:** Enero de 2004 – Mayo de 2026 ($T = 269$ observaciones mensuales continuas)  
> **Dataset Maestro:** [`Code/Py/data_bvl_icen.csv`](file:///c:/Users/atili/OneDrive%20-%20unsa.edu.pe/TIF%20Econometr%C3%ADa%20II/Code/Py/data_bvl_icen.csv) y `data_bvl_icen.dta`

---

## 📌 1. Motivación e Hipótesis de Asimetría

El Fenómeno El Niño Costero genera anomalías térmicas en la superficie marina frente a la costa del Perú con fuertes repercusiones sobre los sectores productivos reales (destrucción de infraestructura de transporte, pérdidas en la agricultura costera e impactos sobre la biomasa de anchoveta). En los mercados financieros, estos desastres climáticos alteran las expectativas de utilidades empresariales, elevan la aversión al riesgo y modifican los precios de los activos.

Sin embargo, los modelos lineales tradicionales asumen una respuesta simétrica: suponen erróneamente que un choque térmico cálido de $+2^\circ\text{C}$ (El Niño destructivo) ejerce un impacto de igual magnitud y signo contrario que una fase fría de $-2^\circ\text{C}$ (La Niña). 

Para superar esta limitación teórica y empírica, esta investigación adopta el enfoque **NARDL (*Non-linear Autoregressive Distributed Lag*)** propuesto por **Shin, Yu & Greenwood-Nimmo (2014)**, evaluando formalmente:
1. **Asimetría de Largo Plazo:** ¿Existe una relación de cointegración asimétrica entre el Índice Costero El Niño (ICEN) y el valor de la Bolsa de Valores de Lima (BVL General)?
2. **Asimetría de Corto Plazo:** ¿Reacciona el mercado bursátil con mayor intensidad o velocidad ante los shocks cálidos ($ICEN^+$) que ante los fríos ($ICEN^-$)?
3. **Canal de Expectativas:** ¿Cómo median las expectativas de inflación a 12 meses y la confianza empresarial a 3 meses la transmisión del shock climático?
4. **Impacto en Volatilidad (Incertidumbre):** Complementado con un modelo **GARCH(1,1)-X**, ¿elevan los episodios de calentamiento la varianza condicional de los retornos bursátiles?

---

## 📊 2. Base de Datos y Operacionalización (2004–2026)

El dataset [`data_bvl_icen.csv`](file:///c:/Users/atili/OneDrive%20-%20unsa.edu.pe/TIF%20Econometr%C3%ADa%20II/Code/Py/data_bvl_icen.csv) contiene **269 observaciones mensuales** sin datos faltantes, estructuradas en el siguiente orden lógico:

### Diccionario de Variables y Códigos Oficiales

| N° | Variable | Código Oficial | Fuente | Rol | Descripción Corta | Unidad / Transformación |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **1-4** | `date_str`, `year`, `month`, `stata_tm` | — | Calendario | Tiempo | Identificadores temporales | `2004-01` a `2026-05` (Stata `%tm`) |
| **5** | **`bvl_general`** | `PN01142MM` | BCRP | **Dependiente** | Índice General de la BVL (S&P/BVL) | Puntos de cierre mensual ($I(1)$) |
| **6** | **`ret_bvl_general`** | — | BCRP | **Dependiente** | Retorno logarítmico continuo | $100 \times \ln(P_t / P_{t-1})$ (%) ($I(0)$) |
| **7** | **`icen`** | — | IGP / ENFEN | **Explicativa** | Índice Costero El Niño | Media móvil 3m anomalía TSM (°C) ($I(0)$) |
| **8** | `icen_calido` | — | IGP / ENFEN | Asimétrica | Suma/shock positivo acumulado | $\max(ICEN_t, 0)$ (°C) |
| **9** | `icen_frio` | — | IGP / ENFEN | Asimétrica | Suma/shock negativo acumulado | $\min(ICEN_t, 0)$ (°C) |
| **10** | `dummy_nino` | — | IGP / ENFEN | Indicador | Evento Niño moderado/fuerte | 1 si $ICEN \ge 1.0$, 0 otros |
| **11** | **`exp_inflacion_12m`**| `PD12912AM` | BCRP | **Expectativa** | Expectativa de Inflación a 12 meses | % anual (encuesta BCRP) ($I(0)$) |
| **12** | `d_exp_inflacion_12m` | — | BCRP | Expectativa | Cambio en expectativa de inflación | Primera diferencia ($\Delta$ p.p.) |
| **13** | **`exp_emp_economia_3m`**| `PD38045AM` | BCRP | **Expectativa** | Confianza Empresarial Economía a 3m | Índice de difusión (>50 optimismo) ($I(0)$) |
| **14** | `d_exp_emp_economia_3m` | — | BCRP | Expectativa | Cambio en confianza empresarial | Primera diferencia ($\Delta$ puntos) |
| **15-16**| `tc_pen_usd`, `ret_tc_pen_usd` | `PN01234PM` | BCRP | Control Macro | Tipo de cambio interbancario y retorno | S/ por USD y variación logarítmica (%) |
| **17-18**| `tasa_ref`, `d_tasa_ref` | `PD04722MM` | BCRP | Control Macro | Tasa de referencia de política y $\Delta$ | % anual y primera diferencia ($\Delta$ p.p.) |
| **19-20**| `cobre_lme`, `ret_cobre_lme` | `PN01652XM` | BCRP | Control Macro | Cotización del Cobre LME y retorno | Centavos US$/lb y retorno logarítmico (%) |
| **21-22**| `petroleo_wti`, `ret_petroleo_wti` | `PN01660XM` | BCRP | Control Macro | Petróleo WTI y retorno | US$/barril y retorno logarítmico (%) |
| **23-24**| `pbi_indice`, `ret_pbi_indice` | `PN01770AM` | BCRP/INEI | Control Macro | Índice de Actividad Económica (PBI) | Base 2007=100 y crecimiento mensual (%) |
| **25-26**| `sp500`, `ret_sp500` | `^GSPC` | Yahoo Fin. | Control Global| Índice bursátil internacional S&P 500 | Puntos y retorno logarítmico (%) |
| **27** | `vix` | `^VIX` | Yahoo Fin. | Control Global| Índice de Volatilidad CBOE (VIX) | Puntos (aversión al riesgo global) |
| **28** | `dummy_crisis2008` | — | Externa | Quiebre | Gran Crisis Financiera Global | 1 de 2008m9 a 2009m6, 0 otros |
| **29** | `dummy_covid` | — | Externa | Quiebre | Shock pandémico COVID-19 | 1 de 2020m3 a 2020m12, 0 otros |

> **Justificación de la Muestra 2004–2026:** Los índices sectoriales del BCRP iniciaron recién en mayo de 2015. Al descartar los sectores y centrar el análisis en la **BVL General**, la muestra se extiende hasta **2004** (cuando inician formalmente la tasa de referencia y las expectativas en el BCRP), duplicando la potencia muestral ($T=269$) y cubriendo los eventos Niño de 2017 y 2023-2024, la crisis de 2008 y la pandemia.

---

## 📐 3. Especificación del Modelo NARDL

Siguiendo a **Shin et al. (2014)**, se descompone la variable explicativa climática en sumas parciales de variaciones positivas y negativas:

$$ICEN_t^+ = \sum_{j=1}^t \Delta ICEN_j^+ = \sum_{j=1}^t \max(\Delta ICEN_j, 0)$$

$$ICEN_t^- = \sum_{j=1}^t \Delta ICEN_j^- = \sum_{j=1}^t \min(\Delta ICEN_j, 0)$$

### 1. Ecuación de Cointegración Asimétrica de Largo Plazo:
$$\ln(BVL_t) = \beta_0 + \beta_1^+ ICEN_t^+ + \beta_1^- ICEN_t^- + \boldsymbol{\gamma}' \mathbf{X}_t + \varepsilon_t$$

Donde $\mathbf{X}_t$ es el vector de covariables de control (Expectativa de inflación, Confianza empresarial, Cobre, Tipo de cambio, Tasa de referencia y S&P 500).

### 2. Modelo Asimétrico de Corrección de Errores (NARDL-ECM):
$$\Delta \ln(BVL_t) = \rho \ln(BVL_{t-1}) + \theta^+ ICEN_{t-1}^+ + \theta^- ICEN_{t-1}^- + \boldsymbol{\vartheta}' \mathbf{X}_{t-1} + \sum_{i=1}^{p-1} \alpha_i \Delta \ln(BVL_{t-i}) + \sum_{j=0}^{q_1-1} \pi_j^+ \Delta ICEN_{t-j}^+ + \sum_{j=0}^{q_2-1} \pi_j^- \Delta ICEN_{t-j}^- + \sum_{k=0}^{r-1} \boldsymbol{\delta}_k' \Delta \mathbf{X}_{t-k} + \lambda_1 \text{Crisis}_{08} + \lambda_2 \text{COVID} + e_t$$

### 3. Pruebas de Hipótesis Cruciales del NARDL:
1. **Bounds Test de Cointegración Asimétrica (Pesaran et al., 2001; Shin et al., 2014):**
   $$H_0: \rho = \theta^+ = \theta^- = \boldsymbol{\vartheta} = 0 \quad \text{vs.} \quad H_1: \text{Existe cointegración}$$
   Se contrasta el estadístico $F_{PSS}$ contra los valores críticos superior $I(1)$ e inferior $I(0)$.
2. **Asimetría de Largo Plazo (Test de Wald):**
   $$H_0: -\frac{\theta^+}{\rho} = -\frac{\theta^-}{\rho} \quad \Longleftrightarrow \quad L^+ = L^-$$
   El rechazo de $H_0$ confirma que el impacto estructural de largo plazo de El Niño difiere del de La Niña.
3. **Asimetría de Corto Plazo (Test de Wald):**
   $$H_0: \sum_{j=0}^{q_1-1} \pi_j^+ = \sum_{j=0}^{q_2-1} \pi_j^-$$
4. **Multiplicadores Dinámicos Acumulados (*Dynamic Multipliers*):**
   Permiten trazar gráficamente las trayectorias de ajuste del índice bursátil tras un shock positivo unitario frente a un shock negativo unitario hasta alcanzar el nuevo equilibrio:
   $$m_h^+ = \sum_{j=0}^h \frac{\partial \ln(BVL_{t+j})}{\partial ICEN_t^+}, \qquad m_h^- = \sum_{j=0}^h \frac{\partial \ln(BVL_{t+j})}{\partial ICEN_t^-}, \qquad h \to \infty$$

---

## 📈 4. Modelo de Volatilidad Condicional: GARCH(1,1)-X

Para modelar la incertidumbre bursátil generada por los shocks climáticos:

$$\text{Ecuación de Media:} \quad ret\_bvl_t = \mu + \sum_{k=1}^m \phi_k ret\_bvl_{t-k} + \varepsilon_t, \quad \varepsilon_t = \sigma_t z_t, \quad z_t \sim \text{iid}(0,1)$$

$$\text{Ecuación de Varianza:} \quad \sigma_t^2 = \omega + \alpha_1 \varepsilon_{t-1}^2 + \beta_1 \sigma_{t-1}^2 + \gamma_1 |ICEN_t| + \gamma_2 ICEN\_calido_t + \delta_1 vix_t$$

Donde $\gamma_1 > 0$ evalúa si las perturbaciones climáticas incrementan la volatilidad del mercado peruano, y $\gamma_2 \neq 0$ captura asimetría directa en la varianza.

---

## 🔬 5. Diagnósticos y Validación Econométrica

1. **Orden de Integración:** Pruebas ADF y Phillips-Perron verifican que la combinación cumple el supuesto de NARDL: regresores $I(0)$ e $I(1)$, sin ninguna variable $I(2)$.
2. **Estabilidad Paramétrica:** Gráficos CUSUM y CUSUMQ (*Cumulative Sum of Recursive Residuals*) para confirmar la estabilidad de los multiplicadores dinámicos tras los shocks de 2008 y 2020.
3. **Diagnóstico de Residuos:**
   * Test de Breusch-Godfrey (ausencia de autocorrelación serial).
   * Test ARCH-LM (ausencia de heterocedasticidad autoregresiva residual).
   * Test de Ramsey RESET (adecuada especificación funcional).

---

## 🚀 6. Guía de Estimación y Replicación

### En Python (Extracción y Preparación):
Ejecutar el pipeline depurado en [`Code/Py/index.ipynb`](file:///c:/Users/atili/OneDrive%20-%20unsa.edu.pe/TIF%20Econometr%C3%ADa%20II/Code/Py/index.ipynb) para actualizar o consultar las series directamente desde las APIs oficiales.

### En Stata (Estimación del Modelo NARDL):
```stata
* 1. Instalar paquete oficial de NARDL
ssc install nardl

* 2. Cargar base de datos generada
use "Code/Py/data_bvl_icen.dta", clear

* 3. Declarar serie de tiempo mensual
tsset stata_tm, monthly

* 4. Generar logaritmos de variables I(1)
gen ln_bvl   = ln(bvl_general)
gen ln_tc    = ln(tc_pen_usd)
gen ln_cobre = ln(cobre_lme)
gen ln_sp500 = ln(sp500)

* 5. Estimación del modelo NARDL con ICEN
* p() rezagos de dependiente, q() rezagos de explicativas, pss calcula Bounds Test
nardl ln_bvl icen, p(2) q(2) pss

* 6. Estimación con covariables de control (Expectativas y Macro)
nardl ln_bvl icen exp_inflacion_12m exp_emp_economia_3m ln_tc ln_cobre ln_sp500, p(2) q(2) pss

* 7. Graficar los Multiplicadores Dinámicos Acumulados (Asimetría)
plotdynamic
```

---

## 📚 7. Referencias Bibliográficas Clave (APA 7)

* **Bollerslev, T.** (1986). Generalized autoregressive conditional heteroskedasticity. *Journal of Econometrics*, 31(3), 307–327. https://doi.org/10.1016/0304-4076(86)90063-1
* **Comité Multisectorial encargado del Estudio Nacional del Fenómeno El Niño [ENFEN].** (2024). *Definición operacional de los eventos El Niño Costero y La Niña Costera en el Perú* (Nota Técnica ENFEN 01-2024). IMARPE / IGP.
* **Pesaran, M. H., Shin, Y., & Smith, R. J.** (2001). Bounds testing approaches to the analysis of level relationships. *Journal of Applied Econometrics*, 16(3), 289–326. https://doi.org/10.1002/jae.616
* **Shin, Y., Yu, B., & Greenwood-Nimmo, M.** (2014). Modelling asymmetric cointegration and dynamic multipliers in a nonlinear ARDL framework. En *Festschrift in Honor of Peter Schmidt* (pp. 281–314). Springer, New York, NY. https://doi.org/10.1007/978-1-4899-8008-3_9
