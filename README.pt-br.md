# n8n-delivery-weather-workflow

Um workflow N8N que consulta as condições climáticas antes de uma entrega e usa um passo de IA para classificar a viabilidade com raciocínio contextualizado.

<img width="1917" height="867" alt="image" src="https://github.com/user-attachments/assets/bc51ae1d-568d-49b7-aa77-7ba472844b30" />

## O Problema

Reagendar uma entrega na hora custa mais do que planejar antes. Quando você tem várias paradas no dia e o veículo não tem proteção contra chuva, uma chuva inesperada significa viagem perdida e cliente insatisfeito.

Esse workflow resolve isso antes da entrega sair.

## Como Funciona

1. O responsável pela entrega preenche um formulário com: cidade, data da entrega, período (manhã / tarde / noite) e tipo de produto.
2. O workflow chama a **OpenWeather API** para buscar os dados climáticos da cidade e data informadas.
3. Os dados climáticos são passados para um **passo de IA**, que analisa as condições no contexto: temperatura, umidade, velocidade do vento, cobertura de nuvens e previsão de precipitação.
4. O passo de IA retorna uma classificação estruturada:
   - **Viável** — condições aceitáveis para a entrega.
   - **Condicional** — algum risco, vale monitorar.
   - **Inviável** — risco alto, recomenda reagendar ou adaptar o veículo.
5. O resultado é apresentado de volta no formulário, com o raciocínio completo e um resumo das condições climáticas.
6. O humano toma a **decisão final**: aprovar, monitorar ou rejeitar.

O workflow aponta o risco. A pessoa decide.

## Exemplo de Saída

```
Classificação: Inviável

Previsão de chuva forte em São Paulo no dia 08/05 à tarde,
17°C, umidade de 95% e rajadas de vento de 8 m/s.
Risco alto para eletrônicos não impermeabilizados.

Recomendação: reagendar a entrega ou adaptar o veículo
para transporte protegido.
```

## Stack

- **N8N** — orquestração do workflow e interface de formulário
- **OpenWeather API** — dados climáticos por cidade e data
- **LLM (passo de IA)** — classificação contextual e geração de output estruturado

## Campos do Formulário

| Campo | Tipo |
|---|---|
| Cidade | Texto |
| Data da Entrega | Data |
| Período da Entrega | Seleção (Manhã / Tarde / Noite) |
| Produto | Texto |

## Por Que N8N

O N8N foi a escolha para construir esse MVP de forma ágil e eficiente — sem código de infraestrutura, sem boilerplate, só conectar as peças visualmente e iterar rápido.

Tornou simples conectar o formulário, a API de clima e o passo de IA em um único workflow. O fluxo visual também facilita ajustar o prompt de classificação ou trocar o provedor de clima futuramente.

A parte interessante foi ver o passo de IA adaptar o raciocínio para cada cenário específico, não apenas verificando limites numéricos, mas gerando uma recomendação baseada no tipo de produto e no período da entrega.

## Fluxo de Decisão

```
Entrada do Formulário
    ↓
OpenWeather API
    ↓
Passo de IA (classificar + raciocinar)
    ↓
Resultado Estruturado (classificação + raciocínio + resumo climático)
    ↓
Decisão Humana (aprovar / condicional / rejeitar)
```

## Observações

- A classificação do passo de IA é apenas uma recomendação. A decisão final sempre cabe ao responsável pela entrega.
- A precisão dos dados climáticos depende da cobertura da OpenWeather para a cidade selecionada.
- O prompt pode ser ajustado para adicionar regras específicas por tipo de produto (ex: limites mais restritivos para perecíveis ou eletrônicos).

## Form 

![Company Registration Form](https://github.com/user-attachments/assets/3d59c941-6067-4299-b323-81f5359bbd2c)

![Options Selection State](https://github.com/user-attachments/assets/ff5504a1-84dc-4c79-973f-8f7cf6c59e63)

![Final Result / Filled Form](https://github.com/user-attachments/assets/80cdc719-3633-4a2f-abcb-1e155fd951b5)
