# API

Aqui você encontra informações de como de integrar com o Maps.

## Ambientes

| Environment | URL                                    |
|:-----------:|----------------------------------------|
| Produção    | https://api.maps.fiddgroup.com         |
| Homologação | https://api.mapshomol.fiddgroup.com    |


## Autenticação

O primeiro passo é realizar a autenticação. A equipe FIDD fornecerá um usuário e senha. A autenticação é feita através de um cabeçalho `Authorization` com o esquema `basic`.

Exemplo:

```
Authorization: Basic XXXXXXXXX
```

Em todas as chamadas será necessário enviar esse cabeçalho.

## Carteiras

O segundo passo é puxar as informações de carteira disponíveis para o seu usuário.
Todos os outros `endpoint` terão como dependência alguma informação da carteira.

Para trazer as informações das carteiras que você tem acesso, basta fazer conforme o exemplo abaixo:

```
curl -X 'GET' \
  'https://api.maps.fiddgroup.com/api/v1/pegasus/carteiras' \
  -H 'accept: text/plain' \
  -H 'Authorization: Basic XXXXXXXXX'
```

O resultado irá trazer um modelo de dados parecido com este:

```json
[
  {
    "id": 100000,
    "mnemonico": "TESTE",
    "nomeCompleto": "TESTE",
    "cnpj": "00000001000101",
    "dataProcessamento": 1689692400000,
    "dataProcessamentoCalculada": "2023-07-18T15:00:00Z",
    "dataImplantacao": 1650466800000,
    "dataImplantacaoCalculada": "2022-04-20T15:00:00Z"
  }
]
```

O `id` é o que será utilizado nos casos em que é solicitado o parâmetro `{idCarteira}`.

## Documentação 

<swagger-ui src="https://api.maps.fiddgroup.com/swagger/v1.0/swagger.json" validatorUrl="none"/>

## Observações

Atualmente o grupo de APIs relacionado à **Composição carteira consolidadora** não trazem informações, pois não estão sendo utilizados por nós.