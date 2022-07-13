# Arquitetura Limpa e Typescript com Filipe Santos - [Informativo Dev](https://web.microsoftstream.com/video/103cf595-df3f-4e22-8d1c-ec401053cbe5)

## Clean Architecture:

- Clean Architecture: O que é isso?
  - Modelo de Arquitetura em Camadas
  - Isola as regras de negócio da infraestrutura da aplicação
  - Implementa os Princípios SOLID
- SOLID:
  - S: Single-responsibility principle - "Uma classe deve ter uma e somente uma responsabilidade, ou seja, apenas mudanças em uma parte das especificações de um sistema devem afetar a especificação da classe."
  - O: Open-closed principle - "Componentes da aplicação devem estar disponíveis para extensão, mas bloqueados para modificação
  - L: Liskov substitution principle - Polimorfismo na prática - "Objetos de um programa devem ser substituíveis por instâncias de seus subtipos de forma que isso não altera o funcionamento do programa."
  - I: Interface segregation principle - "Muitas interfaces específicas e pequenas são melhores do que uma única grande interface com um propósito geral."
  - D: Dependency inversion principle - Sempre desenvolver direcionado para interfaces - "Os componentes da aplicação devem depender de abstrações, não de classes concretas."

## Camadas:

- Entidades:
  - Domínio da Aplicação;
  - Regras de negócio corporativas;
  - Regras de negócio mais críticas dentro da aplicação;
  - Exemplo: Boleto no caso da GN no passado
- Casos de Uso:
  - Regras de negócio da aplicação;
  - Define interações de um determinado ator com o sistema
  - Cliente, usuário, analista de RH
  - O que um cliente quer fazer com um boleto? CDU - EmitirUmBoleto
- Adaptadores de Interface:
  - Camada ingênua
  - Convertem interfaces familiares as regras de negócio para interfaces familiares a infraestrutura da aplicação
  - Conveter CDU para Infraestrutura
- Infraestrutura:
  - Finalidade: fazer integração
  - Onde ficam as bibliotecas, Banco de dados, frameworks e drivers
  - Camada mais ingênua ainda

## Camada de Entidades

- Contém objetos que representam o domínio da aplicação.
- É onde estão concetradas as regras de negócio da Corporação, não se restringindo apenas a aplicação
- Segundo Eric Evans, criador do conceito de DDD, tais objetos podem ser classificados como:
  - Value Objects (Entidades Simples)
    - CPF, CNPJ, Name, Etc
    - Igualdade garantida através das propriedades
  - Entidades
    - User, Item, Product
    - Igualdade garantida através de um identificador único
    - Ex: User tem várias propriedades, só é possível fazer igualdade através do identificador unico
  - Aggregates:
    - Entidade que contém outras entidades
    - Ex: Order (Array de Itens)
    - Papel do Aggregate: Gerenciar entidades filhas

## Camada de Casos de Uso

- É onde estão as regras de negócio específicas da aplicação
- Geralmente é composta por um interactor com a função exclusiva de executar uma ação designada por um ator do sistema
- Assim como as entidades, essa camada não deve conhecer nenhum detalhe de implementação. Ao invés disso, deve providenciar interfaces de entrada e saída que serão implementadas por outras camadas
- Ex: CreateUser, DeleteUser, CancelCharge
- Altamente indicado em fazer o documento de descrição dos casos de uso

## Camada de Adaptadores de Interface

- Camada responsável por traduzir dados do formato mais acessível a um caso de uso para o formato mais acessível a camada de infraestrutura. O processo inverso também é realizado nessa camada
- Implementam interfaces de entrada e saída da camada de casos de uso
- Não devem conhecer detalhes de implementações das outras camadas - camada ingênua
- Podem ser:
  - Controllers - Interface de entrada do caso de uso
  - Presenter - Interface de saída do caso de uso
  - Gateways - Interface de acesso a dados
- Controllers:
  - Define e adapta uma interface de requisição (HTTP, Form, CLI commands) em um modelo de entrada do caso de uso
  - Executa o caso de uso
  - Definir controllers para APIS (Entrada - HTTP Request) e converter as propriedades para um Input Model
- Presenters:
  - Processo inverso do Controller
  - Define e adapta uma interface modelo de resposta do caso de uso em uma estrutura de visualização do dado para uma infraestrutura específica (HTTP response model, View, etc)
  - Responsável por criar e armazenar um objeto de visualização
  - Tratar os erros
- Gateways:
  - Adaptadores mais complexos, pois fazem o meio de campo para que o CDU consiga acessar um determinado dado
  - Implementam interfaces de acesso a dados especificas pelos casos de uso
  - Podem ser implementados com auxílio de vários padrões de projetos:
    - Data Mappers (ORM - Sequelize por exemplo)
    - Repository Patern
    - Unit of Work
    - Adapter
    - Mixins (Juntam várias interfaces em uma única interface)

## Camada de Infraestrutura

- É onde está concentrada toda a infraestrutura da aplicação
- Frameworks, Drivers, DBs, WEB, Express, Router, Bibliotecas
- Nesta camada é feita também a orquestração de todos os outros componentes

_Assistindo vídeo em 37:06_
