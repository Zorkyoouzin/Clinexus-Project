# Clinexus — Objetivo

## Objetivo geral

Desenvolver o **Clinexus**: um sistema de apoio à gestão de pesquisas com animais de laboratório, acessível por **computador e celular**, para que pesquisadores registrem e consultem as anotações de rotina do laboratório (atualmente, pesquisas com ratos envolvendo análise comportamental diante de peptídeos).

## Objetivos específicos

1. **Ser útil no dia a dia:** apoiar tarefas recorrentes, como cadastro de animais, registro de protocolos e de sessões de testes comportamentais.
2. **Reduzir o trabalho manual** de registro e consulta, hoje feitos em papel ou planilhas.
3. **Garantir confiabilidade e rastreabilidade:** preservar a integridade dos dados e o histórico de cada animal e de cada protocolo, com registro de auditoria (quem alterou o quê e quando).
4. **Ser simples de usar:** interface objetiva, adequada ao registro rápido durante a rotina, inclusive em telas pequenas.
5. **Alinhar-se a boas práticas da área:** considerar o Princípio dos 3Rs (Replacement, Reduction, Refinement) e a documentação exigida por comissões de ética (CEUA), mantendo apenas campos de referência à aprovação ética, sem substituir os sistemas oficiais.

## Apoio ao Princípio dos 3Rs

O Clinexus não substitui a avaliação ética da pesquisa nem os sistemas oficiais, mas contribui para os 3Rs na rotina do laboratório:

- **Replacement (substituição):** o sistema não substitui o uso de animais, mas a consulta ao histórico e a pesquisa de registros (UC08, UC09) ajudam a verificar quais dados já existem antes de planejar um novo experimento.
- **Reduction (redução):** o protocolo registra o nº de animais previsto e o relatório (UC10) mostra quantos foram efetivamente utilizados, facilitando a conferência com o planejado; registros confiáveis também evitam repetir sessões por perda de dados.
- **Refinement (refinamento):** o histórico de procedimentos, tratamentos e observações de saúde (UC06) permite acompanhar o bem-estar de cada animal e identificar problemas precocemente.

## Escopo

**Dentro do escopo:** cadastro de animais, linhagens e grupos experimentais; protocolos experimentais; registro de eventos no histórico do animal (procedimentos, tratamentos, saúde); sessões de testes comportamentais com medidas e anexos; consultas e relatórios simples; login com perfis de acesso; auditoria básica.

**Fora do escopo:** substituição de sistemas oficiais de ética (ex.: CIUCA); análise estatística avançada dos resultados; gestão financeira ou de estoque do biotério.

## Plataforma

Aplicação web responsiva (uso em PC e celular) para as anotações de rotina dos pesquisadores, com banco de dados **PostgreSQL**.
