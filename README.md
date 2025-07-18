# Virtual File System - Sistema de Arquivos Virtual

## Descrição do Projeto

O Virtual File System é um sistema de arquivos simulado em memória desenvolvido em Java, que oferece operações básicas de gerenciamento de arquivos e diretórios com controle de permissões por usuário.

## Funcionalidades Principais

1. **Gerenciamento de Usuários**:
   - Adição e remoção de usuários (exceto o usuário root)
   - Controle de permissões individuais por arquivo/diretório

2. **Operações de Arquivos**:
   - Criação de arquivos (`touch`)
   - Leitura e escrita em arquivos (`read`, `write`)
   - Remoção de arquivos (`rm`)
   - Cópia e movimentação de arquivos (`cp`, `mv`)

3. **Operações de Diretórios**:
   - Criação de diretórios (`mkdir`)
   - Listagem de conteúdo (`ls`)
   - Remoção de diretórios (opcionalmente recursiva)

4. **Controle de Permissões**:
   - Alteração de permissões (`chmod`)
   - Verificação de permissões para cada operação

## Tecnologias e Padrões Utilizados

- **Padrão Proxy**: `IFileSystem` atua como interface para a implementação real do sistema de arquivos
- **Tratamento de Exceções Customizadas**: Para diferentes cenários de erro
- **Controle de Acesso Baseado em Usuários**: Com permissões granularizadas

## Estrutura do Projeto

```
src/
├── exception/
│   ├── CaminhoJaExistenteException.java
│   ├── CaminhoNaoEncontradoException.java
│   ├── OperacaoInvalidaException.java
│   └── PermissaoException.java
├── filesys/
│   ├── FileSystem.java       # Implementação do sistema de arquivos
│   ├── IFileSystem.java      # Interface principal
│   ├── Offset.java           # Classe para controle de posição de leitura
│   └── Usuario.java          # Classe de usuário
└── Main.java                 # Interface de linha de comando
```

## Como Executar

1. Certifique-se de ter o Java JDK instalado (versão 8 ou superior)
2. Compile todos os arquivos .java:
   ```
   javac Main.java
   ```
3. Execute o programa informando o usuário:
   ```
   java Main root
   ```

## Instruções de Uso

### Pré-requisitos
- O sistema requer um arquivo `users/users` com a lista de usuários e permissões iniciais
- Formato do arquivo:
  ```
  username /** rwx
  ```

### Comandos Disponíveis

```
1. chmod - Alterar permissões
2. mkdir - Criar diretório
3. rm - Remover arquivo/diretório
4. touch - Criar arquivo
5. write - Escrever em arquivo
6. read - Ler arquivo
7. mv - Mover/renomear arquivo
8. ls - Listar diretório
9. cp - Copiar arquivo
0. exit - Sair
```

### Exemplos de Uso

1. **Criar um diretório**:
   ```
   Digite o comando desejado: 2
   Insira o caminho do diretório a ser criado: /documentos
   ```

2. **Criar um arquivo**:
   ```
   Digite o comando desejado: 4
   Insira o caminho do arquivo a ser criado: /documentos/relatorio.txt
   ```

3. **Escrever em um arquivo**:
   ```
   Digite o comando desejado: 5
   Insira o caminho do arquivo a ser escrito: /documentos/relatorio.txt
   Anexar? (true/false): false
   Insira o conteúdo a ser escrito: Este é um relatório importante.
   ```

4. **Ler um arquivo**:
   ```
   Digite o comando desejado: 6
   Insira o caminho do arquivo a ser lido: /documentos/relatorio.txt
   ```

5. **Alterar permissões**:
   ```
   Digite o comando desejado: 1
   Insira o caminho do arquivo ou diretório: /documentos/relatorio.txt
   Insira o usuário para o qual deseja alterar as permissões: maria
   Insira a permissão (formato: 3 caracteres "rwx"): rw-
   ```

## Considerações Importantes

1. O usuário `root` é especial e tem todas as permissões
2. O sistema é volátil - todos os dados são perdidos ao encerrar a execução
3. As permissões seguem o padrão Unix (r=leitura, w=escrita, x=execução)
4. Operações que requerem permissão falharão se o usuário não tiver os privilégios necessários

## Tratamento de Erros

O sistema lança exceções específicas para diferentes cenários de erro:
- `CaminhoNaoEncontradoException`: Quando um caminho não existe
- `CaminhoJaExistenteException`: Quando tenta criar um arquivo/diretório que já existe
- `PermissaoException`: Quando o usuário não tem permissão para a operação
- `OperacaoInvalidaException`: Quando a operação não é válida para o tipo de arquivo

## Limitações Conhecidas

1. Tamanho fixo do buffer de leitura (256 bytes)
2. Não persiste dados entre execuções
3. Interface textual básica
