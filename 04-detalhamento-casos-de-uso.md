# Clinexus — Detalhamento dos Casos de Uso

Convenções: **PC** = pré-condição, **PÓS** = pós-condição, **FP** = fluxo principal, **FA** = fluxo alternativo, **RN** = regra de negócio. Salvo UC01, todos têm como PC o usuário autenticado. Toda criação, alteração ou inativação de dados é registrada na auditoria (RF20).

---
## UC01 — Autenticar
- **Ator:** Colaborador / Pesquisador Responsável
- **PC:** usuário cadastrado e ativo.
- **PÓS:** sessão iniciada com o perfil do usuário.
- **FP:**
  1. O usuário informa e-mail e senha.
  2. O sistema valida as credenciais.
  3. O sistema abre a sessão e exibe a tela inicial conforme o perfil.
- **FA1 — Credenciais inválidas:** o sistema exibe mensagem genérica e retorna ao passo 1.
- **FA2 — Usuário inativo:** o sistema nega o acesso.
- **RN:** senhas armazenadas com hash; mensagens de erro não revelam qual campo está incorreto.

## UC02 — Gerenciar usuários
- **Ator:** Pesquisador Responsável
- **PÓS:** usuário criado, alterado ou inativado.
- **FP:**
  1. O ator acessa a lista de usuários.
  2. Seleciona "Novo usuário" (ou um existente).
  3. Informa nome, e-mail, perfil e senha inicial.
  4. O sistema valida, salva e audita.
- **FA1 — E-mail já cadastrado:** o sistema rejeita e informa o conflito.
- **FA2 — Inativar usuário:** o sistema marca `ativo = falso` e audita; o histórico do usuário é preservado.
- **RN:** usuários não são excluídos fisicamente; o último Pesquisador Responsável ativo não pode ser inativado.

## UC03 — Gerenciar animais
- **Ator:** Colaborador
- **PÓS:** animal cadastrado ou atualizado.
- **FP:**
  1. O ator acessa "Animais" e escolhe "Novo animal".
  2. Informa identificação, linhagem, sexo, data de nascimento e origem.
  3. O sistema valida e salva.
  4. O sistema registra a operação na auditoria.
- **FA1 — Identificação duplicada:** o sistema rejeita o cadastro.
- **FA2 — Editar animal / alterar status:** o ator altera os dados; o sistema salva e audita.
- **RN:** identificação é única; animais com histórico não podem ser excluídos, apenas ter o status alterado (ativo, finalizado, baixa).

## UC04 — Gerenciar grupos experimentais
- **Ator:** Colaborador
- **PC:** protocolo existente.
- **PÓS:** grupo criado/alterado e animais vinculados ou desvinculados.
- **FP:**
  1. O ator abre um protocolo e escolhe "Novo grupo".
  2. Informa nome e tipo (controle, tratado, outro).
  3. Seleciona os animais do grupo.
  4. O sistema salva os vínculos com data de entrada e audita.
- **FA1 — Animal já está em outro grupo do mesmo protocolo:** o sistema bloqueia o vínculo.
- **FA2 — Desvincular animal:** o sistema registra a data de saída, sem apagar o vínculo, e audita.
- **RN:** um animal pertence a no máximo um grupo por protocolo ao mesmo tempo; nome do grupo é único dentro do protocolo.

## UC05 — Gerenciar protocolos
- **Ator:** Pesquisador Responsável
- **PÓS:** protocolo criado ou atualizado.
- **FP:**
  1. O ator acessa "Protocolos" e escolhe "Novo protocolo".
  2. Informa título, descrição, espécie, nº de animais previsto, nº CEUA (opcional), período e responsável.
  3. O sistema valida, salva e audita.
- **FA1 — Concluir/cancelar protocolo:** o sistema altera o status, audita e bloqueia novos registros de sessão nele.
- **RN:** o nº CEUA é apenas referência, sem validação junto a sistemas oficiais; data de fim não pode ser anterior à de início.

