# Example of yaml configuration templating in helm

### Define a yaml config file template:

[config/config.yaml](config/config.yaml)

```yaml
{{ if not .Values.config }}# This is the default configuration file for the application.{{ end }}
host: {{ .Values.config.host | default "localhost" }}
port: {{ .Values.config.port | default 3000 }}
tls: {{ .Values.config.tls | default false }}
static:
  key: value
```

### Define a ConfigMap using the yaml template:

[templates/config-map.yaml](templates/config-map.yaml)

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config
data:
  config.yaml: |
{{ tpl (.Files.Get "config/config.yaml") . | indent 4 }}

```

### Render using default configuration

```bash
helm template . 
```

### Render using custom configuration

```bash
helm template . --values values-with-config.yaml
```
