Este projeto utiliza Python para raspar dados de comentários de vídeos do YouTube, oferecendo uma ferramenta poderosa para análise de sentimentos, pesquisa de mercado e insights educacionais. Através de duas APIs chave – a API do YouTube e a API da OpenAI –, ele permite coletar dados ricos, como comentários, contagem de likes, datas, nomes de usuários e links para perfis do YouTube.

O diferencial deste projeto é sua flexibilidade: ao inserir a API do YouTube, os usuários podem facilmente raspar dados. Para análises mais profundas, como interpretação de comentários com modelos de linguagem avançados, a API da OpenAI pode ser integrada. Vale ressaltar que o uso da API da OpenAI implica em custos adicionais.

Inicialmente projetado para análise de sentimentos, a aplicabilidade da ferramenta se expandiu para áreas como análise de concorrência e identificação de lacunas didáticas. Um exemplo notável de sua eficácia é a análise de feedback de cursos, onde a ferramenta destacou informações desatualizadas, erros de design e omissões cruciais, permitindo correções significativas.

Usuários podem executar análises complexas ou simples, dependendo da necessidade. Com a capacidade de processar milhares de comentários em um único prompt, especialmente com as atualizações recentes dos modelos GPT que suportam até 128k de tokens, a ferramenta se mostra extremamente versátil.

# Analise-de-coment-rios-do-Youtube
Web Scraping em python para coletar comentários do Youtube
Criar um benchmark para análise de sentimentos de comentários do YouTube exige uma estrutura bem planejada. Vamos dividi-lo em etapas:

**1. Coleta de Dados**

```
pip install google-api-python-client pandas
```

**2. Estrutura do CSV para Análise de Sentimentos**

- 'ID do Comentário': um identificador único para cada comentário.
- 'Comentário': o texto do comentário.
- 'Data': a data em que o comentário foi postado.
- 'Likes': número de curtidas no comentário.
- 'Sentimento': uma coluna vazia que será preenchida após a análise de sentimentos.
    
    

**3. Processamento e Análise de Sentimentos**

- Preencha a coluna 'Sentimento' do CSV com os resultados.

FUNÇÃO obterSentimento(comentário):
resposta = CHAMADA_API_GPT(comentário)
// Suponhamos que a API GPT retorna uma escala de -1 (negativo) a 1 (positivo).

```
SE resposta > 0.5:
    RETORNE "Positivo"
SENÃO SE resposta > -0.5 E resposta <= 0.5:
    RETORNE "Neutro"
SENÃO:
    RETORNE "Negativo"

```

PARA cada comentário em arquivo_CSV:
sentimento = obterSentimento(comentário)
ADICIONE sentimento ao CSV na coluna "Sentimento"


DADOS análiseScores = {
'SentimentoPositivo': 0,
'SentimentoNeutro': 0,
'SentimentoNegativo': 0,
'TemasRecurrentes': {},
'Profundidade': 0,
'IroniaSarcasmo': 0,
'QuestoesFrequentes': {},
'ComparacaoOutrosCursos': 0,
'Recomendações': {}
}

PARA cada comentário em arquivo_CSV:
análise = CHAMADA_API_GPT(comentário)

```
SE análise.sentimento == "Positivo":
    análiseScores['SentimentoPositivo'] += 1
SENÃO SE análise.sentimento == "Neutro":
    análiseScores['SentimentoNeutro'] += 1
SENÃO:
    análiseScores['SentimentoNegativo'] += 1

PARA tema em análise.temas:
    SE tema em análiseScores['TemasRecurrentes']:
        análiseScores['TemasRecurrentes'][tema] += 1
    SENÃO:
        análiseScores['TemasRecurrentes'][tema] = 1

análiseScores['Profundidade'] += análise.profundidade

SE análise.ironia_ou_sarcasmo:
    análiseScores['IroniaSarcasmo'] += 1

PARA questao em análise.questoes:
    SE questao em análiseScores['QuestoesFrequentes']:
        análiseScores['QuestoesFrequentes'][questao] += 1
    SENÃO:
        análiseScores['QuestoesFrequentes'][questao] = 1

SE análise.compara_outros_cursos:
    análiseScores['ComparacaoOutrosCursos'] += 1

PARA recomendação em análise.recomendações:
    SE recomendação em análiseScores['Recomendações']:
        análiseScores['Recomendações'][recomendação] += 1
    SENÃO:
        análiseScores['Recomendações'][recomendação] = 1

```

// Agora, vamos criar um sistema de pontos para avaliar o que é mais relevante.

DADOS relevanciaScores = {
'SentimentoPositivo': análiseScores['SentimentoPositivo'] * 5,
'SentimentoNegativo': análiseScores['SentimentoNegativo'] * -5,
'TemasRecurrentes': SOMA(análiseScores['TemasRecurrentes'].valores()) * 3,
'Profundidade': análiseScores['Profundidade'],
'IroniaSarcasmo': análiseScores['IroniaSarcasmo'] * -3,
'QuestoesFrequentes': SOMA(análiseScores['QuestoesFrequentes'].valores()) * 2,
'ComparacaoOutrosCursos': análiseScores['ComparacaoOutrosCursos'] * 4,
}

ordenarPorRelevância = ORDENE(relevanciaScores, DESCENDENTE)

RETORNE ordenarPorRelevância

DADOS tendências = {}
DADOS intenções = {
'Assistência': 0,
'Sugestão': 0,
'Opinião': 0,
'Pergunta': 0,
'Crítica': 0
}

FUNÇÃO detectarTendências(comentário):
tendência = CHAMADA_API_GPT("Qual tendência este comentário sugere?", comentário)
SE tendência NÃO em tendências:
tendências[tendência] = 1
SENÃO:
tendências[tendência] += 1
RETORNE tendência

FUNÇÃO analisarIntenção(comentário):
intenção = CHAMADA_API_GPT("Qual é a intenção principal deste comentário?", comentário)
SE intenção em intenções:
intenções[intenção] += 1
RETORNE intenção

PARA cada comentário em arquivo_CSV:
detectarTendências(comentário)
analisarIntenção(comentário)

RETORNE tendências, intenções

**4. Cálculos Matemáticos para o Benchmark**

- **Percentual de Sentimentos Positivos:** **`(Número de comentários positivos / Total de comentários) * 100`**
- **Percentual de Sentimentos Neutros:** **`(Número de comentários neutros / Total de comentários) * 100`**
- **Percentual de Sentimentos Negativos:** **`(Número de comentários negativos / Total de comentários) * 100`**
- **Comentários com Maior Engajamento:** Os comentários com mais 'Likes' indicam áreas de forte reação (positiva ou negativa).

**5. Interpretação dos Resultados**

- Se a maioria dos comentários for positiva, isso indica uma boa recepção do curso.
- Comentários negativos podem destacar áreas de melhoria.
- Comentários neutros podem ser mais informativos, portanto, uma análise manual pode ser útil.
- Observar os comentários com mais 'Likes' pode dar uma visão sobre quais opiniões são mais compartilhadas pela comunidade.

**6. Visualização**

- Gráficos de pizza ou barras para mostrar a distribuição de sentimentos.
- Gráficos de linhas para tendências ao longo do tempo (se houver comentários suficientes ao longo de um período prolongado).

Concluindo, esse benchmark oferece uma visão quantitativa e qualitativa sobre a recepção do curso de Inteligência Artificial com base nos comentários do YouTube. Adapte conforme necessário para atender às suas necessidades específicas!
