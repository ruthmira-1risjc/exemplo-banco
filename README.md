# Estrutura do Banco Intranet

## Banco


<details>
<summary>Entidades</summary>

## Entidades:

**Grupo**
   - **Id**: Identificador único.
   - **NomeGrupo**: Nome do grupo.
   - **Descricao**: Descrição do grupo.

2. **Usuario**
   - **Id**: Identificador único.
   - **IconeId**: Chave estrangeira para a tabela `Icone`.
   - **Nome**: Nome completo do usuário.
   - **Email**: Endereço de email do usuário.
   - **DataCriacao**: Data de criação da conta.
   - **Usuario**: Nome de usuário (único).
   - **GrupoId**: Chave estrangeira para a tabela `Grupo`.

3. **Icone**
   - **Id**: Identificador único.
   - **Foto3x4**: Foto da pessoa (BLOB).
   - **Icone**: Ícone associado (BLOB).

4. **Recurso**
   - **Id**: Identificador único.
   - **Nome**: Nome do recurso.
   - **Descricao**: Descrição do recurso.
   - **Rota**: Caminho do recurso na rede.
   - **IconeId**: Chave estrangeira para a tabela `Icone`.
   - **TipoRecursoId**: Chave estrangeira para a tabela `TipoRecurso`.

5. **TipoRecurso**
   - **Id**: Identificador único.
   - **Descricao**: Descrição do tipo de recurso.
   - **Nome**: Nome do tipo de recurso.

6. **Log**
   - **Id**: Identificador único.
   - **Tipo**: Tipo do log.
   - **UltimoLogin**: Data e hora do último login.
   - **UsuarioId**: Chave estrangeira para a tabela `Usuario`.
   - **RecursoId**: Chave estrangeira para a tabela `Recurso`.
   - **Datahora**: Data e hora do log.
   - **Mensagem**: Mensagem do log.

7. **PerfilPermissao**
   - **Id**: Identificador único.
   - **Nome**: Nome do perfil/permissão. Pode representar um tipo ou nome descritivo.
   - **Descricao**: Descrição do perfil/permissão.
   - **GrupoId**: Chave estrangeira para a tabela `Grupo`.
   - **RecursoId**: Chave estrangeira para a tabela `Recurso`.

8. **Exibicao**
   - **Id**: Identificador único.
   - **PerfilPermissaoId**: Chave estrangeira para a tabela `PerfilPermissao`.
   - **Elemento**: Identificadores de elementos específicos da página (ex: botões, ações).

</details>

<details>
<summary>Tabelas</summary>

## Tabelas

#### Grupo
- **Id**: INT (Chave primária)
- **NomeGrupo**: STRING
- **Descricao**: STRING

#### Usuario
- **Id**: INT (Chave primária)
- **IconeId**: INT (Chave estrangeira para `Icone.Id`)
- **Nome**: STRING
- **Email**: STRING
- **DataCriacao**: DATETIME
- **Usuario**: STRING
- **GrupoId**: INT (Chave estrangeira para `Grupo.Id`)

#### Icone
- **Id**: INT (Chave primária)
- **Foto3x4**: BLOB
- **Icone**: BLOB

#### Recurso
- **Id**: INT (Chave primária)
- **Nome**: STRING
- **Descricao**: STRING
- **Rota**: STRING
- **IconeId**: INT (Chave estrangeira para `Icone.Id`)
- **TipoRecursoId**: INT (Chave estrangeira para `TipoRecurso.Id`)

#### TipoRecurso
- **Id**: INT (Chave primária)
- **Descricao**: STRING
- **Nome**: STRING

#### Log
- **Id**: INT (Chave primária)
- **Tipo**: STRING
- **UltimoLogin**: DATETIME
- **UsuarioId**: INT (Chave estrangeira para `Usuario.Id`)
- **RecursoId**: INT (Chave estrangeira para `Recurso.Id`)
- **Datahora**: DATETIME
- **Mensagem**: STRING

#### PerfilPermissao
- **Id**: INT (Chave primária)
- **Nome**: STRING
- **Descricao**: STRING
- **GrupoId**: INT (Chave estrangeira para `Grupo.Id`)
- **RecursoId**: INT (Chave estrangeira para `Recurso.Id`)