## UC06 — Registrar evento no histórico do animal
- **Ator:** Colaborador
- **PC:** animal cadastrado.
- **PÓS:** evento adicionado ao histórico do animal.
- **FP:**
  1. O ator abre o animal e escolhe "Novo evento".
  2. Seleciona o tipo (procedimento, tratamento ou saúde) e informa data/hora e descrição.
  3. Se for tratamento, informa substância, dose, unidade e via de administração.
  4. O sistema salva o evento vinculado ao usuário e audita.
- **FA1 — Campos de tratamento incompletos:** o sistema exige substância, dose, unidade e via de administração.
- **RN:** eventos não são apagados; correções geram alteração auditada.

## UC07 — Registrar sessão de teste
- **Ator:** Colaborador
- **PC:** protocolo em andamento; animal vinculado a um grupo desse protocolo; tipo de teste cadastrado.
- **PÓS:** sessão registrada com medidas e anexos.
- **FP:**
  1. O ator escolhe "Nova sessão".
  2. Seleciona protocolo, animal e tipo de teste; informa data/hora e observações.
  3. Informa as medidas (parâmetro, valor, unidade).
  4. Opcionalmente anexa arquivos.
  5. O sistema salva a sessão e audita.
- **FA1 — Animal fora do protocolo:** o sistema impede o registro.
- **FA2 — Anexo com formato não aceito ou acima de 100 MB:** o sistema rejeita o arquivo e mantém os demais dados.
- **RN:** a sessão pertence a um animal e a um protocolo; ao menos uma medida ou observação é obrigatória; anexos aceitos: PDF, PNG, JPG, MP4, XLSX e CSV, até 100 MB por arquivo.

## UC08 — Consultar histórico
- **Ator:** Colaborador
- **PÓS:** nenhuma alteração de dados.
- **FP:**
  1. O ator seleciona um animal ou grupo.
  2. O sistema exibe linha do tempo com eventos e sessões de teste.
  3. O ator pode abrir os detalhes de cada item.
- **FA1 — Sem registros:** o sistema informa que não há histórico.

## UC09 — Pesquisar registros
- **Ator:** Colaborador
- **FP:**
  1. O ator escolhe a entidade (animais, protocolos ou sessões).
  2. Informa filtros (período, grupo, tratamento, tipo de teste, status).
  3. O sistema lista os resultados paginados.
- **FA1 — Nenhum resultado:** o sistema exibe mensagem e sugere limpar os filtros.

## UC10 — Gerar relatório
- **Ator:** Colaborador
- **PC:** protocolo ou grupo existente.
- **PÓS:** relatório exibido e/ou exportado em PDF.
- **FP:**
  1. O ator seleciona protocolo ou grupo (opcionalmente após UC09).
  2. O sistema consolida animais, tratamentos e sessões e, para protocolos, compara o nº de animais utilizados com o previsto.
  3. O sistema exibe o resumo e oferece exportação em PDF.
- **FA1 — Sem dados no período:** o sistema gera relatório vazio com aviso.

## UC11 — Gerenciar tabelas de apoio
- **Ator:** Pesquisador Responsável
- **FP:**
  1. O ator escolhe a tabela (linhagens, substâncias ou tipos de teste).
  2. Cadastra, edita ou inativa itens.
  3. O sistema valida, salva e audita.
- **FA1 — Nome duplicado:** o sistema rejeita.
- **RN:** itens em uso não podem ser excluídos.

## UC12 — Consultar auditoria
- **Ator:** Pesquisador Responsável
- **FP:**
  1. O ator acessa "Auditoria".
  2. Filtra por usuário, entidade, ação ou período.
  3. O sistema exibe quem alterou o quê, quando, e os valores anterior e novo.
- **RN:** o log é somente leitura e não pode ser editado nem excluído.
