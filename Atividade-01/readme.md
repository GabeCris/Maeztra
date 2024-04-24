# Atividade 01

## Explicação

- ### Conditional Layout

    A nossa atividade consistia em criar uma condição somente com blocos nativos, para renderizar componentes diferentes de acordo com a regra aplicada. No caso: Produto indisponível.

    A VTEX nos disponibiliza esse aplicativo, que renderiza um componente na loja dependendendo de condições predefinidas, podendo ser utilizada em página de produto, páginas de categoria ou também de coleção.

    Existem algumas variantes que nos permitem habilitar a renderização condicional de acordo com o cenário necessário, alguns deles são:

    - condition-layout.product
    - condition-layout.binding
    - condition-layout.category
    - condition-layout.telemarketing

    Para o nosso cenário utilizamos o `condition-layout.product`, onde passamos algumas propriedades que compõem melhor as condições do nosso componente.

    ~~~javascript
    "condition-layout.product#availability": {
      "props": {
        "conditions": [
          {
            "subject": "isProductAvailable"
          }
        ],
        "matchType": "any",
        "Then": "flex-layout.col#right-col",
        "Else": "flex-layout.col#right-col-unavailable"
      }
    }
    ~~~~

    **1. conditions [ subject ]:** Passamos o valor `isProductAvailable` para especificarmos ao nosso componente que a renderização será baseada na disponibilidade do produto. 

    **2. matchType:** Aqui passamos o critério para renderização do layout, onde basicamente informamos quantas condições devem ser atendidas para renderizar o layout.

    **3. Then / Else**: Nessas props passamos os blocos que serão renderizados quando as condições forem atendidas, e também não forem atendidas.

- ### Enable/Disable

    Até o momento da tentativa de execução dessa etapa da atividade, não tenho conhecimento de nenhumm método para permitir que o cliente consiga habilitar/desabilitar conteúdos através do `Site Editor`, estruturando o código somente com blocos nativos.

    Alguns componentes até permitem que o cliente deixe conteúdo vazio, como deixar o texto de um `Rich Text` em branco.

    Porém, temos a possibilidade de adicionar essa funcionalidade através dos blocos customizados, onde estruturamos um schema com tipo booleano. Dessa forma no `Site Editor` irá ficar visível um botão de habilitar/desabilitar, que podemos utilizar no código para renderização condicional.

    Exemplo de schema com tipo booleano: 
    ~~~javascript
    "iframe": {
                "title": "Ativar Iframe",
                "description": "Quando ativo, o mapa dinâmico é substituído por uma Iframe",
                "type": "boolean"
            }
    ~~~

### Bônus

Juntando as duas soluções, blocos customizados, com o componente de Conditional Layout disponibilizado pela VTEX, podemos construir um código no React semelhante a isso.

~~~javascript
return isEnable && (
  <ConditionLayout.Product Else={<SomethingElse />}>
    <MyComponentWhenConditionMatch />
  </ConditionLayout.Product>
)
~~~

Ou poderíamos ir mais longe ainda, desenvolver um componente customizado em que a única responsabilidade dele é receber blocos como children, porém ele possui o schema com tipo booleano para controlar a renderização, como no exemplo acima. Dessa forma podemos declarar o componente no interfaces, e chamá-los juntamente com os blocos nativos, fazendo assim uso das duas possibilidades de desenvolvimento com a VTEX.

