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
