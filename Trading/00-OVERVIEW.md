# Trading · 00-OVERVIEW

## ⚡ ESTADO DE SESIÓN
- **Última sesión:** 2026-08-17
- **Hecho:**
  - Creado sistema `/inicio trading` / `/fin trading` con vault en `Trading/00-OVERVIEW.md`
  - Desarrollado indicador MT5 `StochCross` (Stoch 14,3,3, cálculo sobre cierre de vela)
    - `StochCross_Candles.mq5` — colorea velas en chart principal (dorado alcista, magenta bajista)
    - `StochCross_Sub.mq5` — subventana con K/D + dots de señal
    - Condiciones: cruce K/D + zona (<20 alcista, >80 bajista) + tipo de vela coincidente
    - Inputs configurables: períodos, niveles, colores
- **Pendientes activos:** ninguno

---

## 🧠 Contexto general

**Trader:** Victor Danieri (Dani)
**Base:** Asunción, Paraguay
**Experiencia:** 9+ años
**Instrumento principal:** NAS100
**Timeframes:** H1 / M30 (análisis); entradas en M5/M15
**Broker time:** UTC+2 → Paraguay UTC-4 (offset -6h)

---

## 📐 Sistema técnico

### Indicador core
- **LWMA sobre Typical Price (HLC/3)** como referencia de tendencia y estructura

### Stop Loss
- **ATR(14)** para sizing dinámico de SL

### Risk/Reward
- Mínimo **1:2 R/R** por operación

### Entradas
- Wick sweeps como señal de entrada
- Buffer distances configurables por timeframe

---

## 🤖 EAs e indicadores desarrollados

### Algoritmo SMTV1F (MQL4)
- EA propio, licenciado a **Financial Broker SA**
- Co-fundador: Dani

### Scherman Master System (MQL4/MQL5)
- Multi-estrategia: grid + híbrido
- Sistema complejo con múltiples modos

### StochCross (MQL5) — 2026-08-17
- Indicador de cruce del Stochastic(14,3,3) sobre cierre de vela
- Dos archivos: `StochCross_Candles.mq5` (chart) + `StochCross_Sub.mq5` (subventana)
- Señal alcista: K cruza D hacia arriba + K<20 + vela alcista → vela dorada + dot verde
- Señal bajista: K cruza D hacia abajo + K>80 + vela bajista → vela magenta + dot rojo
- Todos los parámetros configurables via inputs

### Backtesting framework (Python)
- NAS100, WMA en Typical Price
- Wick sweep entries, multi-timeframe M3–H1
- Configurable buffer distances

### MT5 Account Auditor Dashboard
- HTML/JS, parsea exports HTML de MT5
- Ajuste de timezone: broker UTC+2 → PY UTC-4

### NinjaTrader 8 Trade Copier (C#)
- Reescrito para corregir API methods, order action logic, threading

---

## 🖥️ Plataformas

- **MT4 / MT5** — desarrollo principal de EAs
- **NinjaTrader 8** — futuros + forex, post-MT4
- **Financial Broker SA** — empresa co-fundada, usa los EAs

---

## 📁 Vault path
`Trading/` en `victordanieri/victor-projects-vault`

---

## 📝 Notas de sesiones anteriores

### 2026-08-17
- Primera sesión. Setup del vault de trading.
- Creado indicador StochCross para MT5 (ver sección EAs).
