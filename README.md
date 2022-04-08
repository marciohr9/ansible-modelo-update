# Assistente em Ansible para atualização remota de projeto e serviços nos servidores

Código em ansible para atualização remota dos servidores da AGTEC usando como base para o pull dos projetos o [gitlab](http://git.palmas.to.gov.br/dti-desenvolvimento/). Atualmente o código está feito para suportar servidores Debian/Ubuntu com roles para serviços em docker e uvicorn (servidor web assincrono do Python).

## Requerimentos

1. Instale no computador que executará o script o _Ansible_ [link-para-instalação](https://docs.ansible.com/ansible/latest/installation_guide/index.html).
2. É ideal que você tenha salvo no pc local a chave privada de acesso ssh ao host que vai ser atualizado, caso não siga o passo-a-passo a baixo e troque para acesso com password.
3. Siga o passo-a-passo de configuração a baixo.

## Configurações importantes pré-execução na pasta do script

* Antes de executar o script vá em `external_vars.yml` e preencha as 3 variáveis obrigatórias para a perfeita execução do script.
  * `destdir` - informando em qual pasta os arquivos deverão ser baixados.
  * `projectname` - informando o nome do projeto. _O nome do projeto deve ser escrito da mesma forma que está salvo no gitlab (\*muito importante\*)_
  * `host_user` - nome do usuário que logará na máquiana ou que terá permissão de acesso a pasta do projeto. Por padrão use o mesmo nome usado no usuário para login, no arquivo hosts. _(em casos futuros, por exemplo de um projeto php ou wordpress use o nome do usuário do servidor web usado)_
  
* Tambem antes da execução do arquivo principal deve ser feita a configuração do inventário no arquivo _hosts_. abaixo da tag escrita _[servidores]_ assim como no exemplo substitua o adicione o endereço ou hostname do host que será alterado. Em baixo da tag _[servidores:vars]_ altere as seguintes variáveis.
  * `<ansible_user>` - nome do usuário que o _Ansible_ usará para fazer login no servidor, de preferência utilize o mesmo usuário que está setado na configuração feita acima no `host_user`.
  * `<ansible_ssh_private_key_file>` - coloque o caminho absoluto até a chave privada de acesso ssh do host que deve estar salva no pc que executara o script.

## Procedimentos para Execução

1. Para executar o script abra o terminal e dentro da pasta raiz do projeto execute `ansible-playbook -i hosts playbooks.yml`.
  
2. Ao executar o projeto as seguintes váriaveis serão preenchidas via prompt de comando onde foi executado o arquivo principal:

   * `usuário GitLab` - seu id de usuário cadastrado dentro do gitlab com permissões para acesso do grupo dti-desenvolvimento.
   * `senha GitLab` - sua senha de acesso ao gitlab, que juntamente com o usuário será exigida ao executar o script.
   * `Tipo de projeto` - atualmente os scripts suportam projetos rodados diretamente no uvicorn ou através de containers docker. Basta escrever "docker" ou "uvicorn" quando solicitado após login e senha.