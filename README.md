# 🌐 API Gateway

Este servicio actúa como punto de entrada único para todos los microservicios del sistema. Encapsula y enruta las peticiones HTTP entrantes hacia el servicio correspondiente, facilitando la gestión y seguridad.

## Tecnologías utilizadas
- Java 17
- Spring Boot
- Spring Cloud Gateway
- Eureka Client
- Spring Cloud Config Client

## Funcionalidades
- Ruteo dinámico de peticiones HTTP a los microservicios registrados.
- Descubrimiento automático de servicios mediante Eureka.
- Centralización del acceso y gestión de endpoints.
- Centralización de la documentación de los microservicios mediante Swagger UI, accesible desde un único punto de entrada.


## Configuración

El Gateway obtiene su configuración desde el **Spring Cloud Config Server**, incluyendo las rutas definidas para cada servicio.


## 🏁 Ejecución local

Asegúrate de que los servicios de Eureka y Config Server estén corriendo.


## Ejemplo de rutas

```yaml
spring:
  cloud:
    gateway:
      routes:
        # ReportService
        - id: report-service
          uri: lb://report-service
          predicates:
            - Path=/api/v1/reports/**
          filters:
            - RewritePath=/api/v1/reports/(?<segment>.*), /reports/$\{segment}

        #InventoryService
        - id: inventory-service
          uri: lb://inventory-service
          predicates:
            - Path=/api/v1/**
          filters:
            - RewritePath=/api/v1/(?<segment>.*), /$\{segment}

        # Rutas para la documentación Swagger de los microservicios
        - id: report-service-docs
          uri: lb://report-service
          predicates:
            - Path=/v3/api-docs/reports/**

        - id: inventory-service-docs
          uri: lb://inventory-service
          predicates:
            - Path=/v3/api-docs/inventory**
```

## 🖼️ Arquitectura

                 +--------------------+
                 |    API Gateway     |
                 +--------------------+
                           |
             +-------------+--------------+
             |                            |
    +------------------+         +------------------+
    | InventoryService |         |   ReportService  |
    +------------------+         +------------------+
