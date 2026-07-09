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

| Activo | Buy & Hold | Estrategia de cruces | Diferencia |
|---|---|---|---|
| AAPL | +155% | +55% | Buy & Hold gana por ~100 pp |
| TSLA | +290% | +70% | Buy & Hold gana por ~220 pp |

*(Nota: resultados sin descontar comisiones ni impuestos, lo que en la
práctica
