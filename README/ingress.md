# Instalação do Ingress

Instalação e configuração da infraestrutura Ingress com Traefik.

Documentações:

- Instalação: https://github.com/traefik/traefik-helm-chart
- Exemplos configurações: https://github.com/traefik/traefik-helm-chart/blob/master/EXAMPLES.md
- Arquivo oficial: https://github.com/traefik/traefik-helm-chart/blob/master/traefik/values.yaml
- Requisito: [https://helm.sh/](https://helm.sh/)

<br>

Primeiro definindo um namespace para agrupar todos os recursos referentes ao Ingress Traefik.

```
kubectl apply -f ingress/namespace.yml
```
<br>

Via **helm**, adicionar o repositório do traefik
```
helm repo add traefik https://helm.traefik.io/traefik
```

Verificar/Atualizar o repositório
```
helm repo update
```
<br>


Verificar versões
```
# Versão corrente
helm search repo traefik

# Versões anteriores
helm search repo traefik --versions
```
<br>

No arquivo (`ingress/values.yaml`) oficial de instalação do [**Traefik**](https://github.com/traefik/traefik-helm-chart/blob/master/traefik/values.yaml) via **helm**, foram adicionados ajustes de configurações para adequar ao ambiente.

Argumentos extras:
```
additionalArguments:
  - "--serverstransport.insecureskipverify=true"
```

Habilitar dashboard:
```
ingressRoute:
  dashboard:
    enabled: true
```

Outras alterações:
```
metrics:
  prometheus:
    entryPoint: metrics
    buckets: "0.1,0.3,1.2,5.0"
```

Para o storage do certificado digital:
```
deployment:
...
  initContainers:
  - name: volume-permissions
    image: busybox:latest
    command: ["sh", "-c", "mkdir /data/storage; chown -R 65532:65532 /data/storage"]
    securityContext:
      runAsNonRoot: false
      runAsGroup: 0
      runAsUser: 0
    volumeMounts:
      - name: data
        mountPath: /data

...

persistence:
  enabled: true
  name: data
  accessMode: ReadWriteMany
  size: 10Gi
  storageClass: azurefile
  path: /data

...

certResolvers:
  letsencrypt:
    email: infraestrutura@localhost
    storage: /data/storage/acme.json
```

Para storage de gravações dos logs:
```
...
logs:
  general:
    level: INFO
  access:
    enabled: true
    filePath: "/data/storage/log/traefik/access.log"
    bufferingSize: 100
```

<br><br>

## Instalação ou atualização

Comando para instalação do traefik
```
helm install --namespace=traefik -f ingress/values.yaml traefik traefik/traefik
```

Comando para atualização do traefik. Aplicar alterações
```
helm upgrade --namespace=traefik -f ingress/values.yaml traefik traefik/traefik
```
<br><br>


## Middlware

Redirecionamento http para https: `traefik-redirect-https@kubernetescrd`
```
kubectl apply -f middleware/redirect-https.yml
```

<br><br>

## Publicando a porta para acesso:

Traefik
```
kubectl apply -f services/dashboard.yml
```

Prometheus
```
kubectl apply -f services/prometheus.yml
```

Recuperando as portas mapeadas
```
kubectl get services --namespace traefik traefik-ui
kubectl get services --namespace traefik metrics
```
