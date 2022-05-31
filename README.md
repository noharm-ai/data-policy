# Política de Dados NoHarm.ai
Este documento descreve os dados utilizados pela NoHarm.ai, o fluxo de dados na infraestrutura do hospital, o fluxo de dados na infraestrutura da NoHarm.ai e as certificações de segurança. A NoHarm.ai mantém a sua infraestrutura completa na Amazon Web Services (AWS).

## Dados Utilizados
Nesta seção separamos os dados utilizados pela NoHarm.ai como Controladora (a quem competem as decisões referentes ao tratamento de dados pessoais) e Operadora (quem realiza o tratamento de dados pessoais em nome do controlador, neste caso, o hospital)

### Controladora
Como controladora, a NoHarm.ai armazena os dados pessoais das farmacêuticas como: nome, email, senha e número identificador do sistema interno. Outro dado pessoal fornecido pelo hospital é o nome do prescritor. Os dados coletados das farmacêuticas são necessários para que elas sejam identificadas nos relatórios de produtividade. O nome do prescritor é utilizado para que a farmacêutica possa entrar em contato com o prescritor, o que pode ser necessário em caso de dúvida ou anormalidade apontada pelo sistema da NoHarm.ai. Todos estes dados coletados, tanto das farmacêuticas quanto dos prescritores, ficam armazenados no banco de dados, hospedado na Amazon. Os dados são armazenados por tempo indeterminado, tendo em vista que o conjunto de informações se referem ao processo de trabalho da Farmácia Clínica de cada hospital.

### Operadora
Como operadora, a NoHarm.ai armazena as informações do prontuário eletrônico listadas abaixo:
 - Setores: nome do setor;
 - Medicamentos: nome do medicamento;
 - Paciente: número do atendimento, data de nascimento, data de internação, cor, sexo, peso e altura;
 - Prescrição: número da prescrição, número do atendimento, data, leito, prontuário, prescritor;
 - Item da Prescrição: medicamento, dose, frequência, via, horário, complemento;
 - Exames: número do atendimento, tipo do exame, data, resultado;

Os dados são coletados para que as farmacêuticas possam fazer a avaliação das prescrições com mais qualidade, levando em consideração todo o quadro clínico do paciente. Todos os dados ficam armazenados no banco de dados, hospedado na Amazon. Os dados são armazenados por tempo indeterminado, tendo em vista que o conjunto de informações se referem ao histórico clínico dos pacientes.

