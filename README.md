DEPLOY DE UMA APLICAÇÃO JAVA NO ELASTIC BEANSTALK

Essa aplicação irá expor uma API que permite visualizar ou remover carros de uma database

BUILDANDO A APLICAÇÃO
Nessa primeira etapa foi criado um arquivo.gitlab-yml para a criação de uma pipeline. No estágio de build são gerados artefatos. 

SMOKE TEST
O smoke test é um teste bem simples para testar se algo está funcionando
Nesse teste iremos chamar um endpoint. Um Curl será usado para ver se a aplicação responde.

Para isso, iremos subir a aplicação através do artefato gerado no stage anterior. Como a aplicação demora um pouco para inicializar foi adicionado um sleep de 30 segundos e então o script executa o curl e verifica se existe o status "UP". 

Agora iremos fazer o deploy da apicação no Elastic Beanstalk.
- Primeiramente é preciso criar um bucket no S3 para armazenar o artefato gerado no build.
- O nome do bucket foi adicionado como variável no gitlab
- Credenciais também foram adicionados como variáveis
- O ambiente em que ficará hospedado o aplicativo precisa já estar criado para executar esse script.

Deploy
A imagem usada já possui o AWS CLI. Foi definido também que não deve haver nenhum entrypoint. Os comando executados pelo script são para simplesmente copiar o artefact para o bucket criado anteriormente.

O próximo passo é fazer o deploy da aplicação presente no bucket do s3. Para isso seguir 2 passos, o primeiro é criar um novo application-version e depois fazer atualização do ambiente.
Uma variável para identificar a versão do pipeline foi criada.

Nesse novo update do código iremos atualizar informações um endpoint da aplicação. Para isso vamos substituir alguns dados com informações geradas durante o processo de build.

Verificando a versão da aplicação depois do deployment
A verificação será feita através de um curl para o domínio da aplicação. Para obter o domínio é necessário instalar o pacote jq para extrair esse domínio do json gerado no processo de build.

- CODE QUALITY
Nesse novo update foi criado um novo estágio para a verificar a qualidade do código. O PMD foi a ferramenta utilizada para esse prpósito. O que estamos interessados em obter são os reports gerados por essa ferramenta.

- UNIT TEST
Unit tests também foram adicionados ao pipeline. Esses testes sgeralmente são escritp pelo próprio desenvolvedor para detectar erros de programação e garantir a qualidade do código. Esse estágio é implementado ao pipeline de forma semelhante ao estágio de code quality.