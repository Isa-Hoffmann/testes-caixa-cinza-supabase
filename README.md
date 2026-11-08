# Testes Caixa Cinza — API de Autenticação Supabase

## Introdução

Este repositório documenta a configuração de um ambiente de autenticação no Supabase e a execução de testes caixa cinza utilizando o Postman Desktop.

A atividade verifica o comportamento da API por meio de requisições HTTP, análise dos códigos de status e interpretação das respostas JSON.

## Objetivo da Atividade

Configurar a autenticação por e-mail e senha no Supabase, preparar requisições no Postman e validar cenários positivos e negativos de login, registrando os resultados obtidos.

## Configuração do Supabase

Foi criado um projeto no Supabase para disponibilizar o serviço de autenticação. O provedor de autenticação por e-mail foi mantido habilitado.

Também foi criado um usuário de teste em **Authentication → Users**, utilizando o método de autenticação por e-mail e senha.

A URL base utilizada foi:

`https://bahaehecqqtzpratzmol.supabase.co`

Para realizar as requisições, foi utilizada a **Publishable key** do projeto. Por segurança, seu valor completo não é documentado neste repositório.

**Objetivo da configuração:** disponibilizar um serviço de autenticação para testar o fluxo de login sem desenvolver uma interface gráfica.

### Evidências

- Projeto Supabase: `prints/supabase/projeto.png`
- Provedor de e-mail habilitado: `prints/supabase/email-habilitado.png`
- Usuário de teste criado: `prints/supabase/usuario-criado.png`

## Configuração do Postman

Foi utilizado o Postman Desktop para organizar e executar as requisições.

Foi criado um Workspace, um Environment e uma coleção chamada **Teste de Autenticação Supabase**, contendo os cinco cenários de teste.

O Environment foi configurado com as seguintes variáveis:

| Variável | Finalidade |
|---|---|
| `base_url` | URL base do projeto Supabase |
| `api_key` | Publishable key do projeto |
| `email` | E-mail do usuário válido |
| `password` | Senha do usuário válido |
| `wrong_password` | Senha propositalmente incorreta |
| `nonexistent_email` | E-mail que não está cadastrado |

Os valores privados são mantidos localmente no Postman e não devem ser publicados no GitHub.

### Evidências

- Workspace: `prints/postman/workspace.png`
- Environment e variáveis: `prints/postman/environment.png`
- Coleção com os cinco cenários: `prints/postman/colecao.png`

## Configuração das Requisições

Foi utilizado o método **POST** no seguinte endpoint:

`{{base_url}}/auth/v1/token?grant_type=password`

URL completa:

`https://bahaehecqqtzpratzmol.supabase.co/auth/v1/token?grant_type=password`

### Headers

| Header | Valor |
|---|---|
| `apikey` | `{{api_key}}` |
| `Authorization` | `Bearer {{api_key}}` |
| `Content-Type` | `application/json` |

### Body JSON

A requisição utiliza o formato JSON para enviar o e-mail e a senha:

```json
{
  "email": "usuario@email.com",
  "password": "senha-do-usuario"
}
```

Em caso de autenticação bem-sucedida, a API retorna os dados da sessão, incluindo o `access_token`. Em caso de falha, retorna uma resposta de erro.

## Execução dos Testes

Foram executados cinco cenários de teste na coleção `postman/Autenticacao-Supabase.postman_collection.json`.

| ID | Cenário | Resultado esperado | Resultado obtido | Status |
|---|---|---|---|---|
| CT01 | Login válido | HTTP 200 e `access_token` | HTTP 200 — Login realizado com sucesso e token retornado. | APROVADO |
| CT02 | Senha incorreta | Erro de autenticação | HTTP 400 — `invalid_credentials`. | APROVADO |
| CT03 | Usuário inexistente | Acesso negado | HTTP 400 — Erro de autenticação. | APROVADO |
| CT04 | Campos vazios | Erro de validação | HTTP 400 — Requisição rejeitada. | APROVADO |
| CT05 | Credenciais inválidas | Mensagem de erro | HTTP 400 — Requisição rejeitada. | APROVADO |

Todos os cenários apresentaram o comportamento esperado: o acesso foi permitido somente com as credenciais válidas, enquanto as entradas incorretas foram rejeitadas.

### Evidências

As capturas das respostas obtidas no Postman estão organizadas em `prints/testes/`:

- `01-login-valido.png`
- `02-senha-incorreta.png`
- `03-usuario-inexistente.png`
- `04-campos-vazios.png`
- `05-credenciais-invalidas.png`

**Observação:** o valor do `access_token` deve permanecer oculto nas capturas publicadas.

## Registro dos Testes

Os resultados foram registrados na planilha `planilha/registro-testes.xlsx`.

A planilha contém os seguintes campos:

- ID do teste;
- Cenário;
- Entrada utilizada;
- Resultado esperado;
- Resultado obtido;
- Status;
- Observações.

O registro permite comparar o comportamento real da API com o esperado, identificar possíveis falhas e manter evidências da execução.

## Resultados Obtidos

Foram executados cinco testes, sendo:

- **5 testes aprovados;**
- **0 testes reprovados.**

O login válido retornou HTTP 200 e disponibilizou um `access_token`. Os quatro cenários negativos retornaram HTTP 400, impedindo a autenticação com dados incorretos ou incompletos.

Os resultados demonstraram que o serviço de autenticação respondeu conforme o esperado nos cenários avaliados.

## Conclusão

A atividade permitiu configurar e testar uma API de autenticação utilizando o Supabase e o Postman Desktop.

Os cinco cenários executados apresentaram resultados satisfatórios. Foi possível confirmar o funcionamento do login com credenciais válidas e verificar que a API rejeita tentativas com senha incorreta, usuário inexistente, campos vazios e credenciais inválidas.

Durante a execução, foi necessário configurar corretamente as variáveis de ambiente e os parâmetros das requisições para garantir a comunicação com a API.

A atividade também demonstrou a importância dos testes caixa cinza, pois foi possível avaliar o comportamento interno da comunicação com a API conhecendo informações como endpoint, método HTTP, headers e estrutura do body, sem depender de uma interface gráfica.

Como melhoria futura, poderiam ser acrescentados novos cenários de teste, incluindo validações de formato de e-mail, tratamento de erros e testes de segurança.

## Estrutura do Repositório

```text
.
├── README.md
├── postman/
│   ├── Autenticacao-Supabase.postman_collection.json
│   └── Supabase-Testes-postman_environment.json
├── prints/
│   ├── supabase/
│   ├── postman/
│   └── testes/
└── planilha/
    └── registro-testes.xlsx
```