#### Exibicao
- **Id**: INT (Chave primária)
- **PerfilPermissaoId**: INT (Chave estrangeira para `PerfilPermissao.Id`)
- **Elemento**: STRING
</details>

<details>
<summary>Relacionamentos</summary>

## Relacionamentos

1. **Grupo e Usuario**
   - **Descrição**: Um grupo pode ter vários usuários. Um usuário pertence a um grupo.
   - **Cardinalidade**:Um para muitos (1).
     - **Grupo (1, N) — (0, N) Usuario**: Um grupo pode ter zero ou mais usuários.
     - **Usuario (1, 1) — (1, 1) Grupo**: Cada usuário pertence a um único grupo.

2. **Grupo e PerfilPermissao**
   - **Descrição**: Um grupo pode ter várias permissões e perfis associados. Cada perfil pode estar associado a vários grupos.
   - **Cardinalidade**: Muitos para muitos (N).
     - **Grupo (1, N) — (0, N) PerfilPermissao**: Um grupo pode ter zero ou mais permissões/perfis.
     - **PerfilPermissao (1, 1) — (1, N) Grupo**: Cada perfil/permissão pode estar associado a vários grupos.

3. **Recurso e PerfilPermissao**
   - **Descrição**: Um recurso pode ter várias permissões/perfis associados. Uma permissão/perfil está associada a um recurso.
   - **Cardinalidade**: Muitos para muitos (N).
     - **Recurso (1, N) — (0, N) PerfilPermissao**: Um recurso pode ter zero ou mais permissões/perfis.
     - **PerfilPermissao (1, 1) — (1, N) Recurso**: Cada permissão/perfil está associada a um recurso.

4. **PerfilPermissao e Exibicao**
   - **Descrição**: Define quais elementos de uma página são visíveis para um perfil específico. Cada exibição está associada a um único perfil/permissão.
   - **Cardinalidade**: Um para muitos (1).
     - **PerfilPermissao (1, N) — (0, N) Exibicao**: Um perfil/permissão pode ter zero ou mais exibições.
     - **Exibicao (1, 1) — (1, 1) PerfilPermissao**: Cada exibição está associada a um único perfil/permissão.

5. **Usuario e Log**
   - **Descrição**: Um usuário pode gerar vários logs. Cada log é associado a um único usuário.
   - **Cardinalidade**: Um para muitos (1).
     - **Usuario (1, N) — (0, N) Log**: Um usuário pode gerar zero ou mais logs.
     - **Log (1, 1) — (1, 1) Usuario**: Cada log é associado a um único usuário.

6. **Recurso e Log**
   - **Descrição**: Um recurso pode ter vários logs associados. Cada log está vinculado a um único recurso.
   - **Cardinalidade**: Um para muitos (1).
     - **Recurso (1, N) — (0, N) Log**: Um recurso pode gerar zero ou mais logs.
     - **Log (1, 1) — (1, 1) Recurso**: Cada log está associado a um único recurso.

7. **Recurso e TipoRecurso**
   - **Descrição**: Um recurso é classificado por um tipo específico de recurso. Cada tipo de recurso pode ser associado a vários recursos.
   - **Cardinalidade**: Um para muitos (1).
     - **TipoRecurso (1, N) — (0, N) Recurso**: Um tipo de recurso pode estar associado a zero ou mais recursos.
     - **Recurso (1, 1) — (1, 1) TipoRecurso**: Cada recurso está associado a um único tipo de recurso.

</details>

<details>
<summary>Diagrama Mermaid Code</summary>

## Diagrama ER

O diagrama a seguir representa visualmente o relacionamento entre as tabelas:

