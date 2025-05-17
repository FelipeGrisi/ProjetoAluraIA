# ProjetoAluraIA
Projeto final da Imersão Alura IA + Google Gemini


# 🎶 MoodTunes: Criador de playlists no Spotify orientado por afetos.

## Sobre o Projeto

O MoodTunes é um projeto demonstrativo desenvolvido em Python que explora a capacidade de modelos de linguagem de inteligência artificial, especificamente o Google Gemini, para sugerir playlists musicais baseadas no estado emocional do usuário.

O sistema analisa uma descrição textual fornecida pelo usuário sobre como ele se sente, identifica o humor predominante e, em seguida, simula uma busca por músicas que correspondam a essa emoção, apresentando uma lista de sugestões de forma amigável.

Este projeto serve como um exemplo prático de como encadear múltiplas chamadas a um modelo de linguagem para realizar uma tarefa mais complexa, dividindo-a em etapas lógicas (análise, busca simulada, formatação).

## Tecnologias Utilizadas

* **Python:** A linguagem de programação principal.
* **Google Gemini API (via biblioteca `google-genai`):** Utilizada diretamente para alimentar os modelos que executam a análise emocional, a simulação da busca por músicas e a formatação final da playlist.
* **Google Colab:** O ambiente de desenvolvimento online onde o código foi executado e testado.
* **Markdown:** Para a formatação deste arquivo README.

## Como Funciona (Visão Geral do Fluxo)

O projeto segue uma pipeline de processamento de texto:

1.  **Entrada do Usuário:** O usuário descreve seu estado emocional atual em algumas frases.
2.  **Análise Emocional:** O texto do usuário é enviado ao modelo Gemini com um prompt que o instrui a identificar o humor e sugerir características musicais associadas (gêneros, atmosfera, ritmo, etc.).
3.  **Simulação de Busca Musical:** O resultado da análise emocional é enviado novamente ao modelo Gemini com um prompt que o instrui a *simular* o comportamento de uma ferramenta de busca musical (como uma API de streaming). O modelo "inventa" uma lista de músicas que combinem com o humor e características sugeridas, retornando-a em um formato estruturado (JSON simulado). **Nesta versão, não há interação real com o Spotify ou qualquer outro serviço de streaming.**
4.  **Formatação da Playlist:** A lista de músicas simuladas (o JSON retornado pela etapa anterior) é enviada ao modelo Gemini com um prompt que o instrui a formatar essa lista em um texto amigável e envolvente, pronto para ser apresentado ao usuário como uma sugestão de playlist.
5.  **Saída:** O texto final formatado, contendo a playlist sugerida, é exibido no console do Google Colab.

## Configuração e Execução no Google Colab

Para rodar este projeto no seu próprio Google Colab:

