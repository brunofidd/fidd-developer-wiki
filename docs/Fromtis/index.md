# Começando

## Laiautes

[Laiaute - CNAB 444](./CNAB444_REMESSA_FIDD.pdf)

[Laiaute - CNAB 500](./CNAB500_REMESSA_FIDD.pdf)

## Estrutura do CNAB

Esta página tem como objetivo prover informações relacionados a Estrutura do CNAB.

O **arquivo CNAB** (Centro Nacional de Automação Bancária) é um formato padronizado utilizado para troca de informações entre instituições financeiras e empresas. O **layout 500 e 444**, especifica a estrutura de um arquivo CNAB utilizado para transações de pagamento, negociação de títilos, instruções, movimentações, entre outros.
> A estrutura de um arquivo CNAB é composta por registros, que são agrupamentos lógicos de informações relacionadas a cada transação.

Basicamente,

- **Registro Header:** É o primeiro registro do arquivo e contém informações gerais sobre o arquivo, como identificação do originador, data de geração do arquivo, e informações de conta corrente do favorecido *(opcional)*.
    
- **Registro Detalhe:** Esse registro contém os dados específicos de cada título. É o registro onde poderá conter uma ou mais linhas, 15/30/60/300/500+ linhas. Cada registro detalhe representa uma única linha de título onde possui informações como identificador do título, valor do título, valor da movimentação, identificador do documento/contrato, data de vencimento e emissão, número do termo de cessão, dados do sacado e cedente, entre outros dados relevantes.
    
- **Registro Trailer:** É o último registro do arquivo e tem a finalidade de totalizar as informações contidas nos registros detalhe. Esse registro contém informações como o número total de registros detalhe. É o registro mais simples, apenas para completar a estrutura lógica do CNAB.

>*Além desses registros principais, existem registros adicionais que podem ser utilizados para informações complementares, como registros de ocorrências, espécie de título, coobrigação, taxa de juros, endereço do sacado, entre outros.*

Para melhor entendimento sobre os campos, registros e posições, consulte o layout.

## Exemplos

Os layouts de CNAB utilizados pela FIDD são: layout CNAB500 e CNAB444.

=== ":octicons-file-code-16: `CNAB500.rem`"
 
 
	```
    08REMESSA01COBRANCA       00000010896709000170ORIGINADOR ABIENTE DE TESTES  500FIDD GROUP TEST190723        MX0000001260 0101        895547                                                                                                                                                                                                                                                                                                                                                                   000001     
	1                   02000000000000000   RECEBIVEL001          000                 0000000000  000000        01NRO_DOC_0126092400000358694120000000001 18012300002000000000000    NMDOTERMO      000002295384000000000000000292687125000150SACADO TESTE                            RUA TESTE 123                                       88811223CEDENTE AMBIENTE DE TESTES                    00258547000191000000000000000000000000000000000000        00000000000000000000000000000                           000002
	9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             000003
	```

=== ":octicons-file-code-16: `CNAB444.rem`"
 
 
	```
    01REMESSA01COBRANCA       00000036476501000196ORIGINADOR AMBIENTE DE TESTES 500FIDD GROUP TEST190723        MX0000001260 0101        895547                                                                                                                                                                                                                                                                                                           000001
    1                   02000000000000000   RECEBIVEL001          000                 0000000000  000000        01NRO_DOC_0124092400000254712630000000001 18012300002000000000000 NMDOTERMO         000001853624800000000000000286942666000184SACADO TESTE                            RUA TESTE 123                                       88811223CEDENTE AMBIENTE DE TESTES                    0025854700019100000000000000000000000000000000000000000000000002
    9                                                                                                                                                                                                                                                                                                                                                                                                                                                     000003
	```

## Tipos de Movimento

Os Tipos de Movimento no CNAB são chamados de identificação da ocorrência, também conhecida como "código de ocorrência", é um campo utilizado no layout do arquivo CNAB para indicar um código que determina o tipo de movimento que esse título terá dentro do sistema.
> O código da ocorrência / código do movimento (atrelado via sistema), determina qual o tipo de processamento determinado título irá ter ao dar entrada no sistema.
 
Exemplos:

