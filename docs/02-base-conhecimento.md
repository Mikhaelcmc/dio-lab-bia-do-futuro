# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Para que serve no Felanças? |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente e usar essas informações de forma didática |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

O fundo "Multimercado" foi substituido pelo "Imobiliário" pois me sinto mais confortável sobre o assunto

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.



pode ser injetado no prompt ou injetado via codigo, como no exemplo abaixo:
```python
import pandas as pd
import json


df_historico = pd.read_csv('historico_atendimento.csv', sep=',', encoding='utf-8')
df_transacoes = pd.read_csv('transacoes.csv', sep=',', encoding='utf-8')


with open('perfil_investidor.json', 'r', encoding='utf-8') as f:
    df_perfil = pd.DataFrame(json.load(f))


with open('produtos_financeiros.json', 'r', encoding='utf-8') as f:
    df_produtos = pd.DataFrame(json.load(f))


```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

```text

DADOS E PERFIL DO CLIENTE:


HISTÓRICO DO CLIENTE:

TRANSAÇÕES DO CLIENTE:

PRODUTOS FINANCEIROS DISPONIVEIS:

```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