1.  **Obtenha uma Gemini API Key:** Se você ainda não possui uma, crie sua chave gratuitamente no [Google AI Studio](https://aistudio.google.com/app/apikey).
2.  **Abra o Google Colab:** Acesse [colab.research.google.com](https://colab.research.google.com/) e crie um novo notebook (`Arquivo` -> `Novo notebook`).
3.  **Configure a API Key:** No Colab, clique no ícone de chave (🔑) na barra lateral esquerda para abrir "Secrets". Adicione um novo Secret com o nome `GOOGLE_API_KEY` e cole o valor da sua chave API neste campo.
4.  **Copie o Código:** Copie todo o código Python do projeto (importações, configuração da API/cliente, funções auxiliares, funções dos "agentes" simulados e o bloco principal `if __name__ == '__main__':`) para as células do seu notebook Colab. É uma boa prática separar as seções lógicas em células diferentes.
5.  **Execute as Células em Ordem:**
    * A primeira célula contendo `%pip install -q google-genai`.
    * A célula com a configuração da API Key e `genai.Client()`.
    * As células contendo as definições das funções auxiliares (`to_markdown`, `get_model_response`).
    * As células contendo as definições das funções principais (`analisar_emocao_genai`, `simular_chamada_spotify_genai`, `formatar_playlist_genai`).
    * Finalmente, a célula com o bloco principal `if __name__ == '__main__':`.
6.  **Interaja com o Prompt:** Ao executar a última célula, o console solicitará que você descreva como se sente. Digite seu texto e pressione Enter.
7.  **Veja a Sugestão:** O sistema processará sua entrada através das funções e exibirá a playlist sugerida diretamente no output do Colab.

## Estrutura do Código

O código é organizado em torno de funções Python, onde cada uma orquestra uma chamada ao modelo Gemini para uma tarefa específica:

* **Setup Inicial:** Importações, configuração do ambiente e do cliente GenAI.
* `to_markdown(text)`: Função utilitária para formatar texto para exibição no Colab.
* `get_model_response(prompt_text, model_name)`: Função auxiliar para simplificar as chamadas `client.models.generate_content`.
* `analisar_emocao_genai(texto_usuario)`: Recebe o texto do usuário e retorna uma análise formatada do humor/características musicais.
* `simular_chamada_spotify_genai(analise_emocional_str)`: Recebe a análise do humor e retorna uma string JSON simulando a resposta de uma API de busca musical.
* `formatar_playlist_genai(simulated_suggestions_json_str)`: Recebe a string JSON simulada e retorna um texto amigável com a playlist sugerida.
* `if __name__ == '__main__':`: Orquestra a sequência de chamadas das funções e lida com a interação de entrada/saída do usuário.

## Nota sobre o Uso do Google ADK

Inicialmente, este projeto foi concebido utilizando o framework Google Agent Development Kit (ADK) para a orquestração dos "agentes" e a definição de "ferramentas" customizadas. No entanto, durante o desenvolvimento no ambiente Google Colab, enfrentamos problemas persistentes de compatibilidade relacionados à instalação e importação de componentes do `google.adk.tools` (como a classe `Tool`).

Esses problemas manifestaram-se através de `ImportError` e ciclos contínuos de solicitação de reinício do ambiente, aparentemente causados por conflitos de dependência severos entre as versões do `google-adk` disponíveis e as bibliotecas pré-instaladas no ambiente padrão do Colab no momento do desenvolvimento.

Como não foi possível contornar esses problemas de ambiente de forma prática e rápida, optamos por implementar a lógica da pipeline de forma mais direta, utilizando apenas a biblioteca principal `google-genai` e orquestrando as chamadas ao modelo manualmente no código Python.

Portanto, a versão atual deste projeto reflete essa adaptação e não utiliza o framework Google ADK, focando na demonstração das capacidades do modelo Gemini e na lógica da pipeline de forma procedural em Python. O Google ADK continua sendo um framework poderoso para orquestração de agentes em ambientes compatíveis.

## Possíveis Aprimoramentos Futuros

Este projeto é um ponto de partida. Algumas ideias para expandi-lo incluem:

* **Integração Real com APIs de Streaming:** Substituir a lógica de simulação na função `simular_chamada_spotify_genai` por chamadas reais à [Spotify Web API](https://developer.spotify.com/documentation/web-api), API do TIDAL ou outras. Isso envolveria implementar autenticação OAuth 2.0 e usar bibliotecas como `requests` ou `spotipy`.
* **Análise de Áudio/Voz:** Explorar o uso de modelos para analisar o tom de voz do usuário e inferir o estado emocional a partir de áudio.
* **Considerar Histórico do Usuário:** Integrar com a API real para acessar o histórico de audição ou preferências do usuário e refinar as sugestões.
* **Criar a Playlist Diretamente:** Usar a API real para criar a playlist sugerida diretamente na conta do usuário.
* **Interface Gráfica:** Desenvolver uma interface web ou mobile mais interativa.

## Contribuições

Este código é um exemplo educacional. Sinta-se à vontade para usá-lo, modificá-lo e adaptá-lo para seus próprios projetos e aprendizados em IA generativa!


## Autor

Felipe Grisi Correia Pontes
