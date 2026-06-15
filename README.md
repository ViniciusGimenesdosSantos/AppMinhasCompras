# MauiAppMinhasCompras

Aplicativo desenvolvido em **.NET MAUI** para a disciplina de Desenvolvimento Mobile da FATEC Jahu. É um gerenciador de "lista de compras", onde o usuário cadastra produtos com quantidade e preço, podendo consultar, buscar, somar e remover itens, com persistência local em banco de dados **SQLite**.

## Funcionamento do App

O app é composto por duas telas principais (`Views`), acessadas via navegação por pilha (`Navigation.PushAsync`):

### 1. Lista de Produtos (`ListaProdutos`)
Tela inicial do app, exibe os produtos cadastrados em uma `ListView`:

- Cada item mostra Id, Descrição, Preço, Quantidade e Total (calculado automaticamente).
- **Busca**: o `SearchBar` filtra os produtos em tempo real, consultando o banco com `LIKE`.
- **Pull to Refresh**: arrastar a lista para baixo recarrega os dados do banco.
- **Toolbar**: botão "Somar" exibe o valor total de todos os produtos da lista (`lista.Sum(...)`); botão "Adicionar" navega para a tela de cadastro.
- **Selecionar item**: ao tocar em um produto, abre a tela de cadastro já preenchida com os dados (modo edição), via `BindingContext`.
- **Remover item**: um `MenuItem` de contexto (`ContextActions`) permite excluir um produto após confirmação em `DisplayAlert`.

### 2. Cadastro/Edição de Produto (`CadastroProduto`)
Tela com campos de Descrição, Quantidade e Preço (vinculados via `{Binding}`):

- Se a tela for aberta sem um produto anexado (`BindingContext == null`), o botão "Salvar" da toolbar **insere** um novo produto no banco.
- Se houver um produto anexado (vindo da lista), os dados são **atualizados** (`Update`) usando o `Id` do produto original.
- Após salvar, exibe alerta de sucesso e retorna para a lista (`Navigation.PopAsync()`).

### Persistência (SQLite)
A classe `SQLiteDatabaseHelper` (em `Helpers/`) encapsula o acesso ao banco usando `SQLiteAsyncConnection`, oferecendo métodos `Insert`, `Update`, `Delete`, `GetAll` e `Search`. O banco é criado automaticamente em `Environment.SpecialFolder.LocalApplicationData`, acessado de forma global via propriedade estática `App.Db`.

## Conteúdos novos aprendidos

- Integração de **SQLite** em apps MAUI com a biblioteca `sqlite-net`, incluindo criação automática de tabelas (`CreateTableAsync`) a partir de uma classe `Model` anotada com `[PrimaryKey, AutoIncrement]`.
- Operações **CRUD assíncronas** (Create, Read, Update, Delete) encapsuladas em uma classe Helper, separando a camada de dados da camada de apresentação.
- Uso de **ListView** com `Header`, `ItemTemplate`/`DataTemplate`/`ViewCell` e `ContextActions` (menu de ações ao deslizar/pressionar um item).
- **ObservableCollection** para manter a lista da interface sincronizada automaticamente com as alterações feitas em código.
- Implementação de **Pull to Refresh** (`IsPullToRefreshEnabled`, `Refreshing`, `IsRefreshing`).
- **SearchBar** com evento `TextChanged` para realizar busca dinâmica no banco de dados (`LIKE`).
- Reutilização de uma mesma tela para **cadastro e edição**, alternando o comportamento com base na presença de um `BindingContext`.
- Configuração de cultura da aplicação (`CultureInfo("pt-BR")`) para formatação correta de números e moeda.
- Propriedades calculadas em Models (`Total => Quantidade * Preco`) exibidas diretamente via *data binding*.

## Tecnologias

- .NET 10 (MAUI)
- C# / XAML
- SQLite (sqlite-net-pcl)
- Plataformas suportadas: Android, iOS, MacCatalyst, Windows

## Como executar

1. Clone o repositório.
2. Abra o arquivo de solução (`.slnx`/`.sln`) no Visual Studio 2022 (ou superior) com a workload **.NET Multi-platform App UI** instalada.
3. Selecione o destino de execução (ex: Android Emulator ou Windows Machine).
4. Pressione **F5** ou clique em **Run** para compilar e executar o app. O banco de dados SQLite é criado automaticamente na primeira execução.
