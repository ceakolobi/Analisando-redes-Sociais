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
