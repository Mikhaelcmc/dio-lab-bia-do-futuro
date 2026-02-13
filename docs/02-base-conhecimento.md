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

Aqui para garantirmos contexto ao nosso agente foram injetados os dados no prompt, lembrando que, para soluções mais robustas o ideal é que essas informações sejam obtidas dinamicamente para garantir a flexibilidade em tempo real.

```text

DADOS E PERFIL DO CLIENTE:

{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}


HISTÓRICO DO CLIENTE:

data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

TRANSAÇÕES DO CLIENTE:
data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida

PRODUTOS FINANCEIROS DISPONIVEIS:

[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Imobiliário(FII)",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "distribuição de dividendos (Dividend Yield), gira historicamente em torno de 10% a 12% ao ano ",
    "aporte_minimo":  100,
    "indicado_para": "moderado ou arrojado"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]

```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Resumo de gastos:
- Alimentação: R$ 1.550,00
- Saúde: R$ 120,00
- Moradia :R$ 160,00
- Transporte: R$ 180,00
- Lazer: R$ 16,00
- Educação: R$ 150,00
- Total de saidas: R$ 2.176,00

Produtos disponíveis para aplicar:
- Tesouro selic (risco baixo)
- Mercado de ações (risco alto)
- Fundo Imobiliario (risco médio)
- LCI/LCA (risco baixo)
- CDB liquidez diária (risco baixo)
...
```
