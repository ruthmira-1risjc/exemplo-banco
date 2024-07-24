### Estrutura das Entidades

#### Grupo
- **nome_grupo**: Nome do grupo e identificador único.
- **descricao**: Descrição do grupo.
- **perfil_id**: Chave estrangeira para a tabela Perfil.

#### Pessoa
- **id**: Identificador único da pessoa.
- **foto3x4**: Foto da pessoa (BLOB).
- **nome**: Nome completo do usuário.
- **email**: Endereço de email do usuário.
- **data_criacao**: Data de criação da conta.

#### Usuario
- **id**: Identificador único do usuário (referência à tabela Pessoa).
- **usuario**: Nome de usuário (único).
- **senha**: Senha do usuário (armazenada de forma segura, e.g., hash).
- **ultimo_login**: Data e hora do último login.
- **ativo**: Indicador se a conta está ativa.
- **nome_grupo**: Chave estrangeira para a tabela Grupo.

#### Aplicacao
- **nome**: Nome da aplicação e identificador único.
- **descricao**: Descrição da aplicação.
- **caminhoExecutavel**: Caminho do executável na rede.
- **icone**: Ícone da aplicação (BLOB).
- **categoria**: Categoria da aplicação (coluna, flag, ou enumerador).

#### Permissao
- **id**: Identificador único da permissão.
- **nome**: Nome da permissão.
- **descricao**: Descrição da permissão.
- **nome_grupo**: Chave estrangeira para a tabela Grupo.
- **nome_aplicacao**: Chave estrangeira para a tabela Aplicacao.

#### Log
- **id**: Identificador único do log.
- **tipo**: Nome do tipo do log.
- **usuario_id**: Chave estrangeira para a tabela Usuario.
- **nome_aplicacao**: Chave estrangeira para a tabela Aplicacao.
- **datahora**: Data e hora do log.
- **mensagem**: Mensagem do log.

#### Perfil
- **id**: Identificador único do perfil.
- **descricao**: Descrição do perfil.

#### Exibicao
- **perfil_id**: Chave primária e estrangeira para a tabela Perfil.
- **nome_aplicacao**: Chave estrangeira para a tabela Aplicacao.

<br>

## Diagrama
![C:\Users\ruth.silva\Desktop\exemplo-banco\img\bancoEP.jpg](<img/bancoEP.jpg>)


### Relacionamentos

#### Grupo e Perfil
- **Descrição**: Cada grupo está associado a um perfil, mas um grupo pode existir sem um perfil.
- **Cardinalidade**: Um perfil pode estar associado a muitos grupos (1:N).
- **Regra**: 
  - Cada grupo pode ter no máximo um perfil associado.
  - Um perfil pode ser associado a vários grupos.
  - Um grupo pode existir sem estar associado a um perfil.

#### Perfil e Exibicao
- **Descrição**: Cada perfil pode ter várias aplicações associadas através da tabela Exibicao.
- **Cardinalidade**: Muitos para muitos (N:N).
- **Regra**:
  - Um perfil pode ter várias aplicações associadas.
  - Uma aplicação pode estar associada a vários perfis.
  - A tabela `Exibicao` atua como uma tabela de junção para gerenciar essas associações.

#### Aplicacao e Exibicao
- **Descrição**: Uma aplicação pode estar associada a vários perfis através da tabela Exibicao.
- **Cardinalidade**: Muitos para muitos (N:N).
- **Regra**:
  - Uma aplicação pode estar associada a vários perfis.
  - Um perfil pode ter várias aplicações associadas.
  - A tabela `Exibicao` gerencia as relações de associação entre perfis e aplicações.

#### Grupo e Permissao
- **Descrição**: Um grupo pode ter permissões para várias aplicações através da tabela Permissao.
- **Cardinalidade**: Muitos para muitos (N:N).
- **Regra**:
  - Um grupo pode ter várias permissões para diferentes aplicações.
  - Uma aplicação pode ter permissões para vários grupos.
  - A tabela `Permissao` gerencia essas relações de permissão entre grupos e aplicações.

