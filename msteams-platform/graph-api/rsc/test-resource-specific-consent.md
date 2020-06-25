---
title: Testando o consentimento específico do recurso no Teams
description: Detalhes de teste de consentimento específico do recurso no Teams usando o postmaster
localization_priority: Normal
author: laujan
ms.author: lajanuar
ms.topic: How-to
keywords: Gráfico de postagem do AAD RSC do Microsoft Teams Authorization SSO
ms.openlocfilehash: c1c02c2ba0051193aa459d0df26fadfc9fa55550
ms.sourcegitcommit: fdc50183f3f4bec9e4b83bcfe5e016b591402f7c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "44867099"
---
# <a name="test-resource-specific-consent-permissions--in-teams"></a><span data-ttu-id="4bff7-104">Testar permissões de consentimento específicas do recurso no Teams</span><span class="sxs-lookup"><span data-stu-id="4bff7-104">Test resource-specific consent permissions  in Teams</span></span>

<span data-ttu-id="4bff7-105">O consentimento específico de recurso (RSC) é uma integração da API do Microsoft Teams e do Graph que permite que seu aplicativo use pontos de extremidade da API para gerenciar equipes específicas dentro de uma organização.</span><span class="sxs-lookup"><span data-stu-id="4bff7-105">Resource-specific consent (RSC) is a Microsoft Teams and Graph API integration that enables your app to use API endpoints to manage specific teams within an organization.</span></span> <span data-ttu-id="4bff7-106">*Confira*[o consentimento específico de recurso (RSC) — API do Microsoft Teams Graph](resource-specific-consent.md).  </span><span class="sxs-lookup"><span data-stu-id="4bff7-106">Please *see*  [Resource-specific consent (RSC) — Microsoft Teams Graph API](resource-specific-consent.md).</span></span>