```mermaid
erDiagram

    Grupo {
        INT id PK
        STRING nome_grupo
        STRING descricao
    }
    
    Usuario {
        INT id PK
        INT icone_id FK
        STRING nome
        STRING email
        DATETIME data_criacao
        STRING usuario
        INT grupo_id FK
    }

    
    Icone {
        INT id PK
        BLOB foto3x4
        BLOB icone
    }
    
    Recurso {
        INT id PK
        STRING nome
        STRING descricao
        STRING rota
         INT icone_id FK
        STRING tipo_recurso_id FK
    }

     TipoRecurso {
        INT id PK
        STRING descricao
        STRING nome
    }
    
    Log {
        INT id PK
        STRING tipo
        DATETIME ultimo_login
        INT usuario_id FK
        INT aplicacao_id FK
        DATETIME datahora
        STRING mensagem
    }
    
    PerfilPermissao {
        INT id PK
        STRING nome
        STRING descricao
        INT grupo_id FK
        INT aplicacao_id FK
    }
    
    Exibicao {
        INT id PK
        INT perfil_permissao_id FK
        STRING elementos
    }
    
    Grupo ||--o{ Usuario : "possui"
    Grupo ||--o{ PerfilPermissao : "tem"
    PerfilPermissao ||--o{ Recurso : "associado a"
    PerfilPermissao ||--o{ Exibicao : "define"
    Usuario ||--o{ Log : "gera"
    Usuario ||--o{ Icone : "possui"
    Recurso ||--o{ Icone : "possui"
    Recurso ||--o{ Log : "gera"
    Recurso ||--o{ TipoRecurso : "contem"
```
</details>

<details>
<summary>Pontos Importantes</summary>

## Pontos Importantes

### 1. **Grupo e Usuario**

- **Relacionamento:** Um grupo pode ter vários usuários, mas cada usuário pertence a um único grupo.
- **Importância:** Esse relacionamento é fundamental para organizar os usuários em grupos, facilitando o gerenciamento e a aplicação de permissões ou políticas específicas.

**Exemplo:**
- Um grupo chamado "Administração" pode ter usuários como "Ana", "João" e "Carlos".
- O usuário "Ana" está associado ao grupo "Administração", e seu `GrupoId` é a chave estrangeira que referencia o `Id` do grupo.

### 2. **Grupo e PerfilPermissao**

- **Relacionamento:** Um grupo pode ter várias permissões/perfis associados, e cada perfil/permissão pode estar vinculado a vários grupos.
- **Importância:** Esse relacionamento permite que diferentes grupos tenham acesso a diferentes conjuntos de permissões e perfis, facilitando a personalização do acesso às aplicações.

**Exemplo:**
- O grupo "Administração" pode ter um perfil chamado "Perfil Completo" que dá acesso total a todas as aplicações.
- O grupo "Usuários" pode ter um perfil chamado "Perfil Básico" com permissões limitadas.

### 3. **Aplicacao e PerfilPermissao**

- **Relacionamento:** Cada aplicação pode estar associada a várias permissões/perfis, e cada perfil/permissão pode estar associado a várias aplicações.
- **Importância:** Isso permite que as permissões de acesso às aplicações sejam flexíveis e adaptáveis às necessidades dos grupos.

**Exemplo:**
- A aplicação "Sistema de Finanças" pode ter permissões associadas como "Visualizar Relatórios" e "Editar Dados".
- O perfil "Perfil Completo" pode ter acesso a todas as permissões da aplicação "Sistema de Finanças", enquanto o perfil "Perfil Básico" pode ter acesso apenas à visualização de relatórios.

### 4. **PerfilPermissao e Exibicao**

- **Relacionamento:** Um perfil/permissão pode definir várias exibições, e cada exibição está associada a um único perfil/permissão.
- **Importância:** Define quais elementos (como botões, seções, etc.) são visíveis para cada perfil. Isso é útil para personalizar a interface com base nas permissões.

**Exemplo:**
- O perfil "Perfil Completo" pode ter exibições que mostram todas as seções do sistema.
- O perfil "Perfil Básico" pode ter uma exibição que oculta seções avançadas.

### 5. **Usuario e Log**

- **Relacionamento:** Cada usuário pode gerar vários logs, e cada log é associado a um único usuário.
- **Importância:** Permite rastrear atividades e eventos relacionados a cada usuário, ajudando na auditoria e análise de comportamento.

**Exemplo:**
- O usuário "Ana" pode ter logs que mostram suas atividades no sistema, como login e acesso a diferentes aplicações.

### 6. **Aplicacao e Log**

