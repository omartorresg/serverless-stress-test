# Pruebas de Carga y Estrés - Arquitectura Serverless AWS

Este repositorio contiene los scripts de prueba, configuraciones y documentación técnica utilizados para evaluar el rendimiento, la escalabilidad y la resiliencia de una arquitectura *Serverless* desplegada en AWS.

---

## 🏗️ Arquitectura Evaluada
* **API Gateway (HTTP API):** Punto de entrada síncrono para las peticiones (`duth346k92`).
* **AWS Lambda:** Funciones backend para procesamiento de peticiones e integración.
* **Amazon SQS:** Cola de mensajes desacoplada para la absorción de picos asíncronos (`notifications_queue`).
* **Neon PostgreSQL:** Base de datos relacional en la nube.
* **AWS CloudWatch:** Observabilidad y captura de métricas del sistema.

---

## ⚙️ Parámetros de Configuración de Pruebas (`parameters.md`)

* **Herramienta de Pruebas:** Apache JMeter (v5.6.3)
* **Protocolo:** HTTPS / REST API
* **Target Endpoint:** API Gateway HTTP (`https://duth346k92.execute-api...`)

### Escenarios Ejecutados:
1. **Línea Base & Carga Progresiva:**
   * **50 VUs:** Validación de operación nominal y detección de *Cold Starts*.
   * **100 VUs / 250 VUs:** Evaluación de latencias y tasa de procesamiento (*Throughput*).
   * **500 VUs:** Punto máximo de comportamiento estable sin fallos críticos.
2. **Prueba de Estrés (Stress Testing):**
   * **750 VUs:** Rampa de aceleración agresiva (15s) para forzar el punto de colapso estructural.
   * **Resultados:** Tasa de error del **19.07%** y latencia P99 de **2,607 ms**, activando la condición de parada por saturación de conexiones en la base de datos.
3. **Prueba de Recuperación (Recovery Testing):**
   * Cese abrupto de la carga a 750 VUs.
   * Verificación del vaciado seguro de la cola SQS (*draining*) y retorno de las métricas de CloudWatch a niveles basales.

---

## 📂 Estructura del Repositorio

```text
├── scripts/
│   └── test-plan.jmx          # Script de prueba exportado desde Apache JMeter
└── README.md                  # Configuración, parámetros y guía del proyecto
