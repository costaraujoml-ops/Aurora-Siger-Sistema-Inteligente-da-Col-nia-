# Aurora Siger: Sistema Inteligente da Colônia

Sistema em Python que simula o funcionamento inteligente de uma colônia em Marte.
Desenvolvido para a atividade **"A Aurora Estabelece os Primeiros Sistemas da Colônia"** (Fase 3, Cap. 1).

**Grupo 27** | Integrante: Maria Luiza Costa Araujo

---

## O que o sistema faz

A colônia está em operação contínua e, por causa da latência de comunicação entre Marte e a Terra, precisa tomar decisões de forma autônoma. O sistema segue o fluxo **entrada → processamento → saída** e reúne quatro partes:

1. **Organização dos dados:** as informações da colônia ficam em um dicionário aninhado (estrutura chave-valor com hierarquia): energia (solar e eólica), baterias, ambiente, módulos operacionais e histórico de leituras.
2. **Regras de decisão:** condições com `if/elif/else` combinam energia da bateria, consumo e previsão para gerar uma ação clara. O **suporte à vida nunca é desligado**: em caso de economia, são desligados primeiro os módulos não essenciais menos prioritários.
3. **Previsão:** regressão linear simples (mínimos quadrados) estima a energia eólica a partir da velocidade do vento, usando dados históricos.
4. **Análise de energia:** compara geração e consumo e sugere uma ação (alerta ou armazenamento do excedente).

---

## Regras de decisão

Limites adotados pelo grupo (podem ser ajustados no código):

- **Energia baixa:** carga da bateria < 50
- **Consumo alto:** consumo ≥ 60

| Condição | Saída |
|---|---|
| energia baixa **E** consumo alto **E** previsão desfavorável | EMERGÊNCIA: ativar modo de emergência |
| energia baixa **E** consumo alto | ALERTA: reduzir consumo |
| apenas energia baixa | ATENÇÃO: ativar modo economia |
| nenhuma das anteriores | OK: manter sistemas normais |

A **previsão é desfavorável** quando há tempestade de areia ou quando a geração prevista (solar + eólica prevista) é menor que o consumo.

---

## Modelo de previsão

Regressão linear simples: **energia = b0 + b1 · vento**

- b1 = Σ(x − x̄)(y − ȳ) / Σ(x − x̄)²
- b0 = ȳ − b1 · x̄

Com o histórico `vento = [8, 10, 12]` e `energia = [20, 25, 30]`, o modelo resulta em `energia = 0 + 2,5 · vento`. Para `vento = 11`, a previsão é 27,5, que corresponde ao valor aproximado de 27 esperado no enunciado.

O sistema também calcula o **R²** para indicar a qualidade do ajuste.

---

## Como executar

**No Google Colab (recomendado):**
1. Abra o notebook `.ipynb` deste repositório no Google Colab.
2. Vá em **Ambiente de execução → Executar tudo**.
3. Confira a saída do bloco de testes: `Todos os testes passaram!`

**Localmente (Python 3):** baixe o notebook como `.py` e execute:

```
python aurora_siger.py
```

Não são necessárias bibliotecas externas (apenas Python puro).

---

## Exemplos de entrada e saída

| Entrada | Saída |
|---|---|
| energia = 40, consumo = 70 | ALERTA: reduzir consumo |
| geração = 40, consumo = 70 | ALERTA: consumo maior que geração |
| geração = 80, consumo = 30 | SUGESTÃO: armazenar energia excedente |
| vento = 11 | previsão de energia ≈ 27 (modelo: 27,5) |

**Exemplo de execução completa do sistema** (bateria = 40, geração = 40, consumo = 70, vento previsto = 11):

```
=== RELATÓRIO DA COLÔNIA AURORA SIGER ===
Bateria: 40 | Geração: 40 | Consumo: 70
Vento previsto: 11 -> energia eólica prevista: 27.5
Geração prevista total: 42.5
Previsão desfavorável: True
Decisão: EMERGÊNCIA: ativar modo de emergência
Análise de energia: ALERTA: consumo maior que geração
Suporte à vida: MANTIDO LIGADO
Desligar sistemas não essenciais: ['iluminacao_externa', 'laboratorio']
Consumo após economia: 40
```

---

## Estrutura do código

| Parte | Função |
|---|---|
| `colonia` | Dados da colônia (dicionário aninhado) |
| `obter_geracao_total`, `obter_consumo_total` | Consulta aos dados |
| `exibir_hierarquia` | Navegação recursiva pelos subsistemas |
| `decidir_acao` | Regras de decisão |
| `sistemas_nao_essenciais`, `aplicar_modo_economia` | Priorização de sistemas |
| `analisar_energia` | Comparação entre geração e consumo |
| `regressao_linear`, `prever`, `calcular_r2` | Modelo de previsão |
| `executar_sistema` | Integra tudo (entrada → processamento → saída) |
| `modo_interativo` | Entrada de dados pelo usuário, com validação |

---

## Conceitos aplicados

- Estruturas de dados: dicionários, listas e hierarquia
- Lógica condicional e expressões booleanas (AND)
- Subalgoritmos: funções, parâmetros e recursão
- Regressão linear (mínimos quadrados)
- Fluxo entrada → processamento → saída
