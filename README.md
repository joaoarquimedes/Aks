# Azure AKE

<br>

Projeto git para documentar o procedimento de preparação da infraestrutura Kubernetes na Azure AKS.

<br>

Procedimentos:
- [Certificado Digital](README/certificate.md)
- [Ingress](README/ingress.md)
- [ArgoCD](README/argocd.md)

## Secret para o ACR (Azure Container Registry)

Para recuperar o nome de usuário e senha na ACR:
```
az acr credential show --name <_NOME DO REGISTRY_> --resource-group <_NOME RESOURCE GROUP_> --output table
```

Exemplo para criação da secret para uso no Kubernetes interno:
```
kubectl create secret docker-registry <_NOME DA SECRET_>.secret --docker-server <_DOMINIO DA ACR_>.azurecr.io --docker-username <_USUARIO_> --docker-password <_SENHA_> --docker-email <_EMAIL_>
```
