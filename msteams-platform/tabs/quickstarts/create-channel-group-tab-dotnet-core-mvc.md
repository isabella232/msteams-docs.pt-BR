---
title: Criar uma guia de canal e grupo com o ASP.NET Core MVC
author: laujan
description: Um guia de início rápido para criar uma guia de canal e grupo personalizado com o ASP.NET Core MVC.
ms.topic: quickstart
ms.author: lajanuar
ms.openlocfilehash: cda91825ee37da94ee84747c5d2439c2940c728b
ms.sourcegitcommit: e8dfcb167274e996395b77d65999991a18f2051a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2020
ms.locfileid: "47818924"
---
# <a name="create-a-custom-channel-and-group-tab-with-aspnet-core-mvc"></a>Criar uma guia de canal e grupo personalizado com o ASP.NET Core MVC

Neste QuickStart, veremos como criar uma guia de canal/grupo personalizado com C# e ASP.Net Core MVC. Também usaremos o [app Studio para o Microsoft Teams](~/concepts/build-and-test/app-studio-overview.md) para finalizar o manifesto do aplicativo e implantar sua guia no Teams.

[!INCLUDE [dotnet-core-prereq](~/includes/tabs/dotnet-core-prereq.md)]

## <a name="get-the-source-code"></a>Obter o código-fonte

Abra um prompt de comando e crie um novo diretório para o projeto de tabulação. Fornecemos um projeto de [Guia de grupo de canais](https://github.com/OfficeDev/microsoft-teams-sample-tabs/ChannelGroupTabMVC) simples para você começar. Para recuperar o código-fonte, você pode baixar a pasta zip e extrair os arquivos ou clonar o repositório de exemplo no novo diretório:

```bash
git clone https://github.com/OfficeDev/microsoft-teams-sample-tabs.git
```

Depois de ter o código-fonte, abra o Visual Studio e selecione **abrir um projeto ou solução**. Navegue até o diretório do aplicativo guia e abra **ChannelGroupTabMVC. sln**.

Para compilar e executar o aplicativo, pressione **F5** ou escolha **Iniciar Depuração** no menu **depurar** . Em um navegador, navegue até as URLs abaixo e verifique se o aplicativo foi carregado corretamente:

- `http://localhost:44360`
- `http://localhost:44360/privacy`
- `http://localhost:44360/tou`

## <a name="review-the-source-code"></a>Examinar o código-fonte

### <a name="startupcs"></a>Startup.cs

Este projeto foi criado a partir de um modelo vazio do aplicativo Web do ASP.NET Core 2,2 com a caixa de seleção *avançado-configurar para https* selecionada na instalação. Os serviços do MVC são registrados pelo método da estrutura de injeção de dependência `ConfigureServices()` . Além disso, o modelo vazio não habilita o fornecimento de conteúdo estático por padrão, portanto, o middleware de arquivos estáticos é adicionado ao `Configure()` método:

```csharp
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

No ASP.NET Core, a pasta raiz da Web é onde o aplicativo procura arquivos estáticos.

### <a name="appmanifest-folder"></a>Pasta arquivo AppManifest

Esta pasta contém os seguintes arquivos de pacote de aplicativos necessários:

- Um **ícone de cor completa** medindo 192 x 192 pixels.
- Um **ícone de contorno transparente** medindo 32 x 32 pixels.
- Um **manifest.jsno** arquivo que especifica os atributos do seu aplicativo.

Esses arquivos precisam ser zipados em um pacote de aplicativos para uso no carregamento de sua guia para o Microsoft Teams.

### <a name="csproj"></a>. csproj

Na janela do Visual Studio Solution Explorer, clique com o botão direito do mouse no projeto e selecione **Editar arquivo de projeto**. Na parte inferior do arquivo, você verá o código que cria e atualiza sua pasta zip quando o aplicativo é criado:

```xml
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

*ChannelGroup.cs* apresenta um objeto Message e métodos que serão chamados dos controladores durante a configuração.

### <a name="views"></a>Modos de exibição

#### <a name="home"></a>Home

ASP.NET Core trata os arquivos denominados *index* como o padrão/home page do site. Quando a URL do navegador apontar para a raiz do site, **index. cshtml** será exibido como a home page do seu aplicativo.

#### <a name="shared"></a>Compartilhados

A marcação de exibição parcial *_Layout. cshtml* contém a estrutura de página Geral do aplicativo e elementos visuais compartilhados. Ele também fará referência à biblioteca do teams.

### <a name="controllers"></a>Controladores

Os controladores usam a propriedade ViewBag para transferir valores dinamicamente para os modos de exibição.

[!INCLUDE [dotnet-ngrok-intro](~/includes/tabs/dotnet-ngrok-intro.md)]

- Abra um prompt de comando na raiz do diretório do projeto e execute o seguinte comando:

```bash
ngrok http https://localhost:443560 -host-header="localhost:44360"
```

- O Ngrok ouvirá as solicitações da Internet e as roteará para seu aplicativo quando estiver em execução na porta 44355.  Deve ser parecido `https://y8rCgT2b.ngrok.io/` com o local em que o *y8rCgT2b* é substituído pela URL https do ngrok alfanumérico.

- Certifique-se de manter o prompt de comando com o ngrok em execução e tome nota da URL — você precisará dela mais tarde.

## <a name="update-your-application"></a>Atualizar seu aplicativo

Na **guia. cshtml** o aplicativo apresenta ao usuário dois botões de opção para exibir a guia com um ícone vermelho ou cinza. A seleção do botão **selecionar cinza** ou **selecionar vermelho** dispara `saveGray()` ou `saveRed()` , respectivamente, define `settings.setValidityState(true)` e habilita o botão **salvar** na página de configuração. Esse código permite que as equipes saibam que você atende aos requisitos de configuração e a instalação pode continuar. Ao salvar, os parâmetros de `settings.setSettings` são definidos. Por fim, `saveEvent.notifySuccess()` é chamado para indicar que a URL de conteúdo foi resolvida com êxito.

[!INCLUDE [dotnet-update-app](~/includes/tabs/dotnet-update-chan-grp-app.md)]

[!INCLUDE [dotnet-upload-to-teams](~/includes/tabs/dotnet-upload-to-teams.md)]
