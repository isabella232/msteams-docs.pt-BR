---
title: Criar uma guia pessoal com o ASP. NET Core MVC
author: laujan
description: Um guia de início rápido para criar uma guia pessoal personalizada com o ASP. NET Core MVC.
ms.topic: quickstart
ms.author: lajanuar
ms.openlocfilehash: 7fcb0862647dec15bc93eecf9ce637d52892825c
ms.sourcegitcommit: e8dfcb167274e996395b77d65999991a18f2051a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2020
ms.locfileid: "47818910"
---
# <a name="create-a-custom-personal-tab-with-asp-net-core-mvc"></a>Criar uma guia pessoal personalizada com o ASP. NET Core MVC

Neste QuickStart, veremos como criar uma guia pessoal personalizada com C# e ASP. NET Core MVC. Também usaremos o [app Studio para o Microsoft Teams](~/concepts/build-and-test/app-studio-overview.md) para finalizar o manifesto do aplicativo e implantar sua guia no Teams.

[!INCLUDE [dotnet-core-prereq](~/includes/tabs/dotnet-core-prereq.md)]

## <a name="get-the-source-code"></a>Obter o código-fonte

Abra um prompt de comando e crie um novo diretório para o projeto de tabulação. Fornecemos um projeto simples para ajudá-lo a começar. Para recuperar o código-fonte, você pode baixar a pasta zip e extrair os arquivos ou clonar o repositório de exemplo no novo diretório:

``` bash
git clone https://github.com/OfficeDev/microsoft-teams-sample-tabs.git
```

Depois de ter o código-fonte, abra o Visual Studio e selecione **abrir um projeto ou solução**. Navegue até o diretório do aplicativo guia e abra **PersonalTabMVC. sln**.

Para compilar e executar o aplicativo, pressione **F5** ou escolha **Iniciar Depuração** no menu **depurar** . Em um navegador, navegue até as URLs abaixo para verificar se o aplicativo foi carregado corretamente:

* `http://localhost:44335`
* `http://localhost:44335/privacy`
* `http://localhost:44335/tou`

## <a name="review-the-source-code"></a>Examinar o código-fonte

### <a name="startupcs"></a>Startup.cs

Este projeto foi criado a partir de um ASP. Modelo vazio do aplicativo Web do NET Core 2,2 com a caixa de seleção *avançado-configurar para https* selecionada na instalação. Os serviços do MVC são registrados pelo método da estrutura de injeção de dependência `ConfigureServices()` . Além disso, o modelo vazio não habilita o fornecimento de conteúdo estático por padrão, portanto, o middleware de arquivos estáticos é adicionado ao `Configure()` método:

``` csharp
public void ConfigureServices(IServiceCollection services)
  {
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
  }
public void Configure(IApplicationBuilder app)
  {
    app.UseStaticFiles();
    app.UseMvc();
  }
```

### <a name="wwwroot-folder"></a>pasta wwwroot

No ASP. NET Core, a pasta raiz da Web é onde o aplicativo procura arquivos estáticos.

### <a name="appmanifest-folder"></a>Pasta arquivo AppManifest

Esta pasta contém os seguintes arquivos de pacote de aplicativos necessários:

* Um **ícone de cor completa** medindo 192 x 192 pixels.
* Um **ícone de contorno transparente** medindo 32 x 32 pixels.
* Um **manifest.jsno** arquivo que especifica os atributos do seu aplicativo.

Esses arquivos precisam ser zipados em um pacote de aplicativos para uso no carregamento de sua guia para o Microsoft Teams. O Microsoft Teams carregará o `contentUrl` especificado no manifesto, o incorporará em um iframe e o renderizará na sua guia.

### <a name="csproj"></a>. csproj

Na janela do Visual Studio Solution Explorer, clique com o botão direito do mouse no projeto e selecione **Editar arquivo de projeto**. Na parte inferior do arquivo, você verá o código que cria e atualiza sua pasta zip quando o aplicativo é criado:

``` xml
<PropertyGroup>
    <PostBuildEvent>powershell.exe Compress-Archive -Path \"$(ProjectDir)AppManifest\*\" -DestinationPath \"$(TargetDir)tab.zip\" -Force</PostBuildEvent>
  </PropertyGroup>

  <ItemGroup>
    <EmbeddedResource Include="AppManifest\icon-outline.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\icon-color.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\manifest.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
  </ItemGroup>
```

### <a name="models"></a>Modelo

*PersonalTab.cs* apresenta um objeto Message e métodos que serão chamados de *PersonalTabController* quando um usuário seleciona um botão no modo de exibição do *PersonalTab* .

### <a name="views"></a>Modos de exibição

#### <a name="home"></a>Home

Pelas. NET Core trata os arquivos denominados *index* como o padrão/home page do site. Quando a URL do navegador apontar para a raiz do site, *index. cshtml* será exibido como a home page do seu aplicativo.

#### <a name="shared"></a>Compartilhados

A marcação de exibição parcial *_Layout. cshtml* contém a estrutura de página Geral do aplicativo e elementos visuais compartilhados. Ele também fará referência à biblioteca do teams.

### <a name="controllers"></a>Controladores

Os controladores usam a propriedade ViewBag para transferir valores dinamicamente para os modos de exibição.

[!INCLUDE [dotnet-update-personal-app](~/includes/tabs/dotnet-update-personal-app.md)]

[!INCLUDE [dotnet-ngrok-intro](~/includes/tabs/dotnet-ngrok-intro.md)]

* Abra um prompt de comando na raiz do diretório do projeto e execute o seguinte comando:

``` bash
ngrok http https://localhost:44345 -host-header="localhost:44345"
```

* O Ngrok ouvirá as solicitações da Internet e as roteará para seu aplicativo quando estiver em execução na porta 44325.  Deve ser parecido `https://y8rPrT2b.ngrok.io/` com o local em que o *y8rPrT2b* é substituído pela URL https do ngrok alfanumérico.

* Certifique-se de manter o prompt de comando com o ngrok em execução e tome nota da URL — você precisará dela mais tarde.

* Verifique se o *ngrok* está em execução e funcionando corretamente abrindo o navegador e acessando a página de conteúdo por meio da URL https do ngrok que foi fornecida na janela de prompt de comando.

> [! Dica] você precisa ter o aplicativo no Visual Studio e o ngrok em execução para concluir este QuickStart. Se você precisar parar a execução do aplicativo no Visual Studio para trabalhar nele, **Mantenha o ngrok em execução**. Ele continuará a escutar e retomará o roteamento da solicitação do aplicativo quando ele for reiniciado no Visual Studio. Se for necessário reiniciar o serviço do ngrok, ele retornará uma nova URL e você terá que atualizar todos os locais que usam essa URL.

### <a name="run-your-application"></a>Executar o aplicativo

* No Visual Studio, pressione **F5** ou escolha **Iniciar Depuração** no menu **depurar** do aplicativo.

[!INCLUDE [dotnet-personal-use-appstudio](~/includes/tabs/dotnet-personal-use-appstudio.md)]
