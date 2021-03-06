---
title: Diretrizes de design para guias
description: Descreve as diretrizes para a criação de guias de conteúdo e colaboração
keywords: Teams design Guidelines Reference Framework Tabs Configuration guia canal de configuração guia estática guia de equipes de design simples
ms.openlocfilehash: 9ce72e97fa92e7d5db0fd51f29b2b905f378e788
ms.sourcegitcommit: 99c35de7e2c604bd8bce392242c2c2fa709cd50b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2020
ms.locfileid: "48931796"
---
# <a name="content-and-conversations-all-at-once-using-tabs"></a>Conteúdo e conversas, todos ao mesmo tempo usando guias

> [!Important]
> **Guias em clientes móveis**
>
> Siga as [orientações para guias em celular](./tabs-mobile.md) ao criar suas guias. Se sua guia usa autenticação, você deve atualizar o SDK do JavaScript do Microsoft Teams para a versão 1.4.1 ou posterior, ou a autenticação falhará.
>
> **Guias de canal/grupo (configurável) em dispositivos móveis:**
>
> * Os clientes móveis só mostram guias configuráveis com um valor para `websiteUrl` . Se quiser que a sua guia apareça nos clientes móveis do Microsoft Teams, você deve definir o valor de `websiteUrl` .
> * O comportamento de abertura padrão no Mobile é abrir fora do navegador usando o `websiteUrl` . Para aplicativos publicados na loja de aplicativos públicos, se você quiser que a guia de canal seja aberta no Teams por padrão, siga as [orientações para guias em dispositivos móveis](~/tabs/design/tabs-mobile.md)e entre em seu representante de suporte para solicitar a lista branca.

Guias são Canvases que você pode usar para compartilhar conteúdo, reter conversas e hospedar serviços de terceiros, tudo em um fluxo de trabalho orgânica da equipe. Quando você cria uma guia no Microsoft Teams, ele coloca seu aplicativo Web front e Center onde ele é facilmente acessível contra conversas principais.

## <a name="guidelines"></a>Diretrizes

Uma guia boa deve exibir as seguintes características:

### <a name="focused-functionality"></a>Funcionalidade prioritária

As guias funcionam melhor quando são criadas para atender a uma necessidade específica. Concentre-se em um pequeno conjunto de tarefas ou em um subconjunto de dados que é relevante para o canal em que a guia se encontra.

### <a name="reduced-chrome"></a>Cromo reduzido

Evite criar vários painéis em uma guia, adicionar camadas de navegação ou exigir que os usuários rolem verticalmente e horizontalmente em uma guia. Em outras palavras, tente não ter tabulações na guia.

### <a name="integration"></a>Integração

