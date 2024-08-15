# Instalação do Cert-manager

Instalado o cert-manager para gestão automática dos certificados digitais das publicações externas de domínios.

- Link: [https://cert-manager.io/](https://cert-manager.io/)
- Documentação oficial: https://cert-manager.io/docs/installation/kubectl/

<br>

Comando executado para instalação:
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.15.2/cert-manager.yaml
```
<br>

**ClusterIssuer Staging**: Configurando e disponibilizando o Cluster Issuer para criação do certificado digital em estado de teste, utilizando o Let's Encrypt ACME HTTP-01 Challenge.

```
kubectl apply -f cert-manager/clusterissuer-staging.yml
```
<br>

**ClusterIssuer Produção**: Configurando e disponibilizando o Cluster Issuer para criação do certificado digital em produção, utilizando o Let's Encrypt ACME HTTP-01 Challenge.

```
kubectl apply -f cert-manager/clusterissuer.yml
```
<br>

**ClusterIssuer Selfsigned**: Configurando e disponibilizando o Cluster Issuer para criação de certificado auto assinado.

```
kubectl apply -f cert-manager/clusterissuer-selfsigned.yml
```