## Arquitetura do fluxo de dados
A Figura abaixo representa a infraestrutura utilizada no fluxo de dados dentro do sistema NoHarm.
![Image of Infra](https://raw.githubusercontent.com/noharm-ai/data-policy/master/noharm-infra.png)


### Fluxo interno (Hospital) - Coleta e ingestão dos dados:
 - Tasy / MV: O cadastramento dos dados começa no Sistema de Gestão Hospitalar utilizado pelo hospital, onde é feito o cadastro do paciente e todas as movimentações relacionadas aos atendimentos, bem como a prescrição médica. Atualmente a NoHarm.ai tem integração com os sistemas Tasy e MV. 
 - Apache Nifi: Na intranet do hospital (rede corporativa) é instalada uma máquina virtual com o software Apache Nifi com acesso à visões do banco de dados restritas aos dados listados anteriormente. O Apache Nifi é responsável por transformar os dados, dentro da intranet do hospital (rede corporativa), e enviá-los para o banco de dados da NoHarm.ai no serviço RDS da AWS.
 - SRN (Serviço de Resolução de Nomes dos Pacientes): O sistema da NoHarm.ai não armazena na base de dados os nomes dos pacientes por conta da privacidade dos dados e pela Lei Geral de Proteção de Dados (LGPD). Para que a NoHarm.ai resolva os nomes dos pacientes é criado um serviço, na intranet do hospital, acessado através do protocolo SSL. Assim, somente dentro da intranet do hospital (rede corporativa), a NoHarm.ai resolve os nomes do lado do cliente (client-side).

### Ambiente AWS - Armazenamento e processamento dos dados:
Os dados armazenados na infraestrutura da NoHarm.ai trafegam em três ambientes:
 - RDS: Serviço de Banco de Dados Relacional da AWS;
 - Backup (EC2): Serviço de contingência de dados da AWS;
 - Lambda: Serviço de API que gerencia a troca de dados com a interface no navegador da farmacêutica (usuária) através de credenciais específicas de cada usuária para cada hospital;

Além disso, são utilizados os seguintes serviços complementares:
 - Route 53: serviço de DNS (Domain Name System) na nuvem.
 - CloudWatch: monitora os recursos, serviços e aplicações na nuvem
 - SNS (Simple Notification Service): serviço de mensagens 
 - S3 (Simple Storage Service): serviço para armazenamento de objetos.
 - API Gateway: serviço para gerenciar a criação, publicação, monitoramento e segurança das APIs.
 - CloudFront: serviço de rede de entrega rápida de conteúdo.

### Ambiente Google Analytics:
 - Analytics: ferramentas para análise dos dados de acesso ao site da NoHarm.ai.
 - Tag Manager: serviço de gerenciamento de tags para aplicativos móveis e sites. 
 - Data Studio: ferramentas para criar dashboards e relatórios.

### Ambiente de desenvolvimento:
 - GitHub: plataforma para hospedagem e versionamento do código fonte.
 - GitHub Actions: serviço de integração contínua de código usado na construção e no teste do código hospedado no GitHub.
 - updown.io: serviço para monitorar o status do site.

Todas as comunicações entre o Apache Nifi, serviços AWS e clientes são feitas de forma segura utilizando protocolo SSL/TLS para encriptar os dados.

Todos os serviços acima são qualificados pela Health Insurance Portability and Accountability Act (HIPAA, Lei de Portabilidade e Responsabilidade de Seguro de Saúde) dos EUA. 

Mais informações: https://aws.amazon.com/pt/health/healthcare-compliance/

## Gerenciamento dos riscos associados ao tratamento dos dados
A tabela abaixo apresenta os principais riscos relacionados aos tratamento de dados, e quais são as medidas que tomamos para evitar ou reduzir os riscos.

| ID  | Risco referente ao tratamento de dados                                                                                                                             | Medidas<br/>executadas                                                                                                                                                                                       |
|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1   | Acesso físico aos recursos computacionais dentro do hospital.                                                                                                      | 1. Contrato                                                                                                                                                                                                  |
| 2   | Acesso não autorizado ao banco de dados.                                                                                                                           | 1. Usuários diferentes por Hospital                                                                                                                                                                          |
| 3   | Acesso não autorizado à interface do sistema.                                                                                                                      |      1. Limite de requisições por chave<br/>     2. Controle de acesso lógico<br/>3. Timeout da sessão                                                                                                       |
| 4   | Sequestro de dados.                                                                                                                                                | 1. 2FA do Console da AWS                                                                                                                                                                                     |
| 5   | Compartilhar ou distribuir dados com terceiros sem o consentimento do titular dos dados.                                                                           | 1. Contrato entre a NoHarm e o cliente: possibilidade de compartilhamento com grupos de pesquisa.<br/>2. Treinamentos para os colaboradores em relação à segurança são responsabilidade do contratante.<br/> |
| 6   | Reidentificação de dados pseudoanonimizados. Vinculação/associação indevida, direta ou indireta, dos dados ao titular.                                                | 1. Dado pessoal (nome) só é resolvido dentro da rede do cliente mediante acesso autorizado.                                                                                                                  |
| 7   | Falha/erro de processamento (Ex.: execução de script de banco de dados que atualiza dado  com dado equivocado, ausência de validação dos dados de entrada, etc.).  | 1. Ambiente de testes com banco de dados de teste.<br/>3. Backups                                                                                                                                            |
| 8   | Modificação/Remoção não autorizada.                                                                                                                                | 1. Log das ações críticas do usuário de aplicação<br/>2. Cada usuário tem acesso específico para as ações autorizadas                                                                                        |
| 9   | Interceptação de pacotes.                                                                                                                                          | 1. Uso de Protocolo SSL em todas as comunicações                                                                                                                                                             |
| 10  | Falha em considerar os direitos do titular dos dados (Ex.: perda do direito de acesso).                                                                            | 1. Contrato entre a NoHarm e o cliente: a coleta do consentimento é responsabilidade do cliente. Consentimento deve informar sobre o procedimento para acesso aos dados.                                        |
| 11  | Retenção prolongada de dados sem necessidade.                                                                                                                      | 1. Especificação em contrato sobre tempo de retenção dos dados.                                                                                                                                              |
| 12  | Problemas na integração com o sistema de gestão hospitalar.                                                                                                        | 1. Validação da integração com especialistas do domínio.<br/>2. Monitoramento da integração.                                                                                                                 |
| 13  | Problemas de desempenho em operações no sistema.                                                                                                                   | 1. Monitoração do uso de recursos computacionais.<br/>2. Política de escalabilidade para gerenciamento dos recursos de acordo com a demanda.                                                                 |
| 14  | Injeção de dados via script.                                                                                                                                       | 1. Uso de bibliotecas de proteção                                                                                                                                                                            |
| 15  | Perda de dados.                                                                                                                                                    | 1. Backup por parte da NoHarm<br/>2. Backup por parte do cliente                                                                                                                                             |
| 16  | Coleção excessiva (aplicação)                                                                                                                                      | 1. Limitação da coleta na aplicação<br/>2. Limite de coleta nos Relatórios                                                                                                                                   |
| 17  | Informação insuficiente sobre a finalidade do tratamento.                                                                                                          | 1. Política de dados disponibilizada publicamente<br/>2. Contrato entre a NoHarm e o cliente                                                                                                                 |
| 18  | Tratamento sem consentimento do titular dos dados (Caso o tratamento não esteja previsto em legislação ou regulação pertinente).                                   | 1. Contrato entre a NoHarm e o cliente: coleta do consentimeto é responsabilidade do cliente.                                                                                                                |
| 19  | Análise incorreta dos dados por parte do sistema.                                                                                                                  | 1. Atualizações periódicas da aplicação de acordo com especialistas do domínio.<br/>2. Configuração dos parâmetros por parte do cliente.                                                                     |
| 20  | Análise incorreta dos dados por parte do usuário.                                                                                                                  | 1. Menu de ajuda.<br/>2. Treinamento do usuário.                                                                                                                                                             |
## Direitos dos Titulares
Os titulares de dados coletados pela NoHarm.ai na condição de Controladora podem exercer seus direitos em contato com o Encarregado de Proteção de Dados, Henrique Dias, no e-mail henrique@noharm.ai. Caso você tenha sido paciente em algum hospital no qual prestamos serviço não teremos como responder às suas solicitações, pois os dados que coletamos são pseudo-anonimizados, ou seja, não sabemos a quem pertence.
## Compartilhamento de Dados
Quando autorizada pelo cliente, a NoHarm.ai compartilha dados pseudo-anonimizados com grupos de pesquisa para a realização de estudos, de acordo com o Art. 7º Inciso IV da Lei Geral de Proteção de Dados Pessoais. Na realização de estudos em saúde pública, os órgãos de pesquisa poderão ter acesso a bases de dados, que serão tratados exclusivamente dentro do órgão e estritamente para a finalidade de realização de estudos e pesquisas e mantidos em ambiente controlado e seguro, conforme práticas de segurança previstas em regulamento específico e que incluam a anonimização dos dados, bem como consideram os devidos padrões éticos relacionados a estudos e pesquisas. 
## Certificações de Segurança (AWS)
A Amazon Web Services mantém controles robustos para manter a segurança e a proteção de dados. A AWS tem certificações para Controles de gerenciamento de segurança, Controles específicos da nuvem, Relatório de segurança, disponibilidade e confidencialidade, entre outros. 

Mais informaçes: https://aws.amazon.com/pt/compliance/programs/
## Glossário LGPD (Serpro)
 - https://www.serpro.gov.br/lgpd/menu/a-lgpd/glossario-lgpd
 - http://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/L13709.htm (LGPD)