> [!NOTE]
><span data-ttu-id="4bff7-107">Para testar as permissões de RSC, seu arquivo de manifesto de aplicativo do Microsoft Teams deve incluir uma chave de **webApplicationInfo** preenchida com os seguintes campos:</span><span class="sxs-lookup"><span data-stu-id="4bff7-107">To test the RSC permissions, your Teams app manifest file must include a **webApplicationInfo** key populated with the following fields:</span></span>
>
> - <span data-ttu-id="4bff7-108">**ID** — sua ID de aplicativo do Azure AD, *consulte* [registrar seu aplicativo no portal do Azure ad](resource-specific-consent.md#register-your-app-with-microsoft-identity-platform-via-the-azure-ad-portal).</span><span class="sxs-lookup"><span data-stu-id="4bff7-108">**id**  — your Azure AD app id, *see* [Register your app in the Azure AD portal](resource-specific-consent.md#register-your-app-with-microsoft-identity-platform-via-the-azure-ad-portal).</span></span>
> - <span data-ttu-id="4bff7-109">**recurso** — qualquer cadeia de caracteres, *consulte* a observação em [atualizar seu manifesto de aplicativo do Microsoft Teams](resource-specific-consent.md#update-your-teams-app-manifest)</span><span class="sxs-lookup"><span data-stu-id="4bff7-109">**resource**  — any string, *see* the note in  [Update your Teams app manifest](resource-specific-consent.md#update-your-teams-app-manifest)</span></span>
> - <span data-ttu-id="4bff7-110">**permissões de aplicativo** – permissões de RSC para seu aplicativo, *consulte* [permissões específicas do recurso](resource-specific-consent.md#resource-specific-permissions).</span><span class="sxs-lookup"><span data-stu-id="4bff7-110">**application permissions** — RSC permissions for  your app, *see* [Resource-specific Permissions](resource-specific-consent.md#resource-specific-permissions).</span></span>

```json
"webApplicationInfo":{
      "id":"XXxxXXXXX-XxXX-xXXX-XXxx-XXXXXXXxxxXX",
      "resource":"https://AnyString",
      "applicationPermissions":[
         "TeamSettings.Read.Group",
         "ChannelMessage.Read.Group",
         "TeamSettings.Edit.Group",
         "ChannelSettings.Edit.Group",
         "Channel.Create.Group",
         "Channel.Delete.Group",
         "TeamsApp.Read.Group",
         "TeamsTab.Read.Group",
         "TeamsTab.Create.Group",
         "TeamsTab.Edit.Group",
         "TeamsTab.Delete.Group",
         "Member.Read.Group",
         "Owner.Read.Group"
      ]
   }
```

>[!IMPORTANT]
><span data-ttu-id="4bff7-111">No manifesto do aplicativo, inclua apenas as permissões RSC que você deseja que seu aplicativo tenha.</span><span class="sxs-lookup"><span data-stu-id="4bff7-111">In your app manifest, only include the RSC permissions that you want your app to have.</span></span>

## <a name="test-added-rsc-permissions-using-the-postman-app"></a><span data-ttu-id="4bff7-112">Teste adicionado permissões de RSC usando o aplicativo postmaster</span><span class="sxs-lookup"><span data-stu-id="4bff7-112">Test added RSC permissions using the Postman app</span></span>

<span data-ttu-id="4bff7-113">Para verificar se as permissões de RSC estão sendo atendidas pela carga da solicitação de API, você precisará copiar o [código de teste JSON do RSC](test-rsc-json-file.md) para o seu ambiente local e atualizar os seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="4bff7-113">To check whether the RSC permissions are being honored by the API request payload, you'll need to copy the [RSC JSON test code](test-rsc-json-file.md) into your local environment and update the following values:</span></span>

1. <span data-ttu-id="4bff7-114">`azureADAppId`— ID de aplicativo do Azure AD do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4bff7-114">`azureADAppId`  — your app's Azure AD app id.</span></span>
1. <span data-ttu-id="4bff7-115">`azureADAppSecret`— seu segredo de aplicativo do Azure AD (senha)</span><span class="sxs-lookup"><span data-stu-id="4bff7-115">`azureADAppSecret`  — your Azure AD app secret (password)</span></span>
1. <span data-ttu-id="4bff7-116">`teamGroupId`— Você pode obter a ID do grupo de equipe do cliente do teams da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4bff7-116">`teamGroupId` — you can get the team group id from the Teams client as follows:</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="4bff7-117">No cliente do Microsoft Teams, selecione **equipes** na barra de navegação da extrema esquerda.</span><span class="sxs-lookup"><span data-stu-id="4bff7-117">In the Teams client, select **Teams** from the far left nav bar .</span></span>
> * <span data-ttu-id="4bff7-118">Selecione a equipe onde o aplicativo é instalado no menu suspenso.</span><span class="sxs-lookup"><span data-stu-id="4bff7-118">Select the team where the app is installed from the dropdown menu.</span></span>
> * <span data-ttu-id="4bff7-119">Selecionar o ícone **mais opções** (&#8943;)</span><span class="sxs-lookup"><span data-stu-id="4bff7-119">Select the **More options** icon (&#8943;)</span></span>
> * <span data-ttu-id="4bff7-120">Selecione **obter link para a equipe**</span><span class="sxs-lookup"><span data-stu-id="4bff7-120">Select **Get link to team**</span></span> 
> * <span data-ttu-id="4bff7-121">Copie e salve o valor de **GroupId** da cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4bff7-121">Copy and save the **groupId** value from the string.</span></span>

### <a name="using-postman"></a><span data-ttu-id="4bff7-122">Usando o postmaster</span><span class="sxs-lookup"><span data-stu-id="4bff7-122">Using Postman</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="4bff7-123">Abra o aplicativo [postmaster](https://www.postman.com) .</span><span class="sxs-lookup"><span data-stu-id="4bff7-123">Open the [Postman](https://www.postman.com) app.</span></span>
> * <span data-ttu-id="4bff7-124">Selecione **arquivo**  =>  **importar**  =>  **Importar arquivo** para carregar o arquivo JSON atualizado de seu ambiente.</span><span class="sxs-lookup"><span data-stu-id="4bff7-124">Select **File** => **Import** => **Import file** to upload the updated JSON file from your environment.</span></span>  
> * <span data-ttu-id="4bff7-125">Selecione a guia **coleções** .</span><span class="sxs-lookup"><span data-stu-id="4bff7-125">Select the **Collections** tab.</span></span> 
> * <span data-ttu-id="4bff7-126">Selecione a divisa (>) ao lado do **teste RSC** para expandir o modo de exibição de detalhes e ver as solicitações da API.</span><span class="sxs-lookup"><span data-stu-id="4bff7-126">Select the chevron (>) next to the **Test RSC** to expand the details view and see the API requests.</span></span>

<span data-ttu-id="4bff7-127">Execute toda a coleção Permissions para cada chamada de API.</span><span class="sxs-lookup"><span data-stu-id="4bff7-127">Execute the entire permissions collection for each API call.</span></span> <span data-ttu-id="4bff7-128">As permissões que você especificou em seu manifesto de aplicativo devem ser bem-sucedidas, enquanto aquelas não especificadas devem falhar com um código de status HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="4bff7-128">The permissions that you specified in your app manifest should succeed, while those not specified should fail with an HTTP 403 status code.</span></span> <span data-ttu-id="4bff7-129">Verifique todos os códigos de status de resposta para confirmar que o comportamento das permissões de RSC em seu aplicativo atende às expectativas.</span><span class="sxs-lookup"><span data-stu-id="4bff7-129">Check all of the response status codes to confirm that the behavior of the RSC permissions in your app meet expectations.</span></span>

>[!NOTE]
><span data-ttu-id="4bff7-130">Para testar chamadas específicas de API de exclusão e leitura, adicione esses cenários de instância ao arquivo JSON.</span><span class="sxs-lookup"><span data-stu-id="4bff7-130">To test specific DELETE and READ API calls, please add those instance scenarios to the JSON file.</span></span>

## <a name="test--revoked-rsc-permissions-using-postman"></a><span data-ttu-id="4bff7-131">Testar permissões RSC revogadas usando [postmaster](https://www.postman.com/)</span><span class="sxs-lookup"><span data-stu-id="4bff7-131">Test  revoked RSC permissions using [Postman](https://www.postman.com/)</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="4bff7-132">Desinstale o aplicativo da equipe específica.</span><span class="sxs-lookup"><span data-stu-id="4bff7-132">Uninstall the app from the specific team.</span></span>
> * <span data-ttu-id="4bff7-133">Siga as etapas acima para [testar permissões de RSC adicionadas usando o postmaster](#test-added-rsc-permissions-using-the-postman-app).</span><span class="sxs-lookup"><span data-stu-id="4bff7-133">Follow the steps above for [Test added RSC permissions using Postman](#test-added-rsc-permissions-using-the-postman-app).</span></span>
> * <span data-ttu-id="4bff7-134">Verifique todos os códigos de status de resposta para confirmar que as chamadas de API específicas que tiveram êxito falharam com um código de status HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="4bff7-134">Check all of the response status codes to confirm that the specific API calls that succeeded have failed with an HTTP 403 status code.</span></span>

> [!div class="nextstepaction"]
>
> [<span data-ttu-id="4bff7-135">Saiba mais sobre a API do Graph e o Teams</span><span class="sxs-lookup"><span data-stu-id="4bff7-135">Learn more about the Graph API and Teams</span></span>](/graph/api/resources/teams-api-overview?view=graph-rest-1.0)