# Requisitos – Software de Mapeamento e Automação de Processos (RFP)

## 1. Introdução

Este documento define os requisitos que a empresa exige de um software de mapeamento e automação de processos (BPM/Workflow) e serve de base para a RFP e a comparação entre fornecedores. Os requisitos são neutros: não citam nenhum produto.

**Escopo.** Modelagem de processos que cruzam várias áreas, execução automatizada (workflow) com idas e voltas entre áreas, formulários eletrônicos vinculados às atividades, controle de acesso por área, tarefas de aprovação/rejeição e relatórios exportáveis em Excel. Ficam fora do escopo: ERP, RPA e gestão eletrônica de documentos completa (apenas anexos ao processo).

**Convenções.**

- **ID:** RF = requisito funcional principal; RC = requisito funcional complementar; RNF = requisito não funcional.

- **Prioridade (MoSCoW):** **M** (Must) = obrigatório, ausência elimina o fornecedor; **S** (Should) = desejável, pesa na nota; **C** (Could) = opcional, diferencial; **W** (Won't) = fora do escopo desta contratação.

- **Critério de aceite:** condição verificável em demonstração ou prova de conceito (POC).

- **Atendimento:** o fornecedor responde cada requisito como Nativo (N), Configuração (C), Customização/desenvolvimento (D) ou Não atende (X).

**Glossário.**

| Termo                 | Definição                                                                        |
|-----------------------|----------------------------------------------------------------------------------|
| Processo              | Sequência de atividades que gera um resultado para um cliente interno ou externo |
| Instância             | Uma execução específica do processo (ex.: a solicitação nº 2026-0153)            |
| Atividade             | Etapa do processo executada por uma pessoa, área ou sistema                      |
| Raia (lane)           | Faixa do diagrama que representa a área responsável pelas atividades             |
| Formulário            | Tela de captura de dados preenchida durante a execução                           |
| Retorno (ida e volta) | Devolução da instância a uma atividade anterior, da mesma ou de outra área       |
| BPMN 2.0              | Notação padrão de mercado para desenho de processos                              |
| SLA                   | Prazo máximo acordado para concluir uma atividade ou o processo                  |

## 2. Requisitos funcionais principais

Os 33 requisitos abaixo cobrem as seis necessidades centrais; 24 são obrigatórios (M) e eliminam o fornecedor que não os atender.

### 2.1 Modelagem de processos multiárea

| ID    | Requisito                                                                                                             | Prior. | Critério de aceite                                                                |
|-------|-----------------------------------------------------------------------------------------------------------------------|--------|-----------------------------------------------------------------------------------|
| RF-01 | Desenhar processos em editor gráfico (arrastar e soltar) na notação BPMN 2.0                                          | M      | Modelar ao vivo um processo com início, 5 atividades, 1 gateway e fim, sem código |
| RF-02 | Representar cada área como raia (pool/lane), com atividades passando entre raias                                      | M      | Processo com no mínimo 4 áreas desenhado e executado de ponta a ponta             |
| RF-03 | Usar gateways exclusivos, paralelos e inclusivos para decisões e caminhos simultâneos                                 | M      | Instância segue o caminho correto conforme valor de um campo do formulário        |
| RF-04 | Permitir subprocessos reutilizáveis chamados por vários processos                                                     | S      | Subprocesso chamado por 2 processos diferentes, alterado em um único lugar        |
| RF-05 | Documentar cada atividade (descrição, responsável, entradas, saídas, SLA) e gerar o manual do processo em PDF ou Word | S      | Manual gerado automaticamente a partir do modelo                                  |
| RF-06 | Importar e exportar modelos em BPMN 2.0 (XML) e exportar imagem (PNG/SVG/PDF)                                         | S      | Modelo exportado e reimportado sem perda de elementos                             |
| RF-07 | Validar o modelo antes de publicar (atividades sem responsável, caminhos sem fim, gateways sem regra)                 | M      | Publicação bloqueada com mensagem indicando o erro                                |
| RF-08 | Simular o processo (tempos, volumes, gargalos) antes de publicar                                                      | C      | Simulação com 100 instâncias exibindo tempo médio por atividade                   |

### 2.2 Fluxo de ida e volta entre áreas

| ID    | Requisito                                                                                                         | Prior. | Critério de aceite                                                            |
|-------|-------------------------------------------------------------------------------------------------------------------|--------|-------------------------------------------------------------------------------|
| RF-09 | Devolver a instância a qualquer atividade anterior definida no modelo, inclusive de outra área                    | M      | Área C devolve para Área A; Área A corrige e reenvia; fluxo segue normalmente |
| RF-10 | Exigir justificativa obrigatória em toda devolução                                                                | M      | Devolução sem justificativa é bloqueada                                       |
| RF-11 | Ao reenviar, permitir que o processo volte direto para a atividade que devolveu, sem repetir etapas já concluídas | S      | Reenvio pula as etapas intermediárias quando configurado                      |
| RF-12 | Limitar o número de devoluções por instância e escalar ao gestor quando o limite for atingido                     | C      | Na 3ª devolução o gestor é notificado                                         |
| RF-13 | Manter histórico completo de cada ida e volta (quem, quando, de onde, para onde, motivo)                          | M      | Histórico exibido na instância e exportável                                   |

### 2.3 Formulários associados a atividades e processos

| ID    | Requisito                                                                                                                      | Prior. | Critério de aceite                                             |
|-------|--------------------------------------------------------------------------------------------------------------------------------|--------|----------------------------------------------------------------|
| RF-14 | Criar formulários sem programação, com campos texto, número, moeda, data, lista, caixa de seleção, anexo e tabela (grade)      | M      | Formulário com os 8 tipos de campo criado em demonstração      |
| RF-15 | Associar o mesmo formulário ao processo inteiro e definir, por atividade, quais campos são visíveis, editáveis ou obrigatórios | M      | Campo editável na atividade 1 e somente leitura na atividade 3 |
| RF-16 | Associar formulários diferentes a atividades diferentes do mesmo processo                                                      | M      | Processo com 2 formulários distintos executado                 |
| RF-17 | Validar campos (obrigatoriedade, formato, CPF/CNPJ, faixa de valores, regras entre campos)                                     | M      | Envio bloqueado com campo inválido                             |
| RF-18 | Preencher campos automaticamente (usuário, área, data, número sequencial) e por consulta a outros sistemas                     | S      | Dados do solicitante preenchidos sem digitação                 |
| RF-19 | Usar valores do formulário nas regras do fluxo (ex.: valor acima de R\$ 50 mil vai para a diretoria)                           | M      | Roteamento muda conforme valor informado                       |
| RF-20 | Versionar formulários sem afetar instâncias em andamento                                                                       | M      | Instâncias antigas mantêm a versão original                    |

### 2.4 Controle de acesso por área

| ID    | Requisito                                                                             | Prior. | Critério de aceite                                       |
|-------|---------------------------------------------------------------------------------------|--------|----------------------------------------------------------|
| RF-21 | Cadastrar estrutura organizacional (empresa, área, cargo) e grupos de usuários        | M      | Estrutura com 2 empresas e 5 áreas cadastrada            |
| RF-22 | Atribuir atividades a área, cargo, grupo ou pessoa, e não apenas a nomes individuais  | M      | Atividade atribuída a área aparece para todos os membros |
| RF-23 | Restringir quem inicia, executa, consulta e administra cada processo, por área        | M      | Usuário de outra área não visualiza instâncias restritas |
| RF-24 | Restringir visualização e edição de campos sensíveis do formulário por perfil         | M      | Campo "salário" oculto para perfis não autorizados       |
| RF-25 | Separar perfis de modelador, administrador, executor e consulta                       | M      | Executor não consegue alterar o modelo                   |
| RF-26 | Permitir delegação temporária de tarefas (férias, ausência), com data de início e fim | S      | Tarefas redirecionadas ao substituto no período          |

### 2.5 Tarefas de aprovação e rejeição

| ID    | Requisito                                                                                   | Prior. | Critério de aceite                             |
|-------|---------------------------------------------------------------------------------------------|--------|------------------------------------------------|
| RF-27 | Configurar atividades de decisão com as opções Aprovar, Rejeitar e Devolver para ajuste     | M      | Cada opção segue um caminho diferente no fluxo |
| RF-28 | Exigir justificativa na rejeição e na devolução                                             | M      | Rejeição sem comentário é bloqueada            |
| RF-29 | Aprovação em série (alçadas), em paralelo e por quórum (ex.: 2 de 3)                        | M      | Alçada por valor com 3 níveis funcionando      |
| RF-30 | Aprovar pela caixa de tarefas, por e-mail ou pelo celular                                   | S      | Aprovação concluída pelo smartphone            |
| RF-31 | Registrar cada decisão com usuário, data, hora e comentário, sem possibilidade de alteração | M      | Registro aparece na trilha de auditoria        |

### 2.6 Relatórios em Excel

| ID    | Requisito                                                                                                                                                    | Prior. | Critério de aceite                                                                          |
|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|---------------------------------------------------------------------------------------------|
| RF-32 | Exportar para Excel (.xlsx) os dados preenchidos nos formulários, com uma linha por instância e uma coluna por campo, incluindo status, datas e responsáveis | M      | Exportação de 1.000 instâncias abre no Excel com colunas corretas e sem formatação quebrada |
| RF-33 | Montar relatórios pelo usuário (escolher colunas, filtros por período, área, status e campo), salvar e reutilizar                                            | M      | Relatório salvo e reexecutado por usuário de negócio sem apoio de TI                        |

O controle de acesso (2.4) também vale para os relatórios: o usuário só exporta dados das áreas e campos que tem permissão de ver. Agendamento e envio automático de relatórios estão em RC-08.

## 3. Requisitos funcionais complementares

Estes requisitos não foram pedidos diretamente, mas são necessários para operar o processo no dia a dia e controlar prazos.

| ID    | Requisito                                                                                                             | Prior. | Critério de aceite                                                    |
|-------|-----------------------------------------------------------------------------------------------------------------------|--------|-----------------------------------------------------------------------|
| RC-01 | Caixa de tarefas única por usuário, com filtros por processo, prazo e prioridade                                      | M      | Usuário vê todas as pendências de todos os processos em uma tela      |
| RC-02 | Notificações por e-mail e no sistema ao receber, devolver, aprovar ou rejeitar uma tarefa, com texto configurável     | M      | E-mail enviado com link direto para a tarefa                          |
| RC-03 | Prazo (SLA) por atividade e por processo, com alerta antes do vencimento e escalonamento ao gestor após o vencimento  | M      | Tarefa vencida notifica o gestor da área                              |
| RC-04 | Considerar calendário de dias úteis e feriados no cálculo de prazos                                                   | S      | Prazo de 2 dias úteis iniciado na sexta vence na terça                |
| RC-05 | Trilha de auditoria imutável de todas as ações em instâncias, modelos, formulários e permissões                       | M      | Alteração de permissão aparece no log com usuário e data              |
| RC-06 | Versionamento de modelos de processo: publicar nova versão sem interromper instâncias em andamento e comparar versões | M      | Instâncias da v1 terminam na v1; novas iniciam na v2                  |
| RC-07 | Anexar documentos (PDF, imagens, Office) às instâncias, com controle de tamanho e tipo de arquivo                     | M      | Anexo de 20 MB incluído e visualizado                                 |
| RC-08 | Agendar relatórios e enviá-los por e-mail em Excel automaticamente                                                    | S      | Relatório semanal recebido toda segunda às 8h                         |
| RC-09 | Painéis (dashboards) com indicadores: volume, tempo médio por atividade, gargalos, % no prazo, % de devoluções        | S      | Painel por processo e por área, atualizado ao menos a cada hora       |
| RC-10 | Acompanhamento visual da instância, destacando no diagrama a atividade atual e o caminho percorrido                   | S      | Solicitante vê em que etapa está seu pedido                           |
| RC-11 | Reatribuir, cancelar ou suspender instâncias pelo administrador, com justificativa                                    | M      | Instância cancelada com motivo registrado                             |
| RC-12 | Atividades automáticas: envio de e-mail, geração de documento a partir de modelo e chamada a sistemas externos (API)  | S      | Ao aprovar, o sistema gera PDF e envia ao solicitante                 |
| RC-13 | Assinatura eletrônica nas aprovações                                                                                  | C      | Aprovação com assinatura eletrônica válida                            |
| RC-14 | Portal para iniciar processos por usuários externos (clientes, fornecedores)                                          | C      | Fornecedor abre solicitação sem licença interna                       |
| RC-15 | Ambientes separados de teste (homologação) e produção, com migração de modelos entre eles                             | S      | Processo testado em homologação e publicado em produção sem redesenho |

## 4. Requisitos não funcionais

Os valores numéricos abaixo (usuários, tempos, disponibilidade) são referências iniciais e devem ser ajustados ao volume real da empresa antes do envio da RFP.

| ID     | Categoria       | Requisito                                                                                                                                                                                         | Prior. | Critério de aceite                                         |
|--------|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------|
| RNF-01 | Segurança       | Login integrado ao diretório corporativo (Microsoft Entra ID/Active Directory) com SSO e MFA                                                                                                      | M      | Usuário acessa com a conta corporativa, sem senha separada |
| RNF-02 | Segurança       | Criptografia em trânsito (TLS 1.2 ou superior) e em repouso (AES-256)                                                                                                                             | M      | Evidência técnica ou relatório de auditoria do fornecedor  |
| RNF-03 | Segurança       | Política de senha, bloqueio por tentativas e expiração de sessão configuráveis                                                                                                                    | M      | Sessão encerrada após o tempo de inatividade definido      |
| RNF-04 | Segurança       | Certificações de segurança do fornecedor (ISO 27001, SOC 2 ou equivalente)                                                                                                                        | S      | Cópia do certificado vigente                               |
| RNF-05 | LGPD            | Atender à Lei 13.709/2018: mascaramento de dados pessoais, registro de acessos, exclusão ou anonimização sob demanda e dados armazenados no Brasil ou com cláusula de transferência internacional | M      | Demonstração de anonimização e cláusula contratual de LGPD |
| RNF-06 | Desempenho      | Telas e formulários carregam em até 3 segundos com 200 usuários simultâneos                                                                                                                       | M      | Teste de carga ou evidência de cliente de porte similar    |
| RNF-07 | Desempenho      | Exportação de 10.000 linhas para Excel em até 60 segundos                                                                                                                                         | S      | Medição na POC                                             |
| RNF-08 | Disponibilidade | Disponibilidade mínima de 99,5% ao mês em horário comercial, com SLA contratual e multa                                                                                                           | M      | Cláusula de SLA no contrato                                |
| RNF-09 | Continuidade    | Backup diário, retenção mínima de 30 dias, RPO ≤ 24 h e RTO ≤ 8 h                                                                                                                                 | M      | Plano de continuidade documentado                          |
| RNF-10 | Escalabilidade  | Suportar crescimento de usuários, processos e empresas do grupo sem troca de plataforma                                                                                                           | S      | Referência de clientes com volume 3 vezes maior            |
| RNF-11 | Integração      | APIs REST documentadas para iniciar processos, consultar instâncias e ler/gravar dados de formulários                                                                                             | M      | Documentação de API e chamada de teste na POC              |
| RNF-12 | Integração      | Conectores ou integração com o ERP corporativo, e-mail (Microsoft 365) e ferramentas de BI                                                                                                        | S      | Caso de integração com o ERP em uso                        |
| RNF-13 | Usabilidade     | Interface web em português, sem instalação local, compatível com Chrome, Edge e Firefox atuais                                                                                                    | M      | Acesso pelos 3 navegadores                                 |
| RNF-14 | Usabilidade     | Execução de tarefas e aprovações por celular (app ou web responsiva)                                                                                                                              | S      | Aprovação feita no celular na POC                          |
| RNF-15 | Acessibilidade  | Conformidade com WCAG 2.1 nível AA nas telas de execução                                                                                                                                          | C      | Declaração de conformidade                                 |
| RNF-16 | Implantação     | Modelo SaaS (nuvem) com opção de nuvem privada informada na proposta                                                                                                                              | S      | Arquitetura descrita na proposta                           |
| RNF-17 | Suporte         | Suporte em português, por chamado e telefone, com tempo de resposta ≤ 4 h para incidentes críticos                                                                                                | M      | Cláusula de suporte no contrato                            |
| RNF-18 | Treinamento     | Treinamento para modeladores, administradores e usuários finais, com material em português                                                                                                        | M      | Plano de treinamento na proposta                           |
| RNF-19 | Saída de dados  | Exportação completa de dados, modelos e anexos em formato aberto ao fim do contrato, sem custo adicional                                                                                          | M      | Cláusula de devolução de dados no contrato                 |
| RNF-20 | Licenciamento   | Modelo de licença transparente (por usuário nomeado, simultâneo ou por processo), com usuários apenas de consulta a custo reduzido                                                                | S      | Tabela de preços por perfil na proposta                    |

## 5. Avaliação dos fornecedores

Um fornecedor que não atenda a qualquer requisito M é eliminado; os demais são pontuados pela tabela de pesos abaixo.

**Instruções de resposta.** Para cada ID, o fornecedor informa o tipo de atendimento (N, C, D ou X), descreve como atende em até 5 linhas e indica onde comprovar (documentação, vídeo ou tela). Itens marcados D devem trazer esforço e custo estimados. Os requisitos M serão comprovados em demonstração roteirizada ou POC com um processo real da empresa.

**Pontuação por requisito.**

| Atendimento      | Pontos |
|------------------|--------|
| Nativo (N)       | 3      |
| Configuração (C) | 2      |
| Customização (D) | 1      |
| Não atende (X)   | 0      |

Os pontos de cada requisito são multiplicados pelo peso da prioridade: M = 3, S = 2, C = 1.

**Peso de cada critério na nota final.**

| Critério                                                       | Peso (%) |
|----------------------------------------------------------------|----------|
| Requisitos funcionais principais (RF)                          | 40       |
| Requisitos funcionais complementares (RC)                      | 15       |
| Requisitos não funcionais (RNF)                                | 15       |
| Custo total em 5 anos (licença, implantação, suporte)          | 20       |
| Fornecedor (referências, presença no Brasil, saúde financeira) | 10       |

**Pontos em aberto antes de enviar a RFP.**

- [ ] Confirmar o número de usuários por perfil (modelador, executor, consulta) e o volume anual de instâncias

- [ ] Definir o ERP e demais sistemas que precisam de integração (RNF-12)

- [ ] Escolher o processo-piloto que será usado na POC

- [ ] Validar com TI e Segurança da Informação os requisitos RNF-01 a RNF-05

- [ ] Validar os pesos da avaliação com Compras e patrocinador do projeto