- **Relacionamento:** Cada aplicação pode gerar vários logs, e cada log está associado a uma única aplicação.
- **Importância:** Facilita o rastreamento de eventos e atividades específicas para cada aplicação, ajudando a identificar problemas e melhorar o desempenho.

**Exemplo:**
- A aplicação "Sistema de Finanças" pode gerar logs de eventos como erros ou acessos, que ajudam a monitorar o uso e detectar falhas.

</details>

<details>
<summary>Fluxo de Login</summary>

## Fluxo de Login

1. **Autenticação Externa**
   - **Entrada do Usuário:** O usuário fornece suas credenciais (nome de usuário e senha) na tela de login.
   - **Solicitação de Autenticação:** A aplicação de login envia as credenciais para o serviço de autenticação externo (outra aplicação) que valida as credenciais.
   - **Resposta do Serviço de Autenticação:** O serviço retorna um resultado indicando se as credenciais são válidas ou não. Se válidas, o serviço pode fornecer um token de autenticação ou um identificador único do usuário.

2. **Verificação de Credenciais**
   - **Resultado da Autenticação:** Se a autenticação for bem-sucedida, o sistema local (o sistema que gerencia o perfil e permissões) recebe um token ou identificador do usuário.
   - **Recuperação do Perfil:** O sistema local utiliza o identificador do usuário para recuperar o perfil associado ao grupo do usuário no banco de dados, consultando a tabela `Usuario` para encontrar o `GrupoId`.

3. **Recuperação do Perfil e Permissões**
   - **Determinação do Grupo:** O sistema consulta a tabela `Usuario` para identificar o grupo ao qual o usuário pertence usando o `GrupoId`.
   - **Recuperação do Perfil:** O sistema local recupera o perfil associado ao grupo do usuário na tabela `PerfilPermissao`.
   - **Permissões de Acesso e Exibição:** O sistema consulta as tabelas `PerfilPermissao` e `Exibicao` para obter as permissões de acesso e os elementos visíveis para o usuário com base em seu perfil.

4. **Configuração do Ambiente do Usuário**
   - **Permissões de Acesso:** O sistema ajusta o acesso às aplicações e páginas do sistema de acordo com as permissões associadas ao perfil.
   - **Permissões de Exibição:** O sistema configura quais elementos são visíveis nas páginas (como botões e seções) com base nas permissões de exibição associadas ao perfil.

5. **Registro de Log**
   - **Criação de Log:** O sistema registra a tentativa de login e outras ações relevantes na tabela `Log`.
   - **Informações do Log:** Inclui informações como o `UsuarioId`, o `AplicacaoId`, a `Datahora`, e uma mensagem descritiva (por exemplo, "Login bem-sucedido").

6. **Redirecionamento e Acesso**
   - **Redirecionamento:** Após a autenticação e configuração bem-sucedida do ambiente, o usuário é redirecionado para a página inicial do sistema ou para uma página específica conforme definido pelas permissões do perfil.

7. **Gerenciamento de Sessão**
   - **Manutenção da Sessão:** O sistema mantém a sessão ativa do usuário, garantindo que o acesso às diferentes partes do sistema seja controlado conforme as permissões do perfil.
   - **Controle de Acesso:** Durante a sessão, o sistema verifica continuamente as permissões para garantir que o usuário possa acessar apenas as áreas permitidas.

</details>

<details>
<summary>Diagrama da Sequência do Login</summary>

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant User
    participant LoginApp
    participant AuthService
    participant LocalSystem
    participant Database

    User->>LoginApp: Fornece credenciais
    LoginApp->>AuthService: Envia credenciais para autenticação
    AuthService-->>LoginApp: Retorna resultado (token/identificador)
    LoginApp->>LocalSystem: Envia identificador de usuário
    LocalSystem->>Database: Consulta perfil associado ao grupo
    Database-->>LocalSystem: Retorna perfil e permissões
    LocalSystem->>Database: Registra tentativa de login
    Database-->>LocalSystem: Confirmação de registro
    LocalSystem->>User: Redireciona para página inicial
    LocalSystem->>SessionManager: Mantém sessão ativa
```

</details>