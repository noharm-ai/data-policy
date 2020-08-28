# Política de Dados NoHarm.ai

Este documento descreve os dados utilizados pela NoHarm.ai, o fluxos de dados na infraestrutura do hospital, o fluxo de dados na infraestrura da NoHarm.ai e as certificações de segurança. A NoHarm.ai mantém a sua infraestrura completa na Amazon Web Services (AWS).

## Dados Utilizados

Nesta seção separamos os dados utilizados pela NoHarm.ai como Controladora (quem competem as decisões referentes ao tratamento de dados pessoais) e Operadora (realiza o tratamento de dados pessoais em nome do controlador, nesse caso, o hospital)

### Controladora

Como controladora, a NoHarm.ai armazena as dados pessoais das farmacêuticas como: nome, email, senha e número da usuária no sistema do hospital. Outro dado pessoal fornecido pelo hospital é o nome do prescritor.

### Operadora

Como operadora, a NoHarm.ai armazena as informações do prontuário eletrônico listadas abaixo:
 - Setores: nome do setor
 - Medicamentos: nome do medicamento
 - Paciente: número do atendimento, data de nascimento, data de internação, cor, sexo, peso e altura
 - Prescrição: número da prescrição, número do atendimento, data, leito, prontuário, prescritor
 - Ítem da Prescrição: medicamento, dose, frequência, via, horário, complemento
 - Exames: número do atendimento, tipo, data, resultado

## Fluxo Interno (Hospital)

Na intranet do hospital é instalado uma máquina virtual com o software Apache Nifi com acesso à visões do banco de dados restritas aos dados listados acima. O Apache Nifi é repsonsável por transformar os dados, dentro da intranet do hospital, e enviá-los para o banco de dados da NoHarm.ai no serviço RDS da AWS.

### Serviço de Resolução de Nomes

O sistema da NoHarm.ai não armazena na base de dados os nomes dos pacientes por conta da privacidade dos dados e pela Lei Geral de Proteção de Dados (LGPD). Para que a NoHarm.ai resolva os nomes dos paciente é exposto um serviço, através do protocolo SSL, na intranet do hospital. Assim, somente dentro da intranet do hospital, a NoHarm.ai resolve os nomes do lado do cliente (client-side).

## Fluxo Externo (NoHarm.ai)

Os dados armazenados na infraestrutura da NoHarm.ai trafegam em três ambientes:
 - RDS: Serviço de Banco de Dados da AWS;
 - Backup: Serviço de contingência de dados da AWS;
 - Lambda: Serviço de API que gerencia a troca de dados com a interface no navegador da farmacêutica (usuária) através de credenciais específicas de cada usuária para cada hospital;
 
## Certificações de Segurança (AWS)

A Amazon Web Services matém controles robustos para manter a segurança e a proteção de dados. A AWS tem certificaçes para Controles de gerenciamento de segurança, Controles específicos da nuvem, Relatório de segurança, disponibilidade e confidencialidade, entre outros. 

Mais informaçes: https://aws.amazon.com/pt/compliance/programs/

## Glossário LGPD (Serpro)

 - https://www.serpro.gov.br/lgpd/menu/a-lgpd/glossario-lgpd
