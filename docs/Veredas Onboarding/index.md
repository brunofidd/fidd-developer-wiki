# API

Aqui você encontra informações de como de integrar com o Veredas.

## Ambientes

| Environment | URL                                    |
|:-----------:|----------------------------------------|
| Produção    | https://api.veredas.fiddgroup.com      |
| Homologação | https://api.veredashomol.fiddgroup.com |

## Documentação 

<swagger-ui src="https://apipreprod.veredashomol.fiddgroup.com/swagger/v1.0-internal/swagger.json" validatorUrl="none"/>

## Passo a Passo
 
Para servir como uma introdução, este passo a passo apresentará as chamadas necessárias para realizar um processo de *OnBoarding* completo. Recomendamos utilizar nosso ambiente de homologação para realização dos primeiros envios, para que se familiarizem com o funcionamento do sistema.


## *OnBoarding* Cedente

 

### Autenticação

 
O primeiro passo é realizar a autenticação. A equipe FIDD fornecerá um usuário e senha, que deverão ser encaminhados no *endpoint* abaixo:

```http
POST https://{environment}/api/v1/auth
```

Exemplo:

```curl
curl -X POST "https://{environment}/api/v1/auth" \
	 -H  "accept: text/plain" \
	 -H  "Accept-Language: en-US" \
	 -H  "Content-Type: application/json-patch+json" \
	 -d "{  \"email\": \"{user}\",  \"password\": \"{password}\" }"
```

:   !!! warning "Atenção"

        Não esqueça de alterar os valores: `{environment}`, `{user}` e `{password}` por valores reais antes de realizar a chamada para nossa API.
		
 
Se a autenticação ocorrer com sucesso, será retornado um *accessToken*, que deverá ser utilizado em todos os *endpoints* do sistema de *OnBoarding*. O *token* deverá ser encaminhado no formato `Authentication Bearer: {token}`.
 
> **Nota**: Normalmente não será necessário encaminhar o parâmetro *partyId*, pois o mesmo só é exigido caso o usuário represente mais do que uma pessoa.


### Abertura de Processo

Na sequência, devemos realizar a abertura de um processo de *OnBoarding* de cedente. Nessa etapa,  informamos apenas o mínimo de informação para que o sistema realize a abertura do processo.


As informações mínimas compreendem: nome, e-mail e CPF/CNPJ.
 

Para cedentes pessoa física, devemos utilizar o *endpoint* `POST /api/v1/external/process/assignor/person`, enquanto que para cedentes pessoa jurídica, utilizamos o *endpoint* `POST /api/v1/external/process/assignor/organization`.
 

Se a requisição for processada com sucesso,  ambos os *endpoints* retornarão um `id`. Essa informação representa o Identificador do Processo de *OnBoarding* e será necessária para realizar qualquer tipo de operação sobre o processo.
 
> **Atenção**: Observar que o *endpoint* de abertura de processo é diferente para cedente Pessoa Física e Pessoa Jurídica.


### Envio das Informações do Cedente
 

Com o Identificador do Processo em mãos, podemos seguir para a próxima etapa: o envio das informações do cedente. De maneira similar à abertura do processo, existem *endpoints* distintos para pessoa física e jurídica.

* Pessoa Física: `POST /api/v1/external/process/assignor/person/{id}`
* Pessoa Jurídica: `POST /api/v1/external/process/assignor/organization/{id}`

Nesses dois *endpoints* são enviados todas as informações cadastrais do processo. Repare que o `id` presente na URL deve ser trocado para o identificador do processo recebido na etapa anterior.

> **Dica**: Consulte a documentação *swagger* para obter detalhes de todos parâmetros. Para campos que referenciam uma listagem, por exemplo `countryId`, consulte ao final do *swagger* a área *Schemas* para conhecer os valores possíveis.


### Envio de Documentos


Para o completo cadastro do cedente, será necessário o *upload* da documentação. O sistema aceitará os formatos padrões de documentos, como PDF, JPG ou PNG.


O envio de documento é realizado através do *endpoint*: `POST /api/v1/external/process/{processId}/document`.


> **Observação**: este *endpoint* aceita documentos tanto pessoa física como jurídica e pode ser chamado múltiplas vezes para cada processo de *OnBoarding*.

### Vínculo com Fundo


Na sequência, o próximo passo é vincular o(s) fundo(s) na qual o cedente irá operar. Para isso utilizar o *endpoint* abaixo.
`POST: /api/v1/external/process/assignor/{processId}/fund`

 
### Envio para análise

 
Por fim, a última etapa será submeter o processo de *OnBoarding* do cedente para análise da equipe de cadastro da FIDD. Para esta etapa, utilizar o endpoint:
`POST: /api/v1/external/process/{id}/send`

 
Pronto, nesse momento o processo já estará disponível para análise.

 
> **Dica**: Utilize o *endpoint* `GET /api/v1/external/process/list` para recuperar a situação atualizada da análise do processo ou ainda, utilize os *Webhooks* para ser notificado quando houver movimentação do processo.

 
> **Atenção**: Caso a equipe de cadastro necessite de algum esclarecimento ou informação adicional, o processo poderá voltar para preenchimento das informações complementares.