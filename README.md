# Avaliando Modelos Encoder-Decoder em Tarefas de Múltipla Escolha sob o Viés de Forma Superficial
Os modelos de Linguagem Grande (em inglês, LLM’s),
representam um grande avanço na realização de diversas
tarefas no campo da inteligência artificial. Utilizando a
configuração **zero-shot**, ou seja, quando o modelo executa
uma tarefa sem ter sido previamente ajustado com exemplos especı́ficos daquela atividade, é possı́vel realizar tarefas
como classificação de texto, inferência textual, sumarização e
resposta automática a perguntas. Entretanto, foi detectado que,
em tarefas envolvendo a escolha de alternativas em perguntas
de múltipla escolha, esses modelos podem selecionar uma
resposta incorreta, mesmo possuindo conhecimento suficiente
para apontar a resposta correta.

A explicação para esse fenômeno é denominado **surface
form competition**, uma propriedade dos modelos generativos
na qual a probabilidade de gerar uma resposta é diluı́da
entre diferentes formas superficiais possı́veis, mesmo que
todas essas formas transmitam a mesma ideia. Em outras
palavras, o modelo distribui a probabilidade entre várias
versões equivalentes da resposta, o que pode levar a uma
pontuação reduzida para a forma especı́fica presente entre as
alternativas oferecidas. Essa competição entre formas textuais
prejudica a precisão do modelo e afeta negativamente o seu
desempenho em tarefas de múltipla escolha.

Melhor explicando, podemos considerar a seguinte pergunta: “Uma pessoa quer se submergir na água. O que ela deve
usar?” e as suas alternativas “banheira de hidromassagem”,
“poça”, “copo” e “xı́cara”. A resposta correta seria “banheira
de hidromassagem”, no entanto, o modelo pode atribuir maior
probabilidade à palavra “banheira”, por ser mais comum,
mesmo que ela não esteja entre as opções listadas. Assim, ele
pode acabar escolhendo uma alternativa errada, como “poça”,
simplesmente porque a resposta correta teve sua probabilidade
“roubada” por uma forma equivalente mas ausente da lista.

Diante desse problema, o artigo [Surface Form Competition: Why the Highest Probability Answer Isn’t Always Right](https://aclanthology.org/2021.emnlp-main.564/) (Holtzman et al., EMNLP 2021) propõe o método
Domain Conditional Pointwise Mutual Information (PMIDC)
como solução. Esse método busca recalcular a pontuação das
respostas candidatas, considerando não apenas a probabilidade
da resposta dada a pergunta, mas também o quão provável
aquela resposta seria em um contexto neutro da tarefa (o
domı́nio). Dessa forma, o modelo penaliza respostas genéricas
ou comuns, e favorece aquelas que de fato se tornam mais
prováveis por causa da pergunta, corrigindo o viés introduzido
pela surface form competition.