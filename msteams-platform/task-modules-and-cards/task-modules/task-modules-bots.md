---
title: Usando módulos de tarefa nos bots do Microsoft Teams
description: Como usar módulos de tarefas com bots do Microsoft Teams, incluindo cartões de estrutura de bot, cartões adaptáveis e links de fundo.
keywords: robôs da equipe de módulos de tarefa
ms.openlocfilehash: 32fb6a4aa0a8bf2297a4f60331dc5c6c6aceb4e2
ms.sourcegitcommit: 214eccbadb7f3a67236b79a041ef487b7bf6dfbd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2020
ms.locfileid: "44800992"
---
# <a name="using-task-modules-from-microsoft-teams-bots"></a>Usando módulos de tarefa dos bots do Microsoft Teams

Os módulos de tarefa podem ser invocados de bots do Microsoft Teams usando botões em cartões adaptáveis e cartões de estrutura de bot (herói, thumbnail e Office 365 Connector). Os módulos de tarefas costumam ser uma melhor experiência do usuário do que várias etapas de conversa em que você como desenvolvedor precisa manter o controle do estado do bot e permitir que o usuário interrompa/cancele a sequência.

Há duas maneiras de chamar módulos de tarefa:

* **Um novo tipo de mensagem de chamada `task/fetch` .** Usando a `invoke` [ação cartão](~/task-modules-and-cards/cards/cards-actions.md#invoke) para cartões de estrutura de bot ou a `Action.Submit` [ação de cartão](~/task-modules-and-cards/cards/cards-actions.md#adaptive-cards-actions) para cartões adaptáveis, com `task/fetch` o módulo de tarefa (uma URL ou um cartão adaptável) é buscada dinamicamente no bot.
* **URLs de link profundo.** Usando a [sintaxe de link profundo para módulos de tarefa](~/task-modules-and-cards/what-are-task-modules.md#task-module-deep-link-syntax), você pode usar a `openUrl` [ação cartão](~/task-modules-and-cards/cards/cards-actions.md#openurl) para cartões de estrutura de bot ou a `Action.OpenUrl` [ação de cartão](~/task-modules-and-cards/cards/cards-actions.md#adaptive-cards-actions) para cartões adaptáveis, respectivamente. Com URLs de link profundo, a URL do módulo de tarefa ou corpo de cartão adaptável é obviamente conhecido com antecedência, evitando uma viagem de ida e volta ao servidor em relação a `task/fetch` .

>[!IMPORTANT]
>Para garantir comunicações seguras, cada `url` `fallbackUrl` uma e deve implementar o protocolo de criptografia HTTPS.

## <a name="invoking-a-task-module-via-taskfetch"></a>Invocar um módulo de tarefa por meio de tarefa/busca

Quando o `value` objeto da `invoke` ação do cartão ou `Action.Submit` é inicializado da forma adequada (explicado em mais detalhes abaixo), quando um usuário pressiona o botão, uma `invoke` mensagem é enviada ao bot. Na resposta HTTP para a `invoke` mensagem, há um [objeto TaskInfo](~/task-modules-and-cards/what-are-task-modules.md#the-taskinfo-object) incorporado em um objeto envoltório, que o Teams usa para exibir o módulo de tarefa.

![solicitação/resposta de tarefa/busca](~/assets/images/task-module/task-module-invoke-request-response.png)

Vamos examinar cada etapa mais detalhadamente:

1. Este exemplo mostra um cartão de herói da estrutura de bot com uma `invoke` [ação de cartão](~/task-modules-and-cards/cards/cards-actions.md#invoke)de "comprar". O valor da `type` propriedade é `task/fetch` : o restante do `value` objeto pode ser o que você quiser.
2. O bot recebe a `invoke` mensagem http post.
3. O bot cria um objeto Response e o retorna no corpo da resposta POST com um código de resposta HTTP 200. O esquema de respostas é descrito [abaixo na discussão sobre a tarefa/envio](#the-flexibility-of-tasksubmit), mas o importante a ser lembrado agora é que o corpo da resposta http contém um [objeto TaskInfo](~/task-modules-and-cards/what-are-task-modules.md#the-taskinfo-object) incorporado em um objeto envoltório, por exemplo:

    ```json
    {
      "task": {
        "type": "continue",
        "value": {
          "title": "Task module title",
          "height": 500,
          "width": "medium",
          "url": "https://contoso.com/msteams/taskmodules/newcustomer",
          "fallbackUrl": "https://contoso.com/msteams/taskmodules/newcustomer"
        }
      }
    }
    ```

    O `task/fetch` evento e sua resposta para bots é semelhante, conceitualmente, à `microsoftTeams.tasks.startTask()` função no SDK do cliente.
4. O Microsoft Teams exibe o módulo de tarefa.

## <a name="submitting-the-result-of-a-task-module"></a>Enviando o resultado de um módulo de tarefa

Quando o usuário termina com o módulo de tarefa, o envio do resultado para o bot é semelhante [à forma como funciona com guias](~/task-modules-and-cards/task-modules/task-modules-tabs.md#example-submitting-the-result-of-a-task-module), mas há algumas diferenças, portanto, é descrito aqui.

* **HTML/JavaScript ( `TaskInfo.url` )**. Depois de validar o que o usuário inseriu, chame a `microsoftTeams.tasks.submitTask()` função do SDK (mencionada em diante como `submitTask()` para fins de legibilidade). Você pode chamar `submitTask()` sem qualquer parâmetro se quiser que o Microsoft Teams feche o módulo de tarefa, mas a maior parte do tempo desejará passar um objeto ou uma cadeia de caracteres para o seu `submitHandler` . Basta passá-lo como o primeiro parâmetro `result` . O Microsoft Teams invocará `submitHandler` : `err` será `null` e `result` será o objeto/cadeia de caracteres que você deseja `submitTask()` . Se você chamar `submitTask()` com um `result` parâmetro, você **deve** passar um `appId` ou uma matriz de `appId` cadeias de caracteres: isso permite que as equipes validem que o aplicativo enviando o resultado é o mesmo que invocar o módulo de tarefa. O bot receberá uma `task/submit` mensagem `result` , incluindo as descritas [abaixo](#payload-of-taskfetch-and-tasksubmit-messages).
* **Cartão adaptável ( `TaskInfo.card` )**. O corpo do cartão adaptável (conforme preenchido pelo usuário) será enviado ao bot por meio de uma `task/submit` mensagem quando o usuário pressionar qualquer `Action.Submit` botão.

## <a name="the-flexibility-of-tasksubmit"></a>A flexibilidade de tarefa/envio

Na seção anterior, você aprendeu que quando o usuário termina com um módulo de tarefa invocado de um bot, o bot sempre recebe uma `task/submit invoke` mensagem. Como desenvolvedor, você tem várias opções ao *responder* à `task/submit` mensagem:

| Resposta de corpo HTTP                      | Cenário                                |
| --------------------------------------- | --------------------------------------- |
| Nenhum (ignorar a `task/submit` mensagem) | A resposta mais simples é nenhuma resposta. O bot não precisa responder quando o usuário termina com o módulo de tarefa. |
| <pre>{<br/>  "task": {<br/>    "type": "message",<br/>    "value": "Message text"<br/>  }<br/>}</pre> | O Microsoft Teams exibirá o valor de `value` em uma caixa de mensagem pop-up. |
| <pre>{<br/>  "task": {<br/>    "type": "continue",<br/>    "value": &lt;TaskInfo object&gt;<br/>  }<br/>}</pre> | Permite que você "encadear" sequências de cartões adaptáveis juntos em uma experiência de assistente/várias etapas. _Observe que o encadeamento de cartões adaptáveis em uma sequência é um cenário avançado e não está documentado aqui. No entanto, o aplicativo de exemplo Node.js dá suporte a ele e como ele funciona está documentado no [arquivo readme.MD](https://github.com/OfficeDev/microsoft-teams-sample-task-module-nodejs#implementation-notes)._ |

## <a name="payload-of-taskfetch-and-tasksubmit-messages"></a>Carga das mensagens de tarefa/busca e de tarefa/envio

Esta seção define o esquema do que seu bot recebe quando recebe um `task/fetch` objeto de `task/submit` estrutura ou bot `Activity` . O nível superior importante aparece abaixo:

| Propriedade | Descrição                          |
| -------- | ------------------------------------ |
| `type`   | Sempre será`invoke`              |
| `name`   | Um `task/fetch` ou`task/submit` |
| `value`  | A carga definida pelo desenvolvedor. Normalmente, a estrutura do `value` objeto espelha o que foi enviado do teams. Nesse caso, no entanto, é diferente porque queremos oferecer suporte à busca dinâmica ( `task/fetch` ) da estrutura de bot () `value` e ações de cartão adaptável `Action.Submit` ( `data` ), e precisamos de uma maneira de comunicar `context` as equipes ao bot, além do que foi incluído no `value` / `data` .<br/><br/>Fazemos isso combinando os dois em um objeto pai:<br/><br/><pre>{<br/>  "context": {<br/>    "theme": "default" &vert; "dark" &vert; "contrast",<br/>  },<br/>  "data": [value field from Bot Framework card] &vert; [data field from Adaptive Card] <br/>}</pre>  |

## <a name="example-receiving-and-responding-to-taskfetch-and-tasksubmit-invoke-messages---nodejs"></a>Exemplo: recebimento e resposta a mensagens/busca e tarefas/envio de invocação de tarefas-Node.js

> [!NOTE]
> O código de exemplo abaixo foi modificado entre a visualização técnica e a versão final desse recurso: o esquema da `task/fetch` solicitação foi alterado para acompanhar o que foi [documentado na seção anterior](#payload-of-taskfetch-and-tasksubmit-messages). Ou seja, a documentação estava correta, mas a implementação não foi. Consulte os `// for Technical Preview [...]` comentários abaixo para saber o que foi alterado.

```typescript
// Handle requests and responses for a "Custom Form" and an "Adaptive card" task module.
// Assumes request is coming from an Adaptive card Action.Submit button that has a "taskModule" property indicating what to invoke
private async onInvoke(event: builder.IEvent, cb: (err: Error, body: any, status?: number) => void): Promise<void> {
    let invokeType = (event as any).name;
    let invokeValue = (event as any).value;
    if (invokeType === undefined) {
        invokeType = null;
    }
    switch (invokeType) {
        case "task/fetch": {
            if (invokeValue !== undefined && invokeValue.data.taskModule === "customform") { // for Technical Preview, was invokeValue.taskModule
                // Return the specified task module response to the bot
                let fetchTemplate: any = {
                    "task": {
                        "type": "continue",
                        "value": {
                            "title": "Custom Form",
                            "height": 510,
                            "width": 430,
                            "fallbackUrl": "https://contoso.com/teamsapp/customform",
                            "url": "https://contoso.com/teamsapp/customform",
                        }
                    }
                };
                cb(null, fetchTemplate, 200);
            };
            if (invokeValue !== undefined && invokeValue.data.taskModule === "adaptivecard") { // for Technical Preview, was invokeValue.taskModule
                let adaptiveCard = {
                    "type": "AdaptiveCard",
                    "body": [
                        {
                            "type": "TextBlock",
                            "text": "Here is a ninja cat:"
                        },
                        {
                            "type": "Image",
                            "url": "http://adaptivecards.io/content/cats/1.png",
                            "size": "Medium"
                        }
                    ],
                    "version": "1.0"
                };
                // Return the specified task module response to the bot
                let fetchTemplate: any = {
                    "task": {
                        "type": "continue",
                        "value": {
                            "title": "Ninja Cat",
                            "height": "small",
                            "width": "small",
                            "card": {
                                contentType: "application/vnd.microsoft.card.adaptive",
                                content: adaptiveCard,
                            }
                        }
                    }
                };
                cb(null, fetchTemplate, 200);
            };
            break;
        }
        case "task/submit": {
            if (invokeValue.data !== undefined) {
                // It's a valid task module response
                let submitResponse: any = {
                    "task": {
                        "type": "message",
                        "value": "Task complete!",
                    }
                };
                cb(null, fetchTemplates.submitMessageResponse, 200)
            }
        }
    }
}
```

*Consulte também*, [código de exemplo do módulo de tarefa do Microsoft Teams —](https://github.com/OfficeDev/microsoft-teams-sample-task-module-nodejs/blob/master/src/TeamsBot.ts) amostras de NodeJS e [bot Framework](https://github.com/Microsoft/BotBuilder-Samples/blob/master/README.md).

## <a name="example-receiving-and-responding-to-taskfetch-and-tasksubmit-invoke-messages---c"></a>Exemplo: recebendo e respondendo a tarefas/buscar e/enviar mensagens de invocação-C #

Nos bots do C#, `invoke` as mensagens são processadas por um `HttpResponseMessage()` controlador que processa uma `Activity` mensagem. As `task/fetch` solicitações e as `task/submit` respostas são JSON. No C#, não é tão conveniente lidar com o JSON bruto como está em Node.js, portanto, você precisa de classes de wrapper para lidar com a serialização de e para JSON. Ainda não há suporte direto para isso no SDK do Microsoft Teams [C#](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams) , mas você pode ver um exemplo de como essas classes de wrapper simples seriam parecidas no [aplicativo de exemplo C#](https://github.com/OfficeDev/microsoft-teams-sample-task-module-csharp/blob/master/Microsoft.Teams.Samples.TaskModule.Web/Models/TaskModel.cs).

Veja a seguir um exemplo de código em C# para tratamento `task/fetch` e `task/submit` mensagens usando essas classes de wrapper ( `TaskInfo` , `TaskEnvelope` ) extratos da [amostra](https://github.com/OfficeDev/microsoft-teams-sample-task-module-csharp/blob/master/Microsoft.Teams.Samples.TaskModule.Web/Controllers/MessagesController.cs):

```csharp
private HttpResponseMessage HandleInvokeMessages(Activity activity)
{
    var activityValue = activity.Value.ToString();
    if (activity.Name == "task/fetch")
    {
        var action = Newtonsoft.Json.JsonConvert.DeserializeObject<Models.BotFrameworkCardValue<string>>(activityValue);

        Models.TaskInfo taskInfo = GetTaskInfo(action.Data);
        Models.TaskEnvelope taskEnvelope = new Models.TaskEnvelope
        {
            Task = new Models.Task()
            {
                Type = Models.TaskType.Continue,
                TaskInfo = taskInfo
            }
        };
        return Request.CreateResponse(HttpStatusCode.OK, taskEnvelope);
    }
    else if (activity.Name == "task/submit")
    {
        ConnectorClient connector = new ConnectorClient(new Uri(activity.ServiceUrl));
        Activity reply = activity.CreateReply("Received = " + activity.Value.ToString());
        connector.Conversations.ReplyToActivity(reply);
    }
    return new HttpResponseMessage(HttpStatusCode.Accepted);
}

// Helper function for building the TaskInfo object based on the incoming request
private static Models.TaskInfo GetTaskInfo(string actionInfo)
{
    Models.TaskInfo taskInfo = new Models.TaskInfo();
    switch (actionInfo)
    {
        case TaskModuleIds.YouTube:
            taskInfo.Url = taskInfo.FallbackUrl = ApplicationSettings.BaseUrl + "/" + TaskModuleIds.YouTube;
            SetTaskInfo(taskInfo, TaskModuleUIConstants.YouTube);
            break;
        case TaskModuleIds.PowerApp:
            taskInfo.Url = taskInfo.FallbackUrl = ApplicationSettings.BaseUrl + "/" + TaskModuleIds.PowerApp;
            SetTaskInfo(taskInfo, TaskModuleUIConstants.PowerApp);
            break;
        case TaskModuleIds.CustomForm:
            taskInfo.Url = taskInfo.FallbackUrl = ApplicationSettings.BaseUrl + "/" + TaskModuleIds.CustomForm;
            SetTaskInfo(taskInfo, TaskModuleUIConstants.CustomForm);
            break;
        case TaskModuleIds.AdaptiveCard:
            taskInfo.Card = AdaptiveCardHelper.GetAdaptiveCard();
            SetTaskInfo(taskInfo, TaskModuleUIConstants.AdaptiveCard);
            break;
        default:
            break;
    }
    return taskInfo;
}
```

Não mostrado no exemplo acima é a `SetTaskInfo()` função, que define as `height` Propriedades, `width` e `title` do `TaskInfo` objeto para cada caso. Este é o [código-fonte do SetTaskInfo ()](https://github.com/OfficeDev/microsoft-teams-sample-task-module-csharp/blob/master/Microsoft.Teams.Samples.TaskModule.Web/Controllers/MessagesController.cs).

### <a name="bot-framework-card-actions-vs-adaptive-card-actionsubmit-actions"></a>Ações de cartão de estrutura de bot versus ação de cartão adaptável. enviar ações

O esquema para as ações do cartão de estrutura de bot é um pouco diferente das ações de cartão adaptável `Action.Submit` . Como resultado, a maneira de invocar módulos de tarefa é um pouco diferente: o `data` objeto no `Action.Submit` contém um `msteams` objeto para que ele não interfira com outras propriedades no cartão. A tabela a seguir mostra um exemplo de cada:

| Ação do cartão de estrutura de bot                              | Ação de cartão adaptável. Submit                     |
| ------------------------------------------------------ | ------------------------------------------------------ |
| <pre>{<br/>  "type": "invoke",<br/>  "title": "Buy",<br/>  "value": {<br/>    "type": "task/fetch",<br/>    &lt;...&gt;<br/>  }<br/>}</pre> | <pre>{<br/>  "type": "Action.Submit",<br/>  "id": "btnBuy",<br/>  "title": "Buy",<br/>  "data": {<br/>    &lt;...&gt;,<br/>    "msteams": {<br/>      "type": "task/fetch"<br/>    }<br/>  }<br/>}</pre>  |
