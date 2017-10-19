
# API de Ofertas

Visando facilitar a integração no envio de ofertas entre o Buscapé e o lojista, o Buscapé criou a integração por API com o objetivo de diminuir o tempo de atualização das ofertas no site

Para ter acesso a API é preciso criar um **token de acesso**. Se você ainda não tem o token de acesso, siga os passos [**aqui**](http://developer.buscape.com.br/portal/tutoriais/criando-seu-app-token)

__Importante: Nunca forneça seu token ou dados de sua conta para ninguém!__


# Autenticação

Todas as requisições a API deverão conter no header os tokens de autenticação, conforme abaixo:

| Header | Descrição |
|---|---|
| app-token | Token que identifica a Aplicação que está efetuando a chamada aos recursos disponibilizados através da API |
| auth-token | Token que identifica o Lojista que está utilizando a Aplicação para efetuar chamadas aos recursos disponibilizados através da API **(Não obrigatório em ambiente de sandbox)** |

Para adquirir o auth-token, o lojista deverá entrar em contato com o Comercial do Buscapé.

### Códigos de erros

| Código | Descricação |
|---|---|
| 401 | Ausência de um dos Tokens no header ou token não existe |
| 403 | Token revogado ou inválido |

# APIs

O formato padrão nas requisições e respostas das API é **JSON**

### Ambientes

| Ambiente | URL |
|---|---|
| Produção | http://api.buscape.com.br |
| Sandbox | http://sandbox-api.buscape.com.br |


### Headers

#### Request
Todos os requests nas APIs deverão receber os headers abaixo

| Header |  Descrição |
|---|---|
| Content-Type| application/json; charset=utf-8 |
| app-token | Token de aplicação |
| auth-token | Token de autenticação |

#### Response

| Header |  Descrição |
|---|---|
| ticketid | Ticket de resposta referente a requisição |

**ticketid** Será sempre retornado para os endpoints */collection* e */inventory*. Com este id é possível identificar com maiores detalhes o status do processamento da requisição 

### /collection

Cria ou atualiza as ofertas no Buscapé. As ofertas podem ser enviadas em lotes (Lote máximo de 1000 ofertas por requisição)

| Nome | Descrição |
|---|---|
| Endpoint | /product/t1/collection |
| Method | POST |

#### Campos

| Nome | Obrigatório | Descrição | Tipo |
|---|---|---|---|
|groupId|	Não | Agrupador de ofertas (Deverá ser usado para agrupar variações diferentes de uma oferta </br> Exemplo: </br>Tenis xxx Branco (sku=100 - groupId=1) </br>Tenis xxx Preto  (sku=200 - groupId=1) </br>Tenis xxx Azul   (sku=300 - groupId=1) | String(10) |
|sku| Sim |	ID da oferta | String(240) |
|title | Sim | Título da oferta| String(240) |
|barcode| Não | Código de barras da oferta | String |
|category|	Sim | Categoria que a oferta se encontra </br>(Exemplo: Eletrônicos>TV) | String(255) |
|declaredPrice| Não | Preço declarado para seguro dos correios | Double |
|description| Não |	Descrição/Sinopse da oferta, aceita tags HTML: &lt;p&gt;, &lt;br&gt;, &lt;b&gt;, &lt;strong&gt;, &lt;li&gt;, &lt;div&gt;, &lt;span&gt; </br> **Não serão aceitos script e/ou css inline, ou qualquer outra tag não listada acima** | String(4000) |
|images[]| Sim | Lista de imagens da oferta  </br>**A primeira imagem da lista será a imagem exibida no Buscapé** | Array<String(4094)> |
|isbn| Não | Código ISBN para livros | Int(13) |
|link| Sim | Link da oferta para o site Buscapé | String(4094) |
|linkLomadee| Não | Link da oferta para os publishers da Lomadee | String(4094)|
|prices[]| Sim | Lista de preços da oferta| Array |
|prices[].type | Sim |	Tipo do preço, opções: </br>- boleto, cartao_avista</br>- cartao_parcelado_sem_juros</br>- cartao_parcelado_com_juros</br>**A oferta deverá conter ao menos 1 (um) preço à vista (boleto ou cartao_avista) e ao menos 1 (um) preço parcelado** | String |
|prices[].price |Sim| Preço da oferta para o Buscapé | Double |
|prices[].priceCpa| Não| Preço da oferta para CPA| Double |
|prices[].priceLomadee| Não | Preço da oferta para Lomadee | Double |
|prices[].installmentValue| Sim |	Valor da parcela  | Double |
|prices[].installment|	Sim | Quantidade de parcelas | Int |
|productAttributes|	Não | Características principais da oferta </br>Exemplo: Cor, Voltagem, Tamanho, etc | Map<Key,Value> |
|technicalSpecification| Sim | Especificações técnicas da oferta </br> Exemplo: Tamanho da tela, tipo de material, marca, etc | Map<Key,Value>(10000) |
|quantity| Sim | Quantidade/Estoque da oferta | Int |
|sizeHeight| Sim | Altura da oferta (cm)| Int |
|sizeLength| Sim | Comprimento da oferta (cm)| Int |
|sizeWidth| Sim	| Largura da oferta (cm)| Int |
|weightValue| Sim | Peso da oferta (gramas)| Double |
|handlingTimeDays| Não | Tempo de manuseamento da oferta. Deve ser acrescido ao prazo de frete da oferta </br> Exemplo: Tempo necessário para encordoar uma raquete de tênis |marketplace| Não | Indica se essa oferta é vendida no modelo de marketplace| Boolean |

#### Request

	[
	    {
	        "groupId": "",
	        "sku": "",
	        "title": "",
	        "barcode": "",
	        "category": "",
	        "description": "",
	        "images": [
	            "url1",
	            "url..."
	        ],
	        "isbn": "",
	        "link": "",
	        "linkLomadee": "",
	        "prices": [
	            {
	                "type": "",
	                "price": 0,
	                "priceLomadee": 0,
	                "priceCpa": 0,
	                "installment": 0,
	                "installmentValue": 0
	            }
	        ],
	        "productAttributes": {
	            "Atributo 1": "Valor 1",
	            "Atributo ...": "Valor ..."
	        },
	        "technicalSpecification": {
	            "Especificação 1": "Valor",
	            "Especificação ...": "Valor ..."
	        },
	        "quantity": 0,
	        "sizeHeight": 0,
	        "sizeLength": 0,
	        "sizeWidth": 0,
	        "weightValue": 0,
	        "declaredPrice": 0,
	        "handlingTimeDays": 0,
	        "marketplace": false,
	        "marketplaceName": ""
	    }
	]

#### Response

Status code: **200**

Todas as ofertas foram recebidas sem problemas e serão processados em breve

	[
	    {
	        "sku": "414641",
	        "status": "SUCCESS"
	    },
	    {
	        "sku": "314641",
	        "status": "SUCCESS"
		}
	]

* * *
Status code: **400**

Lista de ofertas invalidas

	[
	    {
	        "sku": "1622",
	        "errors": [
	            {
	                "code": "4",
	                "message": "Atributo link inválido. O atributo é obrigatório, precisa ser um link válido, tamanho máx. 4094 caracteres e sem espaços em branco."
	            }
	        ]
	    }
	]

* * *
Status code: **500**

Modelo de resposta de erro de formato inválido ou erro desconhecido

	{
	    "errors": [
	        {
	            "code": 0,
	            "message": "Erro no processamento da requisição."
	        }
	    ]
	}

### /inventory

Atualiza dados da quantidade e preço da oferta já cadastradas no Buscapé (Enviadas previamente pelo endpoint /collection)

| Nome | Descrição |
|---|---|
| Endpoint | /product/t1/inventory |
| Method | PUT |

#### Campos

| Nome | Obrigatório | Descrição | Tipo |
|---|---|---|---|
|sku| Sim |	ID da oferta | String(240) |
|prices[]| Sim | Lista de preços da oferta| Array |
|prices[].type | Sim |	Tipo do preço, opções: </br>- boleto, cartao_avista</br>- cartao_parcelado_sem_juros</br>- cartao_parcelado_com_juros</br>**A oferta deverá conter ao menos 1 (um) preço à vista (boleto ou cartao_avista) e ao menos 1 (um) preço parcelado** | String |
|prices[].price |Sim| Preço da oferta para o Buscapé | Double |
|prices[].priceCpa| Não| Preço da oferta para CPA| Double |
|prices[].priceLomadee| Não | Preço da oferta para Lomadee | Double |
|prices[].installmentValue| Sim |	Valor da parcela  | Double |
|prices[].installment|	Sim | Quantidade de parcelas | Int |
|quantity| Sim | Quantidade/Estoque da oferta | Int |

#### Request

	[
	    {
	        "groupId": "",
	        "sku": "",
	        "title": "",
	        "barcode": "",
	        "category": "",
	        "description": "",
	        "images": [
	            "url1",
	            "url..."
	        ],
	        "isbn": "",
	        "link": "",
	        "linkLomadee": "",
	        "prices": [
	            {
	                "type": "",
	                "price": 0,
	                "priceLomadee": 0,
	                "priceCpa": 0,
	                "installment": 0,
	                "installmentValue": 0
	            }
	        ],
	        "productAttributes": {
	            "Atributo 1": "Valor 1",
	            "Atributo ...": "Valor ..."
	        },
	        "technicalSpecification": {
	            "Especificação 1": "Valor",
	            "Especificação ...": "Valor ..."
	        },
	        "quantity": 0,
	        "sizeHeight": 0,
	        "sizeLength": 0,
	        "sizeWidth": 0,
	        "weightValue": 0,
	        "declaredPrice": 0,
	        "handlingTimeDays": 0,
	        "marketplace": false,
	        "marketplaceName": ""
	    }
	]

#### Response

Status code: **200**

Todas as ofertas foram recebidas sem problemas e serão processados em breve

	[
	    {
	        "sku": "414641",
	        "status": "SUCCESS"
	    },
	    {
	        "sku": "314641",
	        "status": "SUCCESS"
		}
	]

* * *
Status code: **400**

Lista de ofertas invalidas

	[
	    {
	        "sku": "1622",
	        "errors": [
	            {
	                "code": "4",
	                "message": "Atributo link inválido. O atributo é obrigatório, precisa ser um link válido, tamanho máx. 4094 caracteres e sem espaços em branco."
	            }
	        ]
	    }
	]

* * *
Status code: **500**

Modelo de resposta de erro de formato inválido ou erro desconhecido

	{
	    "errors": [
	        {
	            "code": 0,
	            "message": "Erro no processamento da requisição."
	        }
	    ]
	}

### /search

Consulta as ofertas enviadas e seu status atual, se o \{SKU\} não for informado no Endpoint, API retornará todas as ofertas

| Nome | Descrição |
|---|---|
| Endpoint | /product/search |
| Endpoint | /product/search/**\{SKU\}** |
| Method | GET |

#### 1 - Parâmetros

| Nome | Tipo | Obrigatório | Descrição |
|---|---|---|---|
|size|QueryString|Não|Determina a quantidade de ofertas que deve ser retornado por página. Default: 12 itens por página e máximo 1000|
|page|QueryString|Não|Determina a página|
|sku|PathVariable|Sim|Sku da oferta|

#### Response

Status code: **200**

	{
	    "totalPages": 0,
	    "totalItems": 0,
	    "filters": {
	        "size": "0",
	        "page": "0"
	    },
	    "products": [
	        {
	            "summary": {
	                "status": "",
	                "reason": "",
	                "creationDate": "",
	                "updateDate": "",
	                "priceUpdatingDate": "",
	                "stockUpdatingDate": "",
	                "history": null
	            },
	            "productDataSent": {
	                "groupId": "",
	                "sku": "",
	                "title": "",
	                "barcode": 0,
	                "category": "",
	                "description": "",
	                "images": [
	                    ""
	                ],
	                "isbn": "",
	                "link": "",
	                "linkLomadee": "",
	                "prices": [
	                    {
	                        "type": "",
	                        "price": 0,
	                        "priceLomadee": 0,
	                        "priceCpa": 0,
	                        "installment": 0,
	                        "installmentValue": 0
	                    }
	                ],
	                "productAttributes": {
	                    "": ""
	                },
	                "technicalSpecification": {
	                    "": ""
	                },
	                "quantity": 0,
	                "sizeHeight": 0,
	                "sizeLength": 0,
	                "sizeWidth": 0,
	                "weightValue": 0,
	                "declaredPrice": 0,
	                "handlingTimeDays": 0,
	                "marketplace": false,
	                "marketplaceName": ""
	            },
	            "publishedProduct": {
	                "buscapeId": 0,
	                "productBuscapeId": 0,
	                "title": "",
	                "categoryName": "",
	                "categoryId": 0,
	                "price": 0,
	                "priceLomadee": 0,
	                "installment": 0,
	                "installmentValue": 0,
	                "link": "",
	                "linkLomadee": "",
	                "linkInBuscape": ""
	            }
	        }
	    ]
	}



### Códigos de retorno

|Código|HTTP Status|Descrição|
|---|---|---|
|0	|500|	Sistema indisponível no momento.|
|2	|400|	O atributo sellerId é obrigatório.|
|3	|400|	O atributo status é obrigatório. (WAITING / PROCESSING / AVAILABLE_FOR_PUBLICATION / PUBLICATING / FINISHED)|
|4	|400|	O atributo link é obrigatório.|
|5	|400|	O atributo linkLomadee é obrigatório.|
|6	|400|	Atributo price inválido. O atributo é obrigatório, double/float e maior que 0.0|
|7	|400|	Atributo priceLomadee inválido. O atributo deve ser double/float e maior que 0.0.|
|8	|400|	O atributo title é obrigatório.|
|9	|400|	Atributo barcode inválido. O atributo deve ser numérico e ter tamanho máx. 240 caracteres.|
|10	|400|	É obrigatório informar pelo menos uma imagem no atributo images.|
|11	|400|	O atributo batchId é obrigatório.|
|12	|400|	O atributo offerList é obrigatório com tamanho máximo = 1000.|
|13	|400|	O atributo action é obrigatório. (INSERT / DELETE)|
|14	|400|	O atributo sku é obrigatório.|
|15	|400|	O atributo category é obrigatório.|
|16	|400|	O atributo description é obrigatório.|
|17	|400|	O atributo bachExecutionPK é obrigatório.|
|18	|400|	O atributo batchExecutionPK.execution é obrigatório.|
|19	|400|	O atributo batchExecutionPK.status é obrigatório. (WAITING / PROCESSING / AVAILABLE_FOR_PUBLICATION / PUBLICATING / FINISHED)|
|20	|400|	O atributo batchExecutionId é obrigatório.|
|21	|400|	O atributo actionForPublication é obrigatório (BATCH / INVENTORY)|
|22	|400|	É obrigatório informar pelo menos um dos atributos price, priceLomadee ou quantity.|
|23	|400|	SKU não foi encontrado.|
|24	|400|	O atributo url é obrigatório, não pode conter espaços e o arquivo deve ser gzip. (*.gz)|
|25	|400|	Atributo quantity inválido. O atributo tem que ser numérico e igual ou maior que 0.|
|26	|400|	Atributo type inválido. O atributo é obrigatório e as opções possíveis são: boleto, cartao_avista, cartao_parcelado_sem_juros ou cartao_parcelado_com_juros.|
|27	|400|	O atributo installment é obrigatório e deve ser maior que 0 (zero).|
|28	|400|	O atributo prices é obrigatório.|
|29	|400|	Content-Type inválido.|
|30	|400|	Necessário informar pelo menos um preço no atributo prices.|
|31	|400|	Atributo sizeHeight está inválido. É obrigatório e deve ser numérico.|
|32	|400|	Atributo sizeLength está inválido. É obrigatório e deve ser numérico.|
|33	|400|	Atributo sizeWidth está inválido. É obrigatório e deve ser numérico.|
|34	|400|	Atributo weightValue está inválido. É obrigatório e deve ser numérico.|
|35	|400|	Atributo declaredPrice está inválido. Não é obrigatório, mas quando enviado o campo deve ser númerico e maior que 0.|
|36	|400|	Atributo handlingTimeDays está inválido. Não é obrigatório, mas quando enviado o campo deve ser númerico e maior que 0.|
|37	|400|	Formato JSON está inválido.|
|38	|400|	Lista de ofertas esta vazia ou nula. (mínimo 1 produto)|
|39	|400|	Atributo url inválido. O atributo é obrigatório e deve ser uma URL válida.|
|40	|400|	Atributo execHour inválido. O atributo é obrigatório e deve ser numérico.|
|41	|400|	Atributo format inválido. O atributo é obrigatório e deve ser um dos seguintes valores: json, xml, csv.|
|42	|400|	Atributo columnDelimiter inválido. O atributo é obrigatório quando o formato é csv.|
|43	|400|	Atributo execDayOfMonth e execWeekday estão nulos. Ao menos um dos atributos devem ser enviados.|
|44	|400|	Atributo execDayOfMonth está inválido. O atributo deve ser numérico.|
|45	|400|	Atributo execWeekday está inválido. O atributo deve ser numérico.|
|46	|400|	Ambos atributos execDayOfMonth e execWeekday estão preenchidos. Só deve ser enviado apenas 1 deles.|
|47	|401|	Header auth-token inválido.|
|48	|401|	Header app-token inválido.|
|49	|401|	Header auth-token e app-token inválidos.|
|50	|400|	Elemento não pode ser null.|
|51	|400|	Atributo installmentValue inválido. O atributo é obrigatório, double/float e maior que 0.0|
|52	|400|	Atributo status inválido. O atributo é obrigatório e as opções possíveis são: sent, delivered ou canceled|
|53	|400|	Atributos reason inválido. O atributos é obrigatório quando o status = canceled.|
|54	|400|	Atributos trackingCode inválido. O atributos é obrigatório quando o status = sent.|
|55	|400|	Atributos deliveryDate inválido. O atributos é obrigatório quando o status = delivered.|
|56	|400|	O atributo status é obrigatório (SENT / DELIVERED / CANCELED).|
|57	|400|	Formato inválido do atributo image, deve ser um array de links de imagens. Exemplo: ["http://xxx.jpg","http://yyy.png"]|
|58	|400|	Atributo technicalSpecification está inválido. Deverá ter formato map (Exemplo: {"atributo 1":"valor 1", "atributo 2":"valor 2"})|
|59	|400|	Atributo productAttributes está inválido. Deverá ter formato map (Exemplo: {"atributo 1":"valor 1", "atributo 2":"valor 2"})|