Encontre maneiras de notificar os usuários sobre a atividade de guia postando [cartões adaptáveis](../../task-modules-and-cards/what-are-cards.md#adaptive-cards) a uma conversa.

### <a name="conversational"></a>Coloquial

Encontre uma maneira de facilitar a conversa em torno de uma guia. Isso garante que o centro de conversas fique no conteúdo, nos dados ou no processo em mãos.

### <a name="streamlined-access"></a>Acesso simplificado

Certifique-se de que você está concedendo acesso às pessoas certas no momento certo. Manter o processo de entrada simples evitará a criação de barreiras para contribuição e colaboração.

### <a name="responsiveness-to-window-sizing"></a>Capacidade de resposta para dimensionamento de janela

As equipes podem ser usadas em tamanhos de janela tão pequenos quanto 720px, então garantir que uma guia possa ser usada em uma pequena janela seja tão importante quanto a usabilidade em resoluções muito altas.

### <a name="flat-navigation"></a>Navegação simples

Pedimos aos desenvolvedores não adicionar o portal inteiro a uma guia. Manter a navegação relativamente simples ajuda a manter um modelo de conversa mais simples. Em outras palavras, a conversa é sobre uma lista de coisas, como itens de trabalho triantigos, ou uma única coisa, como uma espec.

Há desafios de navegação inerentes com uma hierarquia de navegação profunda em conversas encadeadas. Para a melhor experiência do usuário, a navegação de guia deve ser mantida no mínimo e ser projetada da seguinte maneira:

> [!div class="checklist"]
>
> * **Abre um módulo de tarefa, como um item de trabalho individual ou uma entidade**. Isso impede totalmente o chat e é a melhor opção para manter o chat especificamente sobre a guia e não as subentidades ou experiências de edição.
>* **Abre uma pseudo caixa de diálogo em um iframe**. Se usado com um plano de fundo em tela, recomendamos usar a cor mais clara em vez do escuro. A `app-gray-10 at 30%` transparência funciona bem.
>* **Abre uma página do navegador**.

### <a name="personality"></a>Personalidade

A tela da guia apresenta uma ótima oportunidade de marcar sua experiência. O logotipo é uma parte importante da sua identidade e conexão com os usuários. portanto, não se esqueça de incluí-lo:

> [!div class="checklist"]
>
>* Coloque o logotipo no canto esquerdo ou direito ou ao longo da borda inferior
> * Mantenha seu logotipo pequeno e discreto

A incorporação de suas próprias cores e layouts twill também ajuda na comunicação de personalidade.

> [!TIP]
> Trabalhe com o nosso estilo visual para que seu serviço se sente como parte do teams. *Confira* , por exemplo, [cores de equipes](../../concepts/design/components/color.md)

---

## <a name="tab-layouts"></a>Layouts de guia

[!INCLUDE [Tab layouts](../../includes/design/tab-layouts.html)]

---

## <a name="types-of-tabs"></a>Tipos de guias

[!INCLUDE [Tab types](../../includes/design/tab-types.html)]

---

## <a name="configuration-page-height"></a>Altura da página de configuração

>[!IMPORTANT]
>Em setembro de 2018, a altura da [página de configuração](~/tabs/how-to/create-tab-pages/configuration-page.md) da guia foi aumentada enquanto a largura permanece inalterada. Se o seu aplicativo for projetado para o tamanho mais antigo, a página de configuração da guia terá uma grande quantidade de espaço em branco vertical. Aplicativos de repositório herdados isentos dessa alteração precisarão entrar em contato com a Microsoft após a atualização para acomodar as novas dimensões.

As dimensões da página de configuração de guia:


<img width="450px" title="Tamanhos de guias de configuração" src="~/assets/images/tabs/config-dialog-Contoso2.png" alt="sizes for config tabs" />


### <a name="guidelines-for-tab-configuration-page-format"></a>Diretrizes para o formato de página de configuração de guia

* Baseie a altura mínima da área de conteúdo da página de configuração da guia em elementos gráficos de altura fixa.
* Calcula o espaço vertical disponível (a altura da área de conteúdo na página de configuração) usando o `window.innerHeight` . Isso retorna o tamanho do `<iframe>` no qual a página de configuração reside, o que pode ser alterado em versões futuras. Usando esse valor, seu conteúdo será ajustado automaticamente para futuras alterações.
* Alocar espaço vertical para os elementos de altura variável menos o que é necessário para os elementos de altura fixa.
* Para o estado de *logon* , centralizar o conteúdo verticalmente e horizontalmente.
* Se você quiser uma imagem de plano de fundo, precisará de uma nova imagem, dimensionada para se ajustar à área (preferencial) ou pode manter a mesma imagem e escolher entre:
  * alinhamento no canto superior esquerdo.
  * dimensionar a imagem para ajustá-la.

Quando dimensionado corretamente, sua página de configuração de guia deve ser semelhante a esta:

<img width="450px" title="Nova guia de configuração" src="~/assets/images/tabs/config-dialog-Contoso.png" alt="new config tab"/>

## <a name="best-practices"></a>Práticas recomendadas

### <a name="always-include-a-default-state"></a>Sempre incluir um estado padrão

Inclua um estado padrão para facilitar a configuração de guias, mesmo que sua guia seja configurável.

### <a name="deep-linking"></a>Vinculação profunda

Sempre que possível, cartões e bots devem se vincular detalhadamente a dados mais ricos em uma guia hospedada. Por exemplo, um cartão pode exibir um resumo dos dados de bug, mas clicar nele pode mostrar o erro inteiro em uma guia.

### <a name="naming"></a>Nomenclatura

Em muitos casos, o nome do seu aplicativo criará um nome de guia ótimo. Mas, também considere nomear suas guias de acordo com a funcionalidade que elas fornecem.

### <a name="multi-window"></a>Várias janelas

As guias de canal que têm recursos de edição complexos devem abrir o modo de exibição editor em várias janelas, em vez de uma guia.

### <a name="no-horizontal-scrolling"></a>Sem rolagem horizontal

A guia não deve ter rolagem horizontal.

### <a name="easy-navigation"></a>Navegação fácil

A navegação dentro de um aplicativo de guia deve ser fácil de seguir, ou seja, as páginas têm o seguinte, onde necessário/aplicável:
* Botões voltar
* Cabeçalhos de página
* Estrutural
* Menus de reficação

### <a name="undo-last-action"></a>Desfazer última ação

O usuário deve ser capaz de desfazer a última ação no aplicativo.

### <a name="share-content"></a>Compartilhar conteúdo

Os aplicativos pessoais devem permitir que os usuários compartilhem conteúdo de uma experiência de aplicativo pessoal com outros membros da equipe. A guia canal deve fornecer navegação que complementa a navegação principal do Teams, em vez de entrar em conflito com ela (como barras de navegação do trilho esquerdo).

### <a name="single-view"></a>Modo de exibição único

Os aplicativos pessoais devem apresentar conteúdo de instâncias com escopo de chat de grupo ou de equipe desse aplicativo em um modo de exibição único, por exemplo, um usuário do Trello deve ser capaz de ver todas as instâncias das placas do Trello que participam em um nível de equipe em seu aplicativo pessoal.

### <a name="no-app-bar"></a>Nenhuma barra de aplicativos

As guias não devem fornecer uma barra de aplicativos com ícones no trilho esquerdo que estejam em conflito com a navegação do teams principal.

### <a name="navigation"></a>Navegação

Guias não devem ter mais de três níveis de navegação no aplicativo.

### <a name="l2l3-view"></a>Modo de exibição L2/L3

As páginas secundárias e terciários em uma guia devem ser abertas em um modo de exibição L2/L3 na área de guia principal que é navegada por navegação estrutural.

### <a name="no-link-to-external-browser"></a>Nenhum link para o navegador externo

Os destinos de link em guias não devem ser vinculados a um navegador externo, mas devem ser vinculados a elementos div contidos no Teams, por exemplo, dentro de módulos de tarefas, guias, etc.

## <a name="notifications-for-tabs"></a>Notificações para guias

Há dois modos de notificação para alterações de conteúdo de guia:

> [!div class="checklist"]
>
> * **Use a API do aplicativo para notificar os usuários sobre as alterações**. Esta mensagem aparecerá no feed de atividades do usuário e no link profundo para a guia. *consulte*  [criar links de profundas para conteúdo e recursos no Microsoft Teams](../../concepts/build-and-test/deep-links.md?view=msteams-client-js-latest&preserve-view=true )

> * **Use um bot**. Esse método é preferível, especialmente se o thread de guia for direcionado. O resultado será que a conversa encadeada da guia será movida para o modo de exibição como ativo recentemente. Esse método também permite uma certa sofisticação na forma como a notificação é enviada.

Enviar uma mensagem para um thread de guia aumenta a conscientização da atividade para todos os usuários sem notificar explicitamente todos. Isso é conscientização sem ruído. Além disso, quando você `@mention`  especifica os usuários, a mesma notificação será colocada em seus feeds, vinculando-os diretamente ao encadeamento de tabulação.

### <a name="tab-design-best-practices"></a>Práticas recomendadas de design da guia

* As guias pessoais/estáticas devem permitir que os usuários compartilhem conteúdo de uma experiência de aplicativo pessoal com outros membros da equipe.
* Guias pessoais/estáticas podem apresentar conteúdo de instâncias com escopo de chat de grupo ou equipe do aplicativo em um único modo de exibição.
* Os destinos de link em guias não devem ser vinculados a um navegador externo, mas devem ser vinculados a elementos div contidos no Teams (exemplo-interno, módulos de tarefas, guias, etc.).
* As guias devem ser responsivas aos temas da equipe. Quando o tema do teams é alterado, o tema dentro do aplicativo também deve ser alterado para refletir esse tema.
* As guias devem usar componentes com estilo de equipe onde for possível. Isso significa adotar fontes do Teams, digitar rampas, paletas de cores, sistema de grade, movimento, Tom de voz, etc.
* As guias devem usar os comportamentos de interação do teams para navegação na página, posição e uso de caixas de diálogo, hierarquias de informações, etc.
* As guias devem usar o menu e/ou Breadcrumbs da equipe padrão para navegação no aplicativo. As guias não devem fornecer uma barra de aplicativos com ícones no trilho esquerdo que estejam em conflito com a navegação do teams principal.
* Guias não devem ter mais do que três níveis de navegação no aplicativo.
* As páginas secundárias e terciários em uma guia devem ser abertas em um modo de exibição L2/L3 na área de guia principal que é navegada por navegação estrutural.
* Guias que têm recursos de edição complexos dentro do aplicativo devem abrir o modo de exibição editor em várias janelas, em vez de uma guia (para área de trabalho e Web).
