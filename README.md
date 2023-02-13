# Análise de dados da empresa Discorama

Para sanar as principais demandas da Discorama, foi feita a análise dos dados através do software Power BI que visa responder algumas perguntas sobre o negócio:<br>
<br>•	Qual o ticket médio atual?<br>

O ticket médio foi calculado utilizando a seguinte fórmula DAX:<br>
Ticket Médio = <br>
    VAR faturamento = SUM(payment[amount])<br>
    VAR vendas = CALCULATE<br>
        (count(payment[amount]),<br>
        payment[amount] <> 0)<br>
    VAR ticket = DIVIDE(faturamento, vendas)<br>
    RETURN <br>
    ticket<br>

A fórmula de vendas especifica que o valor do pagamento deve ser diferente de zero pois existiam alguns pagamentos duplicados e o valor das duplicatas estava armazenado como zero, o que afetava a contagem. O resultado foi inserido na Dashboard em uma visualização do tipo cartão.
<br><br>•	Qual o prazo médio de devolução dos discos?
<br>Foi criada uma coluna adicional na tabela “rental” chamada “date_diff”, que calcula a diferença em dias entre a data da locação e a data da entrega através da função “Duration.Days”. A média do valor dessa coluna foi apresentada em uma visualização do tipo indicador, no qual foi estabelecido como meta o prazo de 3 dias para devolução.
<br><br>•	Quais os países e filmes que tem maior tempo médio de devolução?
<br>Os valores foram apresentados em dois gráficos de barras empilhadas. O primeiro apresenta o tempo de devolução agrupado por país, enquanto o segundo apresenta o tempo de devolução e o prazo máximo agrupados por filme.
<br><br>•	Qual a receita por dia, mês e ano?
<br>O valor da soma da receita por locação foi exibido em um gráfico de linhas agrupado por dia, mês e ano.
<br><br>•	Qual o volume de locações e a receita por vendedor?
<br>A soma da receita agrupada por vendedor foi exibida em um gráfico de barras empilhadas, enquanto a contagem de locações agrupada por vendedor foi exibida em um gráfico de colunas empilhadas.
<br><br>•	Para fins de controle de inventário, quais são os filmes mais locados por categoria e qual é a disponibilidade dos filmes por loja?
<br>Em um gráfico de barras, foi apresentada a contagem de itens no inventário agrupados por categoria de filme utilizando o nome de cada categoria no campo de múltiplos pequenos. Para a disponibilidade de filmes por loja, foi criado um gráfico de barras empilhadas contendo a contagem dos itens no inventário agrupados por título, utilizando o código da loja como legenda.
<br><br>•	Quantos clientes existem por país?
<br>O gráfico de clientes por país foi apresentado em uma visualização de gráfico de barras empilhadas, utilizando no eixo X a contagem de identificadores de clientes e no eixo Y o país.
<br><br>•	Qual o total de clientes ativos por loja?
<br>Foram utilizadas visualizações do tipo cartão para exibir o total de clientes ativos. A divisão entre os clientes da loja 1 e os clientes da loja 2 foi feita através da criação de duas medidas utilizando a seguinte fórmula DAX:
<br>Clientes loja 1 = 
<br>    CALCULATE(COUNT(
<br>        customer[customer_id]),
<br>        customer[store_id] = 1
<br>    )
<br>Clientes loja 2 = 
<br>    CALCULATE(COUNT(
<br>        customer[customer_id]),
<br>        customer[store_id] = 2
<br>    )

