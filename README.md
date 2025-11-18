Documentação da Modelagem do Grafo
Tipo,Rótulo (Label),Descrição,Propriedades Chave
Nó,Usuario,Entidade principal da rede social.,"id, nome, localizacao"
Nó,Conteudo,"Unidade de postagem (Foto, Vídeo).","id, tipo, dataCriacao"
Nó,Tag,Temas ou interesses do conteúdo.,nome (ex: Culinaria)
Relacionamento,:CRIOU,Liga o Usuario ao Conteudo que ele publicou.,Nenhuma
Relacionamento,:SEGUE,Liga um Usuario a outro (conexão social).,Nenhuma
Relacionamento,:CURTIU,Interação de engajamento.,dataAcao
Relacionamento,:COMENTOU,Interação de engajamento.,"dataAcao, texto"
Relacionamento,:MARCADO_COM,Liga o Conteudo às suas Tags.,Nenhuma# Analisando-redes-Sociais
----////////////////////////////////////////////////////////////////////////////////////----
A. Análise de Conexões e Comunidades (Pergunta-Chave)Objetivo: Encontrar a conexão mais curta (caminho) entre dois usuários e identificar caminhos através de um tipo de interesse comum (Conteúdo com a mesma Tag).Pergunta de NegócioConsulta Cypher (Exemplo com Alice e Bob)Resultado EsperadoQual é o caminho mais curto entre Alice e Bob?MATCH p=shortestPath((a:Usuario {nome: 'Alice'})-[*]-(b:Usuario {nome: 'Bob'})) RETURN pRetorna a sequência de relacionamentos (ex: Alice segue Bob).Qual Tag conecta a maioria dos usuários mais influentes?MATCH (u:Usuario) <- [:SEGUE] - () WITH u, count(*) AS seguidores WHERE seguidores > 1 MATCH (u) - [:CRIOU] -> (c:Conteudo) - [:MARCADO_COM] -> (t:Tag) RETURN t.nome, count(u) AS InfluencersPorTag ORDER BY InfluencersPorTag DESCRetorna a Tag que é publicada pelos usuários com mais de 1 seguidor.
----////////////////////////////////////////////////////////////////////////////////////////---
Análise de Engajamento por Tópico (Popularidade)Objetivo: Identificar as Tags mais populares (com maior número total de interações, e não apenas o número de posts).Pergunta de NegócioConsulta CypherQuais são as Top 3 Tags (tópicos) com o maior engajamento total (Curtidas + Comentários)?`MATCH (t:Tag) <- [:MARCADO_COM] - (c:Conteudo) <- [r:CURTIU
