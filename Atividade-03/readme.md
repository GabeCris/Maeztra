# Atividade 03

## Explicação

1. ### search.json

    Em VTEX IO as páginas são estruturadas nativamente com blocos montados em um arquivo **JSON**, em que o mesmo tem seus dados organizados em chave-valor, necessitando do uso das vírgulas para delimitação tanto em arrays quanto em objetos.

    O próprio console nos retorna um erro auto explicativo:

    ~~~javascript
    error: blocks/search.jsonc: Parse error on line 23
    ~~~    
    
    Apontando assim, a linha em que se encontra o problema no arquivo de busca da página, onde o interpretador não conseguiu fazer o parse do código por conta da falta da vírgula na delimitação dos blocos.

    Esse problema, entre outros, é muito comum quando não há tanta afinidade com a estrutura do **JSON**, pois se não possuir um formatador de código que auxilie nesses detalhes, fica difícil identificá-los sem causar algum erro.
    Por exemplo, falta de chaves de fechamento, falta de aspas em strings, etc.

    [Arquivo Corrigido](https://github.com/GabeCris/Maeztra/blob/main/Atividade-03/search.jsonc)

2. ### product-comparison-drawer.json
    
    Semelhante ao problema anterior, o console nos retorna um erro auto explicativo.

     ~~~javascript
    error: App build failed with message: Error resolving block "list-context.comparison-product-summary-slider#drawer": Unable to find an extension point declared for block "slider-layout#comparison-drawer" ...
    ~~~    

    Nesse caso o erro correu na tentativa de build do projeto, quando não foi encontrado o ponto de extensão declarado para o bloco, (`slider-layout#comparison-drawer` como block de `list-context.comparison-product-summary-slider#drawer`). E isso pode acontecer por alguns motivos, sendo eles:

     - Blocos configurados incorretamente: No VTEX IO alguns componentes tem a possibilidade de inserir outros componentes aninhados, o que ocorre através dos `blocks`, `children` e `allowed`. Ao tentar adicionar um componente que não tem esse suporte, também receberá uma mensagem de erro. 

        `Importante:` O nosso problema está justamente nesse ponto, pois o `slider-layout#comparison-drawer` não é um bloco aceito no `list-context.comparison-product-summary-slider#drawer`, apenas no children

        ~~~javascript
        "list-context.comparison-product-summary-slider#drawer": {
            "blocks": ["product-summary.shelf.product-comparison#drawer"],
            "children": ["slider-layout#comparison-drawer"]
        },
        ~~~

    - Dependências ausentes: Em alguns casos o erro pode estar relacionado a falta de instalação de alguma dependência, ou inserção da mesma no manifest.json do tema da loja.

    - Nome do bloco incorreto: O nome do bloco precisa ser exatamente igual tanto na declaração quanto na chamada dele. Isso vale tanto para os componentes nativos disponbilizados pela vtex, exemplo: `slider-layout`, quanto pelo identificador que o desenvolvedor associa ao bloco, exemplo: `#comparison-drawer`.

    - Bloco não declarado: O bloco precisa estar presente em algum arquivo do projeto (não necessariamente o mesmo em que é chamado). Caso não haja nenhum arquivo declarando o bloco com o mesmo componente e identificador, o interpretador irá retornar erro.

    Essas são algumas causas dos erros mais comuns relacionados a blocos que tenho conhecimento.

    A solução mais prática é fazer uma pesquisa local no arquivo ou até mesmo globalmente no repositório, analisando o bloco acusado no erro, identificando se ele existe, se é um problema de nomenclatura ou de dependência. E se necessário, adicioná-lo ou ajustá-lo. Caso não haja sucesso, fazer uma busca mais aprofundada na documentação.

    [Arquivo Corrigido](https://github.com/GabeCris/Maeztra/blob/main/Atividade-03/product-comparison-drawer.json)
