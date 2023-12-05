# ApiGateway

Este proyecto es un ejemplo de cómo implementar un API Gateway en Java utilizando Spring Cloud Gateway. Un API Gateway es un componente esencial en arquitecturas de microservicios, ya que actúa como un punto de entrada único para las solicitudes de los clientes, facilitando la gestión, enrutamiento y coordinación de las diferentes partes del sistema.

## Configuración

El archivo de configuración principal es `application.yml`, donde se definen las rutas del Gateway y se especifican los microservicios correspondientes. Aquí hay un vistazo a la configuración:

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: stops-route
          uri: ${STOPS_ROUTE_URI:http://localhost:8081}
          predicates:
            - Path=/stop/list
          filters:
            - AddResponseHeader=X-Powered-By, Gateway Service
        - id: calculators-route
          uri: ${CALCULATORS_ROUTE_URI:http://localhost:8082}
          predicates:
            - Path=/api/calculadora/**
          filters:
            - AddResponseHeader=X-Powered-By, Gateway Service
        - id: stephen-king-route
          uri: ${STEPHEN_KING_ROUTE_URI:https://stephen-king-api.onrender.com}
          predicates:
            - Path=/api/**
          filters:
            - AddResponseHeader=X-Powered-By, Gateway Service
        - id: postman-route
          uri: ${POSTMAN_ROUTE_URI:https://postman-echo.com}
          predicates:
            - Path=/post
          filters:
            - AddResponseHeader=X-Powered-By, Gateway Service
  management:
    endpoints:
      web:
        exposure:
          include: "*"
    endpoint:
      health:
        show-details: always
      gateway:
        enabled: true
```

### Microservicios Enlazados

- **Unitrack - Stop Read Service (Port: 8081):** Forma parte del repositorio [Unitrack](https://github.com/JhMateo/Unitrack). Este microservicio es responsable de proporcionar información sobre paradas y rutas. Puedes encontrar más detalles sobre este servicio específico [aquí](https://github.com/JhMateo/Unitrack/tree/master/backend/stop-services/stop-read-service), aunque su puerto se ha modificado para este proyecto.

- **CalculadoraAPI (Port: 8082):** Este microservicio, disponible en [GitHub](https://github.com/JhMateo/CalculadoraAPI), ofrece funcionalidades de calculadora a través de la ruta `/api/calculadora/**`. El resultado de las operaciones es devuelto en un formato JSON como este:

    ```json
    {
      "num1": 5.0,
      "num2": 3.0,
      "operation": "+",
      "result": 8.0
    }
    ```
    
- **Stephen King API (Port: External):** Este microservicio, ubicado en [https://stephen-king-api.onrender.com](https://stephen-king-api.onrender.com), ofrece datos relacionados con las obras de Stephen King. El API responde a solicitudes en la ruta `/api/**`.

- **Postman Echo (Port: External):** Este microservicio, disponible en [https://postman-echo.com](https://postman-echo.com), ofrece funcionalidades para probar solicitudes HTTP. El API responde a solicitudes en la ruta `/post`.


## Uso

Para aprovechar este API Gateway, simplemente realiza solicitudes HTTP según las rutas definidas en la configuración. El Gateway gestionará de manera transparente la conexión con los microservicios subyacentes.

¡Explora las posibilidades que ofrece este Gateway y personalízalo según tus necesidades! Si tienes alguna pregunta o sugerencia, no dudes en comunicarte.
