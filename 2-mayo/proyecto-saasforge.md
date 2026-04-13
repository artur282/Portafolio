# 📊 SaaSForge

## 1. Visión general
SaaSForge es una plataforma fundacional de Software-as-a-Service (SaaS) multi-tenant lista para producción. Implementa patrones complejos empresariales como aislamiento de datos por inquilino mediante Row-Level Security (RLS) en PostgreSQL, procesamiento de datos vía pipelines ETL, facturación con pasarelas de pago y visualización dinámica de reportes.

## 2. Tecnologías principales
* **Backend**: FastAPI, Python 3.11+, SQLAlchemy
* **Base de datos**: PostgreSQL (RLS habilitado)
* **Datos y ETL**: Pandas, SQLite (para validación temporal)
* **Servicios de terceros**: Stripe API (Billing)
* **Visualización & Monitoreo**: Plotly (reportes), OpenTelemetry (logs)

## 3. Arquitectura
* **Tenant Isolation**: Arquitectura de base de datos compartida pero esquemas/tablas aisladas mediante `Row-Level Security` activado a nivel de conexión de base de datos interceptando el token del usuario.
* **DataBridge (ETL)**: Servicios que ingieren feeds de datos externos, los validan, limpian y los cargan de forma segura por tenant.
* **PayFlow (Billing)**: Integración con Stripe usando Webhooks. Manejo asíncrono para actualizar el estado de suscripción de los tenants de forma segura frente a fallos.
* **LogStream**: Arquitectura de ingestión para trazas estructuradas. Monitoreo analítico.

## 4. Requerimientos / Features Clave
1. **Autenticación Multi-tenant**: JWT que incluya `tenant_id`. Las queries de base de datos automáticamente filtran por RLS.
2. **Subscripciones Automáticas**: Creación de checkout sessions, webhooks verificados, lógica de downgrade/upgrade y restricciones de acceso según el plan (Premium/Basic).
3. **Pipeline ETL**: Endpoints para cargas masivas que usen Pandas para validar formato y tipos antes de ingestión.
4. **Reportes API**: Endpoints que generen datos crudos o figuras Plotly (JSON) de uso y métricas por tenant.
5. **Trazabilidad**: OpenTelemetry configurado para registrar errores, requests HTTP y consultas SQL largas.

## 5. Diseño de Base de Datos
* `tenants`: id, name, subscription_tier, stripe_customer_id
* `users`: id, tenant_id, email, password_hash
* `data_records`: id, tenant_id, payload (ingestado por ETL)
* Las políticas RLS aseguran que `WHERE tenant_id = current_setting('app.current_tenant_id')`.
