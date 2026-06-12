# Laboratorio de Observabilidad

## Cambio realizado

Durante el desarrollo del laboratorio se modificó el archivo `docker-compose.yml`, ya que al ejecutar el comando `docker compose up -d --build` se generaba un error relacionado con el montaje `rslave`.

Por ello, se cambió la siguiente línea:

```yaml
- /:/host:ro,rslave
```

por:

```yaml
- /:/host:ro
```

Luego de realizar ese cambio, los contenedores pudieron levantarse correctamente.

## Instrucciones para validar el trabajo

1. Clonar el repositorio:

```bash
git clone https://github.com/joaccopro/lab_Observabilidad.git
cd lab_Observabilidad
```

2. Levantar los servicios:

```bash
docker compose up -d --build
```

3. Verificar el estado de los contenedores:

```bash
docker compose ps
```

4. Acceder a los servicios principales:

* Frontend: http://localhost:8080
* Backend métricas: http://localhost:3001/metrics
* Grafana: http://localhost:3000
* Prometheus: http://localhost:9090
* Alloy: http://localhost:12345

5. Entrar a Grafana con:

```text
usuario: admin
clave: admin
```

6. Verificar que existan las fuentes de datos:

* Prometheus
* Loki

7. Revisar el dashboard creado en Grafana con los paneles de CPU y registros.

8. Generar carga desde el frontend con el botón **Generar carga de CPU** o usando:

```bash
curl "http://localhost:3001/load?seconds=90"
```

9. Verificar que la alerta **CPU backend > 50%** pase a estado `Firing`.

10. Revisar en los registros de aplicación el mensaje:

```text
grafana_alert_received
```

---

# 13. Preguntas a responder

## 1. ¿Por qué necesitamos a Loki además de Prometheus si ya tenemos `/metrics`?

Porque Prometheus solo trabaja con métricas numéricas, como CPU, memoria o peticiones, pero Loki permite revisar logs, ósea, mensajes, errores y eventos generados por la aplicación o infraestructura. Por eso, Prometheus ayuda a detectar un problema y Loki ayuda a entender qué ocurrió.

## 2. ¿Qué ventaja aporta que las fuentes de datos de Grafana estén aprovisionadas como código y no creadas a mano?

Que permite que la configuración sea automática y repetible. Al estar definida como código, Grafana reconoce Prometheus y Loki al iniciar sin configurarlos manualmente. Haciendo que ayude en reducir errores y facilita levantar el mismo entorno en otra máquina.

## 3. El panel "CPU contenedor" y el panel "CPU host" pueden mostrar valores muy distintos. ¿Por qué? ¿Cuál usarías para alertar sobre una aplicación concreta?

Pueden ser distintos porque el CPU del contenedor mide el consumo de una aplicación específica, mientras que el CPU del host mide el uso total de la máquina.

El que utilizaría para alertas sobre una aplicación concreta sería el CPU del contenedor, porque refleja directamente el comportamiento de esa aplicación.

## 4. ¿Qué diferencia hay entre el intervalo de evaluación y el período pendiente de una alarma?

Que el intervalo de evaluación indica cada cuánto tiempo Grafana revisa la condición de la alerta y, en cambio, el período pendiente indica cuánto tiempo debe mantenerse esa condición antes de pasar a `Firing`. Esto evita que la alarma se active por picos breves.







go sabado profe <3