<br> As medidas foram, então, dispostas em um gráfico de rosca.
<br>O Painel
<br><br>
Além da criação de medidas adicionais, foram necessários alguns ajustes nos dados recebidos. Os valores referentes a moedas foram importados pelo Power BI como valores inteiros devido ao uso de pontos para separar os centavos. Foi necessária a criação de uma coluna de exemplo para que fosse estabelecido o padrão das vírgulas dando ao número duas casas decimais. Para melhor organização do painel, foram criados botões que dão acesso às visualizações gerais, de inventário e de cliente. 
<br>Na primeira página foram dispostos os principais indicadores do negócio, relativos à analise geral: locação por filme, receita por vendedor, locação por vendedor, tempo de devolução por país, tempo de devolução por filme, ticket médio, prazo médio de devolução e receita por período. O painel está disposto na imagem a seguir.
<br><br>![image](https://user-images.githubusercontent.com/89671532/217552461-5bcf7938-7322-4198-bd23-a122d43e8a58.png)
<br><br>Pode ser observado que atualmente o tempo médio de devolução dos filmes é de 4,5 dias. Atualmente cada filme conta com um prazo de entrega diferente, conforme exposto no gráfico “tempo de devolução por filme”. Idealmente os filmes deveriam ter um mesmo prazo de entrega para facilitar o controle dos funcionários. Foi colocado como meta o prazo médio de 3 dias para entrega – a meta pode ser alterada em função da necessidade. 
<br>Existem também alguns filmes cuja quantidade de locações é bastante baixa, conforme pode ser observado no gráfico de Locação por filme. É válido o estudo da viabilidade de manter e fazer a reposição dos títulos no catálogo em caso de problemas.
<br>Em relação aos vendedores, ambos têm desempenhos bastante semelhantes tanto em receita quanto em volume. A métrica deverá ser observada de forma contínua. 
<br><br> ![image](https://user-images.githubusercontent.com/89671532/217552320-0d732929-9beb-46aa-9f71-9eff26d6e53e.png)
<br><br>Para melhor controle do inventário e da disponibilidade de filmes, foi criado uma segunda aba contendo as tabelas de Locação por loja e categoria e de Inventário por loja e categoria. A quantidade da locação de cada categoria deverá guiar o inventário da companhia. Foram observados em diversas categorias que o volume de locação é consideravelmente maior em uma loja, mas o estoque tem maior disponibilidade em outra. Os títulos deverão ser movidos entre as lojas de forma a ajustar o estoque de acordo à necessidade. 
<br>Na mesma página também existe uma visualização de filmes em cada loja para melhor controle da localização dos títulos.
<br><br> ![image](https://user-images.githubusercontent.com/89671532/217552615-0c209bde-3347-47e6-bc25-4f0fb822047c.png)
<br><br>Para controle de clientes foi criada uma terceira aba contendo algumas informações. Visando entender qual o maior mercado da empresa, foi criado um gráfico de clientes por país, que mostra que boa parte dos clientes se concentram em alguns poucos países. Foram criados cartões com a quantidade de clientes ativos, ticket médio de cada loja e número de clientes por loja. Pode ser observado que o ticket e o número de clientes é equilibrado entre as duas unidades. Ao clicarmos em um país da lista, a tabela de clientes e e-mails é filtrada de forma que só apareçam os clientes daquele país, facilitando a visualização e a veiculação de publicidade por e-mail.
<br><br>
Conclusão<br>
<br>
Em um primeiro momento, a empresa deverá investir em marketing de mídias sociais para aumentar seu portifólio de clientes. Foi observado que os países com maior número de clientes são a Índia, China, Estados Unidos, Japão, México, Brasil e Rússia, então a veiculação de publicidade nesses países, unido ao conhecimento já existente do serviço no local, deverá gerar resultados positivos. Podem ser implementadas soluções como bônus ou descontos para aqueles que indicarem o serviço para amigos. Deverão ser estabelecidas métricas a serem acompanhadas durante a veiculação das campanhas para que os resultados sejam estudados e possíveis ajustes sejam feitos. Além disso, alguns países possuem um número extremamente baixo de clientes. É uma boa ideia pausar a operação nesses locais e focar naqueles com potencial para aumento de clientes. O serviço pode ser retomado posteriormente, de forma gradual e controlada, durante um processo de expansão.
A companhia também deve estudar a migração gradual do serviço para o digital. Isso permitiria que clientes em novos países tivessem acesso ao serviço, aumentando o portfólio, além de permitir o controle do tempo de devolução dos filmes – eles ficariam disponibilizados por um período já predefinido na plataforma, e problemas como esse não existiriam. Para os filmes físicos, o ideal é que o tempo de devolução seja o mesmo para todas as mídias, facilitando o controle por parte dos funcionários.
Os dados devem seguir sob acompanhamento e modificações deverão ser feitas conforme novos dados forem surgindo. 

