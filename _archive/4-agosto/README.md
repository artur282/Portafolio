# 🚗 Agosto — IoT & Real-Time Data Engineering

> _"Cuando los datos llegan de miles de sensores por segundo y no podés perder ni uno."_

## 🎯 Objetivo del mes

Dominar la arquitectura de ingesta masiva de datos en tiempo real combinando Rust para la captura de alta frecuencia, Kafka como broker de eventos distribuidos y TimescaleDB para persistencia y análisis de series temporales — el stack estándar de la industria IoT.

---

## 📅 Proyectos del mes

### 🏗️ Proyecto Principal: [IoTStream](./proyecto-iotstream.md)
Plataforma de telemetría vehicular en tiempo real. Rust/Axum ingesta miles de eventos por segundo desde sensores simulados (GPS, RPM, temperatura), Kafka garantiza exactly-once delivery, TimescaleDB los persiste con continuous aggregates automáticos, y FastAPI expone analytics predictivos de mantenimiento preventivo.

- **Tecnologías**: Rust (Axum, rdkafka), Apache Kafka, TimescaleDB, FastAPI, Grafana, k6
- **Estado**: ⬜ Pendiente

---

## 🧠 Habilidades que se desarrollan

- Rust en producción: ingesta HTTP/2 de alto throughput sin GC pauses.
- Event-driven architecture con Apache Kafka: particiones, consumer groups, exactly-once semantics.
- TimescaleDB: hypertables, continuous aggregates, compresión automática, retention policies.
- Diseño de sistemas tolerantes a fallos: out-of-order events, sensor dropout, late-arriving data.
- Observabilidad: Grafana + TimescaleDB datasource para dashboards de series temporales.
