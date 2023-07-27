# php-graph-tutorial
Aplicativo de console PHP que usa o Microsoft API do Graph para acessar dados em nome de um usuário.

## Pré-requisitos
Você deve ter PHP e Composer instalados em seu computador de desenvolvimento.

Você também deve ter uma conta de trabalho ou de estudante da Microsoft com uma caixa de correio Exchange Online. Se você não tiver uma conta microsoft, poderá [se inscrever no Programa de Desenvolvedores do Microsoft 365](https://developer.microsoft.com/microsoft-365/dev-program) para obter uma assinatura gratuita do Microsoft 365.

<details>
<summary>Observações</summary>
  Este tutorial foi escrito com PHP versão 8.1.5 e Composer versão 2.3.5. As etapas neste guia podem funcionar com outras versões, mas isso não foi testado.
</details>

## Registrar o aplicativo no portal
Você registrará um novo aplicativo no Azure Active Directory para habilitar a autenticação do usuário. Você pode registrar um aplicativo usando o centro de administração do Azure Active Directory.

1. Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com/) e faça logon usando uma **conta de trabalho ou de estudante**.
2. Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.

    ![Azure-Registro](https://learn.microsoft.com/pt-br/graph/tutorials/images/aad-portal-app-registrations.png)

3. Selecione **Novo registro**. Insira um nome para seu aplicativo, por exemplo, `Graph User Auth Tutorial`.
4. Defina **tipos de conta com suporte** conforme desejado. As opções são:

    |Opção|Quem pode entrar?|
    |-----|-----------------|
    |Contas apenas neste diretório organizacional	|Somente usuários em sua organização do Microsoft 365|
    |Contas em qualquer diretório organizacional	|Usuários em qualquer organização do Microsoft 365 (contas corporativas ou escolares)|
    |Contas em qualquer diretório organizacional... e contas pessoais da Microsoft	|Usuários em qualquer organização do Microsoft 365 (contas corporativas ou escolares) e contas pessoais da Microsoft|

5. Deixe o **URI de Redirecionamento** vazio.
6. Selecione **Registrar**. Na página **Visão geral** do aplicativo, copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa. Se você escolheu **Contas neste diretório organizacional apenas** para tipos de **conta com suporte**, copie também a **ID do Diretório (locatário)** e salve-a.

    ![App ID](https://learn.microsoft.com/pt-br/graph/tutorials/images/aad-application-id.png)

7. Selecione **Autenticação** em **Gerenciar**. Localize a seção **Configurações avançadas** e **altere a alternância Permitir fluxos de cliente público** para Sim e escolha **Salvar**.

    ![Fluxos de cliente](https://learn.microsoft.com/pt-br/graph/tutorials/images/aad-default-client-type.png)

## Restaurar pacotes com o composer
Abra sua CLI (interface de linha de comando) no diretório em que você baixou este projeto. Execute o seguinte comando:

``` Bash
composer install
```

## Configurações do aplicativo
Nesta seção, você adicionará os detalhes do registro do aplicativo ao projeto.

1. No diretório raiz do seu projeto, adicione o código a seguir.
``` Bash
cp local.env .env
```
2. Editar o arquivo `.env`, atualize os valores conforme a tabela a seguir

    |Configuração|Valor|
    |-----|-----------------|
    |`CLIENT_ID`|A ID do cliente do registro do aplicativo|
    |`TENANT_ID`|Se você escolheu a opção para permitir apenas que usuários em sua organização entrem, altere esse valor para sua ID de locatário. Caso contrário, deixe como `common`.|

## Executar o aplicativo

1. Execute o aplicativo. Insira `1` quando solicitado para uma opção. O aplicativo exibe uma URL e um código de dispositivo.

```Bash
$ php main.php

PHP Graph Tutorial

Please choose one of the following options:
0. Exit
1. Display access token
2. List my inbox
3. Send mail
4. Make a Graph call
1
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and
enter the code RB2RUD56D to authenticate.
```

2. Abra um navegador e navegue até a URL exibida. Insira o código fornecido e entre.

<details>
<summary>Importante</summary>
  Fique atento a todas as contas existentes do Microsoft 365 que são registradas no navegador ao navegar para https://microsoft.com/devicelogin. Use recursos do navegador, como perfis, modo convidado ou modo privado para garantir que você se autentique como a conta que pretende usar para teste.
</details>