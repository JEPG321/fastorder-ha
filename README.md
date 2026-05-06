# fastorder-ha

Sistema de pedidos en linea basado en microservicios con alta disponibilidad, PostgreSQL, Redis, RabbitMQ, Docker, Prometheus y Grafana.

## Estrategia de ramas

### Rama `main`

`main` es la rama estable final del proyecto. Debe contener solamente versiones probadas y listas para entrega o despliegue. Nadie debe trabajar directamente sobre `main`; los cambios llegan a esta rama por Pull Request desde `develop` cuando la integracion ya fue validada.

### Rama `develop`

`develop` es la rama de integracion y pruebas. Todas las ramas `feature/*` deben salir desde `develop`, y los Pull Request de nuevas funcionalidades deben apuntar primero a `develop`.

### Ramas `feature/*`

Cada integrante debe trabajar en una rama `feature/*` segun el microservicio o area asignada:

- `feature/api-gateway`: trabajo relacionado con el API Gateway.
- `feature/menu-service`: trabajo relacionado con el servicio de menu.
- `feature/order-service`: trabajo relacionado con el servicio de pedidos.
- `feature/inventory-service`: trabajo relacionado con el servicio de inventario.
- `feature/kitchen-service`: trabajo relacionado con el servicio de cocina.
- `feature/delivery-service`: trabajo relacionado con el servicio de entregas.
- `feature/notification-service`: trabajo relacionado con el servicio de notificaciones.
- `feature/monitoring-tests`: trabajo relacionado con monitoreo, Prometheus, Grafana y pruebas.

### Flujo de trabajo recomendado

1. Clonar el repositorio.
2. Cambiarse a `develop`.
3. Actualizar `develop` antes de empezar.
4. Crear o cambiarse a la rama `feature/*` asignada.
5. Hacer commits pequenos y descriptivos.
6. Subir los cambios a GitHub.
7. Abrir un Pull Request desde la rama `feature/*` hacia `develop`.
8. Revisar, probar e integrar cambios en `develop`.
9. Cuando `develop` este probado, abrir un Pull Request desde `develop` hacia `main`.

### Comandos basicos de Git

Clonar repositorio:

```bash
git clone https://github.com/JEPG321/fastorder-ha.git
cd fastorder-ha
```

Cambiarse a `develop`:

```bash
git checkout develop
```

Actualizar `develop`:

```bash
git pull origin develop
```

Crear rama feature desde `develop`:

```bash
git checkout develop
git pull origin develop
git checkout -b feature/nombre-de-la-feature
```

Subir cambios:

```bash
git status
git add .
git commit -m "Descripcion breve del cambio"
git push -u origin feature/nombre-de-la-feature
```

Abrir Pull Request hacia `develop`:

1. Entrar al repositorio en GitHub.
2. Seleccionar la rama `feature/nombre-de-la-feature`.
3. Crear un Pull Request con destino `develop`.
4. Esperar revision, resolver comentarios y validar pruebas.

Integrar `develop` hacia `main` cuando este probado:

```bash
git checkout develop
git pull origin develop
```

Luego abrir un Pull Request en GitHub desde `develop` hacia `main`.

## API Gateway

El servicio `api-gateway` es el punto de entrada unico de FastOrder HA. Esta construido con Spring Boot, Spring Cloud Gateway, Actuator y Micrometer Prometheus. No contiene logica de negocio; su responsabilidad es redirigir peticiones hacia los microservicios internos y exponer endpoints de salud y metricas.

### Rutas configuradas

El gateway corre en el puerto `8080` y redirige las rutas publicas hacia los servicios internos:

| Ruta publica | Servicio interno |
| --- | --- |
| `/api/menu/**` | `http://menu-service:8081` |
| `/api/orders/**` | `http://order-service:8082` |
| `/api/inventory/**` | `http://inventory-service:8083` |
| `/api/kitchen/**` | `http://kitchen-service:8084` |
| `/api/delivery/**` | `http://delivery-service:8085` |
| `/api/notifications/**` | `http://notification-service:8086` |

Todas las rutas usan `StripPrefix=1`, por lo que el prefijo `/api` se elimina antes de reenviar la peticion. Por ejemplo:

```text
http://localhost:8080/api/menu -> http://menu-service:8081/menu
```

### Levantar con Docker Compose

Desde la raiz del repositorio:

```bash
docker compose up --build api-gateway
```

El archivo `docker-compose.yml` incluye la red `fastorder-network` y deja preparado el `depends_on` hacia los servicios internos. Las imagenes de los demas microservicios deben existir o ser reemplazadas por sus builds cuando cada servicio sea implementado.

Mientras los demas servicios aun no existan, se puede levantar solo el gateway para probar Actuator:

```bash
docker compose up --build --no-deps api-gateway
```

### Probar health check

```bash
curl http://localhost:8080/actuator/health
```

### Probar metricas Prometheus

```bash
curl http://localhost:8080/actuator/prometheus
```
