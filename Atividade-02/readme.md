# Atividade 02

## Explicação

Supondo que o login na loja presente no vendor do manifest (`maeztra`), já tenha sido realizado, temos alguns passos importantes para realização de um deploy, sendo eles:

1. Fazer update da versão do manifest, assim como atualizar o changelog com as alterações realizadas no repositório
2. Publicar a nova versão do aplicativo
3. Realizar comando responsável pelo deploy do aplicativo
4. Linkar no workspace desejado
5. Fazer a instalação das alteração

Segue explicação detalhada de cada ponto:

- ### Update Version:

    Esse passo pode ser feito de duas maneiras: manualmente, alterando o valor de version presente no manifest: `"version": "5.1.0"`, ou o mais sugerido, que é usando os comandos qua a própria VTEX disponibiliza para lançar uma nova versão. 
    [Doc com todos os comandos](https://developers.vtex.com/docs/guides/vtex-io-documentation-releasing-a-new-app-version).

    Considerando que em nosso cenário adicinonamos uma dependência de um aplicativo que irá alterar as opções de faturamento, precisamos utilizar a linha de código que altera o major (versão principal) do nosso aplicativo. Sendo então:
    ~~~javascript
    vtex release major stable
    ~~~    

    Após feito o comando, a versão será atualizada automaticamente e já estará pronta para ser publicada.

    Versão atualizada do nosso exemplo: `"version": "6.0.0"`

- ### Publish: 

    Estando com a versão do nosso aplicativo atualizada, podemos publicá-la na VTEX usando o seguinte comando na raiz do nosso repositório:
    ~~~javascript
    vtex publish
    ~~~    
- ### Deploy:
    Assim que a execução do publish terminar, no próprio terminal você encontrará no fim da linha o comando já formatado com o `vendor`, `name` e `version` corretos para realizar o deploy. Que ficaria mais ou menos assim:

    ~~~javascript
    vtex deploy maeztra.store-theme@6.0.0
    ~~~    

    **Uma dica** aqui é utilizar `-f` no final do comando, para forçar o deploy e impedir que a vtex realize 7 minutos de teste na nossa versão.

    `vtex deploy maeztra.store-theme@6.0.0 -f`

- ### Link:
    Nesse passo nós escolheremos o workspace de instalação de nosso aplicativo. 
    Caso a tarefa seja subir para a produção, nós linkamos então na `master` com o seguinte comando:
     ~~~javascript
    vtex use master
    ~~~   

    Caso seja necessário, adicionar o complemento `--production` ao final do comando para instalar as alterações em um ambiente de prod, que poderá servir tanto para testes quanto para inserção de conteúdos através do site editor, e posteriomente terá a possibilidade de ser replicado na master.
- ### Install: 
    E por último, o passo que não deve ser esquecido, é fazer a instalação da nossa nova versão. Utilizamos o comando:
     ~~~javascript
    vtex install 
    ~~~   

    Ao rodar somente esse comando, todos os deploys que estiverem esperando instalação dentro do repositório, serão instalados, caso queira especificar qual app deverá ser atualizado, basta inserior o `vendor`, `name` e `version`, semelhante ao comando de deploy.

### Observações: 
Vale lembrar que esse tutorial é apenas uma receita comum para a maioria dos casos, existem algumas particularidades e especificadades de cada projeto que podem acabar alterando um pouco esse fluxo de deploy, como por exemplo subir para um ambiente de `--production`, que posteriomente precisará instalar separadamente para a master, entre outros detalhes que podem acabar impactando nesse passo a passo.