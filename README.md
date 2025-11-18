Projeto de Grafo com Neo4j: Análise de Rede Social para Startup
Este projeto demonstra a aplicação de um banco de dados de grafos, utilizando o Neo4j (na plataforma AuraDB), para modelar uma rede social e extrair insights analíticos avançados. O objetivo é fornecer à startup "Analista de Redes" a capacidade de identificar usuários influentes, mapear comunidades de interesse e entender a dinâmica de engajamento do conteúdo.📊 1. Modelagem do Grafo (Esquema)A estrutura da rede social foi modelada com base em 3 tipos de Nós (Nodes) e 4 tipos de Relacionamentos (Relationships), conforme o modelo de Propriedade de Grafo.ElementoRótulo (Label)DescriçãoPropriedades ChaveNóUsuarioEntidade principal da rede social.id, nome (ex: "Alice", "Bob"), localizacao (ex: "SP", "RJ")NóConteudoUnidade de postagem.id (ex: "p101"), tipo (ex: "Foto", "Vídeo"), dataCriacaoNóTagTemas de interesse do conteúdo.nome (ex: "Culinaria")Relacionamento:CRIOULiga o Usuario ao Conteudo que ele publicou.NenhumaRelacionamento:SEGUELiga um Usuario a outro (conexão social).NenhumaRelacionamento:CURTIUInteração de engajamento.dataAcaoRelacionamento:COMENTOUInteração de engajamento.dataAcao, textoRelacionamento:MARCADO_COMLiga o Conteudo às suas Tags.Nenhuma
O grafo completo inicial possui 10 Nós (4 Usuario, 3 Conteudo, 3 Tag) e 13 Relacionamentos.

🧠 2. Análise de Insights Avançados (Consultas Cypher)
As consultas Cypher a seguir demonstram o poder dos grafos para responder a perguntas de negócio complexas que seriam difíceis em um banco de dados relacional tradicional.

A. Análise de Influência e Conexão (Shortest Path)
Pergunta de Negócio: Como podemos encontrar o caminho de conexão mais curto entre dois usuários, e quem são os usuários mais influentes da rede?
1. Consulta de Caminho Mais Curto (shortestPath)
Esta consulta calcula o menor número de passos (grau de separação) entre dois usuários.
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
MATCH p=shortestPath((a:Usuario {nome: 'Alice'})-[*]-(b:Usuario {nome: 'Bob'})) 
RETURN p
Insight Gerado: Mapeia a proximidade e a estrutura de comunidades.
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
3. Consulta de Influência (Total de Seguidores)
Esta consulta identifica os usuários com base no número de seguidores que possuem, fundamental para a segmentação de campanhas de marketing de influência.
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
MATCH (influencer:Usuario) <- [r:SEGUE] - (follower:Usuario) 
RETURN influencer.nome AS NomeInfluencer, count(r) AS TotalSeguidores 
ORDER BY TotalSeguidores DESC
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
Resultado da Consulta no Neo4j: | NomeInfluencer | TotalSeguidores | | :--- | :--- | | Alice | 2 | | Bob | 1 |

Insight Gerado: A usuária Alice é a mais influente na rede com 2 seguidores.

B. Análise de Engajamento e Interesse por Conteúdo
Pergunta de Negócio: Quais conteúdos estão gerando mais engajamento total (Curtidas + Comentários) e quais Tags (tópicos) estão por trás das interações dos usuários?

1. Consulta de Engajamento Total por Conteúdo
Soma todas as interações (CURTIU e COMENTOU) para cada item de conteúdo.
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
MATCH (c:Conteudo) <- [r] - (u:Usuario) 
WHERE type(r) IN ['CURTIU', 'COMENTOU'] 
RETURN c.id AS PostID, c.tipo AS Tipo, count(r) AS EngajamentoTotal 
ORDER BY EngajamentoTotal DESC
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
Resultado da Consulta no Neo4j: | PostID | Tipo | EngajamentoTotal | | :--- | :--- | :--- | | p101 | Foto | 2 | | p102 | Vídeo | 1 | | p103 | Foto | 1 |

Insight Gerado: O post de ID p101 é o mais popular com 2 interações.

2. Consulta de Tags com Maior Frequência de Interação (Interesse de Usuário)
Esta consulta ajuda a segmentar o usuário Alice por seus interesses ativos (curtidas ou comentários).

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
MATCH (u:Usuario {nome: 'Alice'}) - [interacao:CURTIU|COMENTOU] -> (c:Conteudo) 
- [:MARCADO_COM] -> (t:Tag) 
RETURN t.nome AS TagDeInteresse, count(t) AS Frequencia 
ORDER BY Frequencia DESC
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
Resultado da Consulta no Neo4j: | TagDeInteresse | Frequencia | | :--- | :--- | | Culinaria | 1 |

Insight Gerado: O principal interesse registrado ativamente por Alice é Culinária.

💻 3. Código e Ambiente
Embora o foco tenha sido alterado da interface (Flask/Elementor) para a documentação, o projeto original previa uma API de conexão.

Conexão: Neo4j AuraDB (Instância Analizando dados RS).

Linguagem da API (Opcional): Python 3 (Flask)

Comando de Teste de API (Desabilitado): O ambiente de desenvolvimento estava configurado em /home/eduardo/projeto-dio-grafos. O comando de execução seria python3 app.py, mas a integração não é mais necessária para o desafio.

Os arquivos CSV/JSON exportados do Neo4j e os screenshots das consultas Cypher no console servem como a principal evidência da conclusão deste desafio de projeto.

CEA -171120252329
