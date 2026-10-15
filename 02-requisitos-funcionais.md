# Clinexus — Requisitos Funcionais

**Perfis:** *Pesquisador Responsável* (acesso total) e *Colaborador* (registro e consulta).

## Acesso e usuários
| ID | Requisito | Prioridade |
|----|-----------|-----------|
| RF01 | O sistema deve permitir autenticação por e-mail e senha. | Alta |
| RF02 | O sistema deve controlar o acesso por perfil (Pesquisador Responsável e Colaborador). | Alta |
| RF03 | O Pesquisador Responsável deve poder cadastrar, editar e inativar usuários. | Alta |

## Animais
| ID | Requisito | Prioridade |
|----|-----------|-----------|
| RF04 | O sistema deve permitir cadastrar animais com identificação, linhagem/genótipo, sexo, data de nascimento e origem. | Alta |
| RF05 | O sistema deve permitir editar animais e alterar seu status (ativo, finalizado, baixa). | Alta |
| RF06 | O sistema deve permitir cadastrar e manter linhagens. | Média |
| RF07 | O sistema deve permitir registrar eventos no histórico do animal: procedimentos, tratamentos (incluindo substância, dose e via de administração) e observações de saúde. | Alta |
| RF08 | O sistema deve permitir cadastrar e manter substâncias (ex.: peptídeos). | Média |

## Protocolos e grupos
| ID | Requisito | Prioridade |
|----|-----------|-----------|
| RF09 | O sistema deve permitir cadastrar protocolos com título, descrição, espécie, nº de animais previsto, responsável, período e nº de protocolo CEUA (apenas referência). | Alta |
| RF10 | O sistema deve permitir criar grupos experimentais dentro de um protocolo (ex.: controle, tratado). | Alta |
| RF11 | O sistema deve permitir vincular e desvincular animais a grupos, mantendo o histórico de entrada e saída. | Alta |

## Testes comportamentais
| ID | Requisito | Prioridade |
|----|-----------|-----------|
| RF12 | O sistema deve permitir cadastrar tipos de teste comportamental. | Média |
| RF13 | O sistema deve permitir registrar sessões de teste associadas a um animal, a um protocolo e a um tipo de teste, com data/hora e observações. | Alta |
| RF14 | O sistema deve permitir registrar medidas (parâmetro, valor, unidade) em cada sessão. | Alta |
| RF15 | O sistema deve permitir anexar arquivos a uma sessão (ex.: planilhas, vídeos, imagens), nos formatos PDF, PNG, JPG, MP4, XLSX e CSV, com até 100 MB por arquivo. | Média |

## Consulta e relatórios
| ID | Requisito | Prioridade |
|----|-----------|-----------|
| RF16 | O sistema deve permitir consultar o histórico completo de um animal (eventos e sessões) ao longo do tempo. | Alta |
| RF17 | O sistema deve permitir consultar o histórico comportamental de um grupo. | Média |
| RF18 | O sistema deve permitir pesquisar animais, protocolos e sessões por filtros (data, grupo, tratamento, tipo de teste). | Alta |
| RF19 | O sistema deve gerar relatório-resumo de um protocolo ou grupo experimental (para protocolos, incluindo o nº de animais utilizados em relação ao previsto), exportável em PDF. | Média |

## Auditoria
| ID | Requisito | Prioridade |
|----|-----------|-----------|
| RF20 | O sistema deve registrar automaticamente quem criou, alterou ou inativou cada informação, e quando. | Alta |
| RF21 | O Pesquisador Responsável deve poder consultar o log de auditoria. | Média |

## Requisitos não funcionais (resumo)
- **Usabilidade:** interface responsiva (PC e celular), com fluxos curtos para as tarefas mais frequentes.
- **Segurança:** autenticação obrigatória, senhas armazenadas com hash, backups periódicos.
- **Confiabilidade:** consistência dos dados e preservação do histórico (sem exclusão física de registros de pesquisa).
- **Desempenho:** tempo de resposta adequado mesmo com crescimento do volume de dados.
