# Tutorial de Criação de Keystore para Spring Config Server

Este tutorial orienta você sobre como criar um arquivo JKS (Java KeyStore) para uso com o Spring Config Server. 

## Passo 1: Gerar o Keystore

Execute o comando abaixo para gerar um keystore usando `keytool`. Este comando cria um keystore chamado `configserver.jks` na pasta `src/main/resources`.

```sh
keytool -genkeypair \
  -alias mykey \
  -keyalg RSA \
  -keysize 2048 \
  -validity 365 \
  -keystore src/main/resources/configserver.jks \
  -storetype JKS \
  -storepass nopass \
  -keypass nopass \
  -dname "CN=spring-config-server, OU=TI, O=MyDigital, L=Teresina, ST=Piauí, C=BR"
````

**Parâmetros do comando:**

- `-alias mykey`: Alias para o par de chaves.
- `-keyalg RSA`: Algoritmo de criptografia.
- `-keysize 2048`: Tamanho da chave.
- `-validity 365`: Validade do certificado em dias.
- `-keystore src/main/resources/configserver.jks`: Caminho e nome do arquivo keystore.
- `-storetype JKS`: Tipo do keystore.
- `-storepass nopass`: Senha do keystore.
- `-keypass nopass`: Senha da chave.
- `-dname "CN=spring-config-server, OU=TI, O=MyDigital, L=Teresina, ST=Piauí, C=BR"`: Informações do certificado.

## Passo 2: Configurar o Spring Cloud Config Server

Depois de criar o keystore, configure o Spring Cloud Config Server para usar o arquivo JKS.

Adicione a seguinte configuração no arquivo `application.yml` ou `bootstrap.yml` do Config Server:

```yaml
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: file:///${user.home}/config-repo
          clone-on-start: true

encrypt:
  key-store:
    location: classpath:configserver.jks
    password: nopass
    alias: mykey
    secret: nopass
