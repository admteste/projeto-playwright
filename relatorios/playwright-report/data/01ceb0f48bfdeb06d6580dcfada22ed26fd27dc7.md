# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: login-usuario.spec.js >> Caso de teste - Login de usuários >> Não deve autenticar usuário com credenciais inválidas.
- Location: tests\login-usuario.spec.js:37:9

# Error details

```
Test timeout of 30000ms exceeded.
```

```
Error: expect(locator).toBeVisible() failed

Locator:  getByText('E-mail ou senha inválidos.')
Expected: visible
Received: undefined

Call log:
  - Expect "toBeVisible" with timeout 5000ms
  - waiting for getByText('E-mail ou senha inválidos.')
  - 

```

# Page snapshot

```yaml
- main [ref=e2]:
  - region "Apresentação do sistema" [ref=e3]:
    - generic [ref=e4]:
      - generic [ref=e5]: BS
      - generic [ref=e6]:
        - heading "BlueStock" [level=1] [ref=e7]
        - paragraph [ref=e8]: Controle inteligente de produtos
    - generic [ref=e9]:
      - generic [ref=e10]: Sistema para testes com Cypress
      - heading "Gerencie produtos com uma interface moderna, rápida e responsiva." [level=2] [ref=e11]
      - paragraph [ref=e12]: Use este projeto em sala para praticar automação de testes em fluxos de login, cadastro, consulta, edição e exclusão.
  - region "Formulário de login" [ref=e13]:
    - heading "Entrar no sistema" [level=2] [ref=e14]
    - paragraph [ref=e15]: Acesse sua conta para gerenciar os produtos.
    - alert [ref=e16]: E-mail ou senha inválidos.
    - generic [ref=e17]:
      - generic [ref=e18]: E-mail
      - textbox "E-mail" [ref=e19]:
        - /placeholder: admin@bluestock.com
        - text: teste@email.com
      - generic [ref=e20]: Senha
      - textbox "Senha" [ref=e21]:
        - /placeholder: Digite sua senha
        - text: Teste@2026
      - button "Entrar" [active] [ref=e22] [cursor=pointer]
    - paragraph [ref=e23]:
      - text: Ainda não tem conta?
      - link "Criar usuário" [ref=e24] [cursor=pointer]:
        - /url: cadastro.html
    - paragraph [ref=e25]:
      - text: "Usuário inicial:"
      - strong [ref=e26]: admin@bluestock.com
      - text: /
      - strong [ref=e27]: Admin@123
```

# Test source

```ts
  1  | import { test, expect } from '@playwright/test';
  2  | 
  3  | //mapeando a URL da página de login do sistema
  4  | const URL_LOGIN = 'https://sergiocotiazure20265-ops.github.io/produtos-cypress-site/index.html';
  5  | const URL_PRODUTOS = 'https://sergiocotiazure20265-ops.github.io/produtos-cypress-site/produtos.html';
  6  | 
  7  | //Criar o caso de teste
  8  | test.describe('Caso de teste - Login de usuários', () => {
  9  | 
  10 |     //Função executada antes de cada cenário de teste
  11 |     test.beforeEach(async ({ page }) => {
  12 |         //Abrir a página de login do sistema
  13 |         await page.goto(URL_LOGIN);
  14 |     });
  15 | 
  16 |     //Cenário de teste
  17 |     test('Deve realizar login com sucesso para usuário válido.', async({ page }) => {
  18 |         
  19 |         //Preencher os campos de login e senha corretos
  20 |         await page.locator('#emailLogin').fill('admin@bluestock.com');
  21 |         await page.locator('#senhaLogin').fill('Admin@123');
  22 | 
  23 |         //Clicar no botão de login
  24 |         await page.locator('#btnEntrar').click();
  25 | 
  26 |         //Verificar se o usuário foi autenticado
  27 |         await expect(page).toHaveURL(URL_PRODUTOS);
  28 | 
  29 |         //Gerar evidência
  30 |         await page.screenshot({
  31 |             path: 'evidencias/login-valido.png',
  32 |             fullPage: true
  33 |         });
  34 |     });
  35 | 
  36 |     //Cenário de teste
  37 |     test('Não deve autenticar usuário com credenciais inválidas.', async({ page }) => {
  38 | 
  39 |         //Preencher os campos de login e senha inválidos
  40 |         await page.locator('#emailLogin').fill('teste@email.com');
  41 |         await page.locator('#senhaLogin').fill('Teste@2026');
  42 | 
  43 |         //Clicar no botão de login
  44 |         await page.locator('#btnEntrar').click();
  45 | 
  46 |         //Verificar se o sistema exibe mensagem de erro
  47 |         await expect(
  48 |             page.getByText('E-mail ou senha inválidos.')
> 49 |         ).toBeVisible();
     |           ^ Error: expect(locator).toBeVisible() failed
  50 | 
  51 |         //Gerar evidência
  52 |         await page.screenshot({
  53 |             path: 'evidencias/acesso-negado.png',
  54 |             fullPage: true
  55 |         });
  56 |     });
  57 | 
  58 | });
  59 | 
```