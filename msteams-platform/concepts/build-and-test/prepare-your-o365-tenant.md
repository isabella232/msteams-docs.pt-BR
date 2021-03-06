---
title: Prepare seu locatário do Microsoft 365
description: Como começar a usar o Teams no Microsoft 365
keywords: Configurar o carregamento de Microsoft 365 locatário Teams
ms.openlocfilehash: 67a0342a7e8605097eed53dd1b0bdf273d537c0e
ms.sourcegitcommit: d61f14053fc695bc1956bf50e83956613c19ccca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2020
ms.locfileid: "48452761"
---
# <a name="prepare-your-microsoft-365-tenant"></a>Prepare seu locatário do Microsoft 365

Se você for assinante do Microsoft 365, poderá desenvolver aplicativos para o Microsoft Teams com um dos seguintes [planos](https://products.office.com/business/compare-more-office-365-for-business-plans):

* Básica
* Padrão
* Enterprise E1, E3 e e5
* Developer
* Education, Education Plus e Education e5

O Microsoft Teams também estará disponível para clientes que se inscreveram no E4 antes da sua [aposentadoria](https://support.office.com//article/important-information-for-office-365-enterprise-e4-customers-f9572348-43a2-43fa-a3d8-3b6c9c042147).

## <a name="just-need-a-development-environment"></a>Precisa apenas de um ambiente de desenvolvimento?

Se você não tiver uma conta do Microsoft 365, você pode se inscrever para uma assinatura do [microsoft 365 Developer Program](https://developer.microsoft.com/microsoft-365/dev-program) . É *grátis* por 90 dias e será renovado continuamente, desde que você o esteja usando para a atividade de desenvolvimento. Se você tiver uma assinatura do Visual Studio *Enterprise* ou *Professional* , ambos os programas incluirão uma [assinatura](https://aka.ms/MyVisualStudioBenefits)gratuita de desenvolvedor do Microsoft 365, ativa pela vida da sua assinatura do Visual Studio. *Confira* [Configurar uma assinatura de desenvolvedor do Microsoft 365](https://docs.microsoft.com/office/developer-program/office-365-developer-program-get-started).

## <a name="enable-microsoft-teams-for-your-organization"></a>Habilitar o Microsoft Teams para sua organização 

Se o Microsoft Teams não tiver sido habilitado para sua organização, você precisará fazer isso primeiro. Confira nossa orientação detalhada para [habilitar o Microsoft Teams para sua organização](/microsoftteams/enable-features-office-365).

## <a name="enable-custom-teams-apps-and-turn-on-custom-app-uploading"></a>Habilitar aplicativos personalizados do Teams e ativar o carregamento de aplicativos personalizados

Ative o aplicativo de Sideload personalizado para o seu locatário do desenvolvedor da seguinte maneira:

1. Faça logon no [centro de administração do Microsoft 365](https://admin.microsoft.com/Adminportal/Home?source=applauncher#/homepage#/) com sua credencial de administrador. 

2. Selecione **Mostrar todas as**  -->  **equipes**. 

![imagem do menu da central de administração](~/assets/images/prepare-test-tenant/admin-center.png)

> [!Note] 
> Pode levar até 24 horas para que a opção "equipes" seja exibida. Durante o interim, você pode [carregar seu aplicativo personalizado para um ambiente do teams](/microsoftteams/upload-custom-apps#validate) para teste e validação.

3. Navegar para políticas de instalação de **aplicativos do teams**  -->  **Setup Policies**  -->  **global (padrão para toda a organização)**  

![ativar o modo de exibição Sideload](~/assets/images/prepare-test-tenant/turn-on-sideload.png)

4. Alternar **carregar aplicativos personalizados** para a posição **ativado** .

Isso é tudo. Agora, seu locatário de teste permitirá o Sideload de aplicativos personalizados.

> [!Note] 
> Pode levar até 24 horas antes de o Sideload ser habilitado. Durante o interim, você pode usar **upload \<your tenant> para** para testar seu aplicativo.

![exibição do aplicativo updload](~/assets/images/prepare-test-tenant/upload-for-contoso.png)

Para obter informações completas sobre como essas configurações interagem, *consulte*, [gerenciar políticas e configurações personalizadas de aplicativos no Microsoft Teams](https://docs.microsoft.com/microsoftteams/teams-custom-app-policies-and-settings) e [gerenciar políticas de instalação de aplicativos no Microsoft Teams](https://docs.microsoft.com/microsoftteams/teams-app-setup-policies).
