# Política de Dados NoHarm.ai

Este documento descreve os dados utilizados pela NoHarm.ai, o fluxos de dados na infraestrutura do hospital, o fluxo de dados na infraestrura da NoHarm.ai e as certificações de segurança. A NoHarm.ai mantém a sua infraestrutura completa na Amazon Web Services (AWS).

## Dados Utilizados

Nesta seção separamos os dados utilizados pela NoHarm.ai como Controladora (quem competem as decisões referentes ao tratamento de dados pessoais) e Operadora (realiza o tratamento de dados pessoais em nome do controlador, nesse caso, o hospital)

### Controladora

Como controladora, a NoHarm.ai armazena as dados pessoais das farmacêuticas como: nome, email, senha e número da usuária no sistema do hospital. Outro dado pessoal fornecido pelo hospital é o nome do prescritor. Os dados coletados das farmacêuticas são necessários para que elas sejam identificadas nos relatórios de produtividade. O nome do prescritor é utilizado para que a farmacêutica possa entrar em contato, o que pode ser necessário em caso de dúvida ou anormalidade apontada pelo sistema da NoHarm.ai. Todos estes dados coletados tanto das farmacêuticas quanto dos prescritores ficam armazenados no banco de dados, hospedado na Amazon. Os dados são armazenados por tempo indeterminado, tendo em vista que o conjunto de informações se referem ao processo de trabalho da Farmácia Clínica.

### Operadora

Como operadora, a NoHarm.ai armazena as informações do prontuário eletrônico listadas abaixo:
 - Setores: nome do setor;
 - Medicamentos: nome do medicamento;
 - Paciente: número do atendimento, data de nascimento, data de internação, cor, sexo, peso e altura;
 - Prescrição: número da prescrição, número do atendimento, data, leito, prontuário, prescritor;
 - Ítem da Prescrição: medicamento, dose, frequência, via, horário, complemento;
 - Exames: número do atendimento, tipo, data, resultado;

Os dados são coletados para que as farmacêuticas possam fazer a avaliação da prescrição com mais qualidade, levando em consideração todo o quadro clínico do paciente. Todos os dados ficam armazenados no banco de dados, hospedado na Amazon. Os dados são armazenados por tempo indeterminado, tendo em vista que o conjunto de informações se referem ao histórico clínico dos pacientes.

## Direitos dos Titulares

Os titulares de dados coletados pela NoHarm.ai na condição de Controladora podem exercer seus direitos em contato com o Encarregado de Proteção de Dados, Henrique Dias, no e-mail henrique@noharm.ai. Caso você tenha sido paciente em algum hospital no qual prestamos serviço não teremos como responder às suas solicitações, pois os dados que coletamos são paseudonimizados, ou seja, não sabemos a quem pertence.

## Compartilhamento de Dados

A NoHarm.ai compartilha dados pessoais anonimizados com grupos de pesquisa para a realização de estudos, de acordo com o Art. 7º Inciso IV da Lei Geral de Proteção de Dados Pessoais. Na realização de estudos em saúde pública, os órgãos de pesquisa poderão ter acesso a bases de dados, que serão tratados exclusivamente dentro do órgão e estritamente para a finalidade de realização de estudos e pesquisas e mantidos em ambiente controlado e seguro, conforme práticas de segurança previstas em regulamento específico e que incluam a anonimização dos dados, bem como considerem os devidos padrões éticos relacionados a estudos e pesquisas.

## Fluxo Interno (Hospital)

Na intranet do hospital é instalado uma máquina virtual com o software Apache Nifi com acesso à visões do banco de dados restritas aos dados listados acima. O Apache Nifi é repsonsável por transformar os dados, dentro da intranet do hospital, e enviá-los para o banco de dados da NoHarm.ai no serviço RDS da AWS.

### Serviço de Resolução de Nomes

O sistema da NoHarm.ai não armazena na base de dados os nomes dos pacientes por conta da privacidade dos dados e pela Lei Geral de Proteção de Dados (LGPD). Para que a NoHarm.ai resolva os nomes dos paciente é exposto um serviço, através do protocolo SSL, na intranet do hospital. Assim, somente dentro da intranet do hospital, a NoHarm.ai resolve os nomes do lado do cliente (client-side).

## Fluxo Externo (NoHarm.ai)

Os dados armazenados na infraestrutura da NoHarm.ai trafegam em três ambientes:
 - RDS: Serviço de Banco de Dados da AWS;
 - Backup: Serviço de contingência de dados da AWS;
 - Lambda: Serviço de API que gerencia a troca de dados com a interface no navegador da farmacêutica (usuária) através de credenciais específicas de cada usuária para cada hospital;

Todas as comunicações entre o Apache Nifi, serviços AWS e clientes são feitas de forma segura utilizando protocolo SSL/TLS para encriptar os dados.

Todos os serviços acimas são qualificados pela Health Insurance Portability and Accountability Act (HIPAA, Lei de Portabilidade e Responsabilidade de Seguro de Saúde) dos EUA. 

Mais informações: https://aws.amazon.com/pt/health/healthcare-compliance/
 
## Relatório de Impacto de Proteção de Dados (RIPD)
 
A NoHarm desenvolve todos os anos um Relatório de Impacto de Proteção de Dados e compartilha o resultado desse estudo interno com os Hospitais que utilizam a ferramenta.
 
## Certificações de Segurança (AWS)

A Amazon Web Services matém controles robustos para manter a segurança e a proteção de dados. A AWS tem certificaçes para Controles de gerenciamento de segurança, Controles específicos da nuvem, Relatório de segurança, disponibilidade e confidencialidade, entre outros. 

Mais informaçes: https://aws.amazon.com/pt/compliance/programs/

## Glossário LGPD (Serpro)

 - https://www.serpro.gov.br/lgpd/menu/a-lgpd/glossario-lgpd
 - http://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/L13709.htm (LGPD)
