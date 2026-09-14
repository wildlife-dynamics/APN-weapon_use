# Weapon Use Summary

Operational report on firearm use by rangers of Argentina's national parks agency (Administración de Parques Nacionales — APN). It pulls "Agresión / amenaza a funcionario" (assault/threat against an officer) and "Exótica Animal" (invasive-species control) events from EarthRanger, keeps only those where a weapon was actually discharged (`uso_arma` array non-empty), and reports incident counts, ammunition use, personnel involved, injuries sustained, and — for exotic-species control — control-action outcomes and animals killed by species.

## Dashboard widgets

| Widget | Description |
|--------|-------------|
| Incidentes con uso de armamento | Total count of weapon-use incidents in the period |
| Total de municiones utilizadas | Total ammunition rounds fired across all incidents |
| Personal que efectuó disparos | Count of distinct personnel who discharged a weapon |
| Animales abatidos (Exótica) | Total animals killed, from Exótica events |
| Necesidad de desfundar el arma | Count of Agresión incidents where the officer needed to draw their weapon |
| Mapa de incidentes | Every weapon-use incident plotted at its recorded coordinates. Groupby setting colors points by event type, event ID, or another attribute |
| Uso de armamento por mes | Line chart of incident count over time, one line per event type |
| Uso de municiones por mes | Line chart of ammunition used over time, one line per event type |
| Uso de munición por tipo | Table: event count and total rounds used, grouped by ammunition type |
| Actividad del personal | Table: per-officer breakdown of Agresión incidents, Exótica incidents, total incidents, and total ammunition used |
| Lesiones sufridas | Table of injuries recorded on Agresión events (date/time, injury type, description) — one row per injury |
| Resultado de acción de control | Doughnut chart of control-action outcomes recorded on Exótica events |
| Animales abatidos por especie | Bar chart of animals killed, grouped by species (Exótica events only) |

This workflow has no groupers — all widgets show totals for the configured time range in a single dashboard view. The "Uso de armamento por mes" / "Uso de municiones por mes" trend charts and their time bucketing (day/week/month) are configurable via the Time Interval setting.

## Event types

| Event type | Notes |
|------------|-------|
| Agresión / amenaza a funcionario (`amenazas_funcionario`) | Assault or threat against an officer. Drives the "Necesidad de desfundar el arma" stat and the "Lesiones sufridas" injury table. |
| Exótica Animal (`animal_exotico`) | Invasive/exotic-species control action. Drives the "Resultado de acción de control" and "Animales abatidos por especie" charts. |

Both event types carry a `uso_arma` array in their event details recording each individual weapon-use entry (shooter, ammunition type and quantity); only events where this array is non-empty are counted as weapon-use incidents. Agresión records the shooter under `tirador`, Exótica under `nombre_tirador` — the workflow normalises both into a single `tirador_name` column before aggregating.

## Requirements

[pixi](https://pixi.sh) is required for environment and dependency management. You will also need an EarthRanger connection configured for the `apn` (or `apncentral`) data source.
