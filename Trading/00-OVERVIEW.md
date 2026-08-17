# Trading · 00-OVERVIEW

## ⚡ ESTADO DE SESIÓN
- **Última sesión:** 2026-08-17
- **Pendientes activos:**
  - (sesión inicial — definir junto a Dani)

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

## 🤖 EAs desarrollados

### Algoritmo SMTV1F (MQL4)
- EA propio, licenciado a **Financial Broker SA**
- Co-fundador: Dani

### Scherman Master System (MQL4/MQL5)
- Multi-estrategia: grid + híbrido
- Sistema complejo con múltiples modos

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
_(se irán agregando con /fin trading)_