|Código|Movimento|Descrição|
|----------------|-------------------------------|-----------------------------|
|01|Aquisição|Realiza a entrada de títulos na carteira do fundo.        |
|02|Baixa|Efetua a baixa de determinado título da carteira do fundo. Considerado uma baixa total, esse movimento inativa o título do estoque.          |
|06|Prorrogação de Vencimento|Faz com que a data de vencimento original do título seja alterada para a data de vencimento nova informada.|
|14|Liquidação Parcial|Liquida parcialmente determinado título, atua como baixa parcial, subtraindo valor nominal - valor pago.|
|46|Alteração valor nominal (face)|Opera diretamente no valor nominal original do título, alterando para o novo valor informado.|
|74|Baixa por Recompra|Baixa determinados títulos do estoque em formato de Recompra.|
|84|Entrada por Recompra|Realiza a entrada de novos títulos no estoque em formato de Recompra.|
|99|Reativação|Aciona a ativação de determinado título que esteja inativado.|

Para outros tipos de movimento consulte o [Suporte FIDD](mailto:nicolas.schmidt@fiddgroup.com).

## Código do Originador
Os **Originadores** são a Entidade que **origina as cessões** dentro do Fromtis.
> É um código criado pelo TI da FIDD que serve como 'passe' para entrada do CNAB no sistema.

- O código do Originador é informado nas posições 27 a 46 do arquivo CNAB, na parte do registro header.

Para solicitar o seu código de originador, entre em contato com o [Suporte FIDD](mailto:nicolas.schmidt@fiddgroup.com).


## Manual WebService

[Manual de Serviços WS do Fromtis](./[MANUAL - SERVIÇOS WS].pdf).

>Para envio de requests ao Fromtis é necessário possuir credenciais.
>(Solicite seu acesso  [Suporte FIDD](mailto:nicolas.schmidt@fiddgroup.com)).

Serviço de **Importação Arquivo CNAB** - Realiza a importação de CNAB para o Portal.

- https://fidc.fiddgroup.com/portal/portal-servicos/servicos/soap/importacaoArquivoCnab?wsdl

Exemplo do Request:

    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" 
    xmlns:soap="http://soap.consulta.servicos.portal.fidc.fromtis.com.br/">
     <soapenv:Header/>
     <soapenv:Body>
     <soap:importarArquivo>
     <arquivoCnab>
     <cnpjFundo>99999999999999</cnpjFundo>
     <arquivo>cid:cnab001.zip</arquivo>
     </arquivoCnab>
     </soap:importarArquivo>
     </soapenv:Body>
    </soapenv:Envelope>


Exemplo do Response:

    <S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
     <S:Body>
     <ns2:importarArquivoResponse xmlns:ns2="http://soap.consulta.servicos.portal.fidc.fromtis.com.br/">
     <retornoImportacaoArquivo>
     <statusCode>000</statusCode>
     <descricaoRetorno>Importado com Sucesso, Acompanhe o arquivo pelo 
    Portal.</descricaoRetorno>
     </retornoImportacaoArquivo>
     </ns2:importarArquivoResponse>
     </S:Body>
    </S:Envelope>


> *Alternativo*:
>*Serviço de **Importação Arquivo Remessa** - Realiza a importação de CNAB para o sistema.*
>*https://fidc.fiddgroup.com/portal/portal-servicos/servicos/soap/importacaoArquivoRemessa?wsdl*

Serviço de **Relatório de Estoque** - Realiza o agendamento do relatório de Estoque.
- https://fidc.fiddgroup.com/portal/portal-servicos/servicos/soap/agendador/relatorioEstoque?wsdl
> Request para gerar um id de agendamento do relatório de estoque.

Serviço de **Consulta de Relatório** - Realiza uma consulta de determinado relatório agendado.
- https://fidc.fiddgroup.com/portal/portal-servicos/servicos/soap/agendador/consultarRelatorio?wsdl
> Request para consultar o id de agendamento gerado do relatório.

Serviço de **Relatório Liquidados/Aquisição** - Realiza o agendamento do relatório de liquidação/aquisição.
- https://fidc.fiddgroup.com/portal/portal-servicos/servicos/soap/agendador/relatorioAquisicaoLiquidados?wsdl
> Request para gerar um id de agendamento do relatório de liquidados/aquisição. Para consultar utilize o request consultarRelatorio.