#### Pessoa e Usuario
- **Descrição**: Cada pessoa tem um usuário associado para login e autenticação.
- **Cardinalidade**: Um para um (1:1).
- **Regra**:
  - Cada pessoa possui exatamente um usuário associado.
  - Cada usuário está vinculado a uma única pessoa.
  - Informações pessoais e de autenticação são gerenciadas separadamente.

#### Usuario e Log
- **Descrição**: Um usuário pode gerar vários logs.
- **Cardinalidade**: Um para muitos (1:N).
- **Regra**:
  - Um usuário pode gerar múltiplos registros de log.
  - Cada log está associado a um único usuário.

#### Aplicacao e Log
- **Descrição**: Uma aplicação pode gerar vários logs.
- **Cardinalidade**: Um para muitos (1:N).
- **Regra**:
  - Uma aplicação pode ter múltiplos registros de log associados a ela.
  - Cada log está associado a uma única aplicação.


### Pontos Importantes

1. **Associação Única de Perfil a Grupo**:
   - Cada grupo pode ter no máximo um perfil associado. Isso significa que não é possível que um grupo tenha mais de um perfil ao mesmo tempo.
   - No entanto, um grupo pode existir sem estar associado a um perfil, o que permite flexibilidade na configuração inicial dos grupos.

2. **Flexibilidade de Associação de Aplicações a Perfis**:
   - Perfis podem ter várias aplicações associadas e uma aplicação pode pertencer a vários perfis. Isso permite uma configuração flexível das permissões e acessos das aplicações.
   - A tabela `Exibicao` facilita a gestão dessas associações, garantindo que as relações N:N sejam corretamente mantidas.

3. **Gestão de Permissões através de Grupos**:
   - Permissões são geridas através da tabela `Permissao`, que liga grupos a aplicações.
   - Isso permite que diferentes grupos tenham diferentes níveis de acesso às aplicações, conforme definido pelas permissões.

4. **Segregação de Informações Pessoais e de Autenticação**:
   - A separação das entidades `Pessoa` e `Usuario` assegura que as informações pessoais e de autenticação sejam geridas de forma independente, aumentando a segurança e a organização do sistema.

5. **Rastreabilidade Através dos Logs**:
   - A associação direta dos logs com a entidade `Usuario` garante que todas as ações sejam rastreáveis até o usuário específico, facilitando a auditoria e a segurança do sistema.

### Exemplo de Implementação de Regras

1. **Adicionar um Grupo**:
   - Ao criar um grupo, inicialmente ele pode não ter um perfil associado. Um perfil pode ser associado posteriormente conforme necessário.

2. **Adicionar um Perfil**:
   - Ao criar um perfil, várias aplicações podem ser associadas a ele. Esse perfil pode então ser vinculado a vários grupos, concedendo-lhes as permissões definidas pelo perfil.

3. **Gerenciamento de Permissões**:
   - Permissões específicas podem ser atribuídas a grupos através da tabela `Permissao`, que define quais aplicações um grupo pode acessar.

4. **Login e Auditoria**:
   - Ao efetuar login, o sistema verifica as credenciais na tabela `Usuario`. Ações realizadas pelo usuário são registradas na tabela de `Log`, associando cada ação ao usuário específico e à aplicação utilizada.


### Vantagens dessa Abordagem

1. **Segregação de Responsabilidades**: Mantém as informações pessoais e de autenticação separadas, facilitando a gestão e segurança.
2. **Segurança**: Assegura que informações sensíveis de login sejam gerenciadas em uma tabela dedicada.
3. **Clareza nos Logs**: Os logs são associados diretamente aos usuários que realizam ações, facilitando a rastreabilidade.


### Fluxo de Login

1. **Entrada do Usuário**: O usuário insere seu nome de usuário e senha.
2. **Autenticação**: O sistema verifica se o nome de usuário existe na tabela Usuario e se a senha corresponde ao hash armazenado.
3. **Autorização**: O sistema verifica as permissões do usuário com base no grupo e no perfil associado.
4. **Acesso**: O usuário ganha acesso às aplicações e funcionalidades conforme suas permissões.
5. **Registro de Log**: A ação de login é registrada na tabela de logs, incluindo informações sobre data, hora e usuário.
