# Backtest: Estrategia de Cruce de Medias Móviles (Golden Cross / Death Cross)

## Objetivo del proyecto
Evaluar con datos reales si una estrategia técnica clásica (cruce de media móvil
de 20 y 50 días) genera mejores retornos que simplemente comprar y mantener
("Buy & Hold") en dos activos con perfiles de volatilidad muy distintos.

## Metodología
- **Datos:** precios históricos diarios (2023-01-01 a 2026-07-07), obtenidos con
  la librería `yfinance`.
- **Activos analizados:** AAPL (Apple) y TSLA (Tesla), elegidos por tener
  volatilidades anualizadas muy distintas (25.7% vs 58.0%).
- **Señal de trading:** comprado cuando la media móvil de 20 días está por
  encima de la de 50 días; fuera del mercado cuando está por debajo.
- **Control de look-ahead bias:** la señal se aplica con un rezago de 1 día
  (`shift(1)`), ya que en la práctica solo se puede operar con la información
  del cierre del día anterior.
- **Benchmark:** comprar el activo el primer día y mantenerlo sin vender
  ("Buy & Hold"), sin ningún tipo de señal.

## Resultados

### Estrategia 1: Cruce de medias móviles (20/50 días)

| Activo | Buy & Hold | Estrategia de cruces | Diferencia |
|---|---|---|---|
| AAPL | +155% | +55% | Buy & Hold gana por ~100 pp |
| TSLA | +290% | +70% | Buy & Hold gana por ~220 pp |

### Estrategia 2: RSI (vender en sobrecompra > 70)

| Activo | Buy & Hold | Estrategia RSI | Diferencia |
|---|---|---|---|
| AAPL | +155% | ~+153% | Buy & Hold gana por ~2 pp (prácticamente empatados) |
| TSLA | +288.32% | +26.02% | Buy & Hold gana por ~262 pp |

*(Nota: resultados sin descontar comisiones ni impuestos, lo que en la
práctica empeoraría aún más el desempeño de ambas estrategias activas.)*

## Hallazgos principales
1. En las cuatro combinaciones probadas (2 estrategias × 2 activos), Buy & Hold
   fue igual o superior a la estrategia activa — ninguna estrategia técnica
   simple logró superar a simplemente comprar y mantener en este periodo.
2. **Cruce de medias:** perdió de forma consistente y significativa en ambos
   activos, penalizado por el "lag" (retraso) inherente a las medias móviles y
   por señales falsas en mercados laterales ("whipsaw").
3. **RSI:** el desempeño dependió fuertemente del tipo de activo. En AAPL
   (tendencia sostenida, menos explosiva) el RSI casi igualó a Buy & Hold. En
   TSLA (tendencia con subidas muy pronunciadas) el RSI perdió drásticamente,
   porque la regla "vender en sobrecompra" sacó a la estrategia del mercado
   justo durante los tramos de mayor ganancia — la sobrecompra no anticipó una
   corrección, sino que coincidió con fase de fortaleza (momentum).
4. **Conclusión general:** el supuesto detrás de un indicador (ej. "lo que
   sube mucho, corrige", en el caso del RSI) no se cumple igual en todos los
   activos. La efectividad de una estrategia técnica depende del régimen de
   mercado y del comportamiento específico del activo, no solo de la regla en
   sí misma — lo que refuerza la importancia de probar antes de asumir.
