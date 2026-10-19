# Notification Service

Servicio HTTP base construido con Java 25, Spring Boot y Maven.

## Ejecución local con Maven

Requiere JDK 25 configurado en `JAVA_HOME`.

```bash
./mvnw spring-boot:run
```

En Windows:

```powershell
.\mvnw.cmd spring-boot:run
```

La comprobación de estado queda disponible en `http://localhost:8081/health`
(o en el puerto indicado por `PORT`).

## Monitoreo de disponibilidad

`GET /health` es público y responde HTTP `200 OK` con este cuerpo JSON:

```json
{"status":"UP"}
```

Es una comprobación de que el servicio HTTP está vivo: no consulta RabbitMQ ni
Brevo.

Para consultarlo desde UptimeRobot o un monitor equivalente, configura un monitor
HTTP(S) con la URL `https://<tu-servicio>.onrender.com/health`, método `GET`,
intervalo de 5 minutos y código de respuesta esperado `200`.

Render suspende los servicios gratuitos después de 15 minutos sin tráfico
entrante. Las consultas externas cada 5 minutos deberían evitar esa suspensión
por inactividad mientras el monitor siga activo. El servicio sigue sujeto a los
límites del plan gratuito, incluidas las 750 horas mensuales compartidas por
workspace. Consulta la [documentación de Render](https://render.com/docs/free).

## Ejecución local con Docker CLI

```bash
docker build -t notification-service .
docker run --rm -p 8080:8080 notification-service
```

## Despliegue en Render

El archivo `render.yaml` configura un Web Service con runtime Docker y la ruta
de salud `/health`. Render asigna el puerto mediante la variable `PORT`, que la
aplicación consume automáticamente.

No se requiere Docker Compose dentro de la aplicación desplegada.
