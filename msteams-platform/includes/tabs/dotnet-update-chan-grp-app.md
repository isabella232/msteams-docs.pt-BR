### <a name="_layoutcshtml"></a>_Layout. cshtml

Para que sua guia seja exibida no Teams, você deve incluir o **SDK do cliente JavaScript do Microsoft Teams** e incluir uma chamada para depois que `microsoftTeams.initialize()` a página for carregada. Esta é a maneira como sua guia e o cliente do teams se comunicam:

- Navegue até a pasta **compartilhada** , abra **_Layout. cshtml**e adicione o seguinte à `<head>` marca:

```html
<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js"></script>
<script src="https://statics.teams.cdn.office.net/sdk/v1.6.0/js/MicrosoftTeams.min.js"></script>
```

>[!IMPORTANT]
>Não copie/cole as `<script src="...">` URLs desta página, pois elas podem não representar a versão mais recente. Para obter a versão mais recente do SDK, sempre vá para: [API JavaScript do Microsoft Teams](https://www.npmjs.com/package/@microsoft/teams-js.com).

### <a name="tabcshtml"></a>Tab. cshtml

Abra **Tab. cshtml** e atualize o incorporado `<script>` da seguinte maneira:

- Na parte superior do script, ligue `microsoftTeams.initialize()`para.

- Atualize os `websiteUrl` valores `contentUrl` e em cada função com a URL https ngrok para sua guia.

Agora, o código deve se parecer com o seguinte com o **y8rCgT2b** substituído pela URL do ngrok:

```javascript
    microsoftTeams.initialize();

    let saveGray = () => {
        microsoftTeams.settings.registerOnSaveHandler(function (saveEvent) {
            microsoftTeams.settings.setSettings({
                websiteUrl: `https://y8rCgT2b.ngrok.io`,
                contentUrl: `https://y8rCgT2b.ngrok.io/gray/`,
                entityId: "grayIconTab",
                suggestedDisplayName: "MyNewTab"
            });
            saveEvent.notifySuccess();
        });
    }

    let saveRed = () => {
        microsoftTeams.settings.registerOnSaveHandler(function (saveEvent) {
            microsoftTeams.settings.setSettings({
                websiteUrl: `https://y8rCgT2b.ngrok.io`,
                contentUrl: `https://y8rCgT2b.ngrok.io/red/`,
                entityId: "redIconTab",
                suggestedDisplayName: "MyNewTab"
            });
            saveEvent.notifySuccess();
        });
    }
```

Certifique-se de salvar a **guia atualizada. cshtml**.

## <a name="build-and-run-your-application"></a>Criar e executar o aplicativo

- No Visual Studio, pressione **F5**ou escolha **Iniciar Depuração** no menu **depurar** . Verifique se o **ngrok** está em execução e funcionando corretamente abrindo o navegador e acessando a página de conteúdo por meio da URL https do ngrok que foi fornecida na janela de prompt de comando.

>[!TIP]
>Você precisa ter o aplicativo no Visual Studio e o ngrok em execução para concluir este QuickStart. Se você precisar parar a execução do aplicativo no Visual Studio para trabalhar nele, **Mantenha o ngrok em execução**. Ele continuará a escutar e retomará o roteamento da solicitação do aplicativo quando ele for reiniciado no Visual Studio. Se for necessário reiniciar o serviço ngrok, ele retornará uma nova URL e você terá que atualizar seu aplicativo com a nova URL.

## <a name="upload-your-tab-to-teams-with-app-studio"></a>Carregar sua guia para o Teams com o app Studio

>[!Note]
> Usamos o app Studio para editar o arquivo **manifest. JSON** e carregar o pacote concluído para o Microsoft Teams. Você também pode editar manualmente o arquivo **manifest. JSON** , se preferir. Se você fizer isso, certifique-se de compilar a solução novamente para criar o arquivo **Tab. zip** para carregar.

- Abra o cliente do Microsoft Teams. Se você usar a [versão baseada na Web](https://teams.microsoft.com) , poderá inspecionar seu código de front-end usando as [ferramentas de desenvolvedor](~/tabs/how-to/developer-tools.md)do navegador.

- Abra o app Studio e selecione a guia **Editor do manifesto** .

- Selecione o bloco **importar um aplicativo existente** no editor de manifesto para começar a atualizar o pacote de aplicativos para sua guia. O código-fonte vem com seu próprio manifesto parcialmente concluído. O nome do seu pacote de aplicativos é **Tab. zip**. Ele deve ser encontrado aqui:

```bash
/bin/Debug/netcoreapp2.2/tab.zip
```

- Carregar **Tab. zip** para o app Studio.

### <a name="update-your-app-package-with-manifest-editor"></a>Atualizar seu pacote de aplicativos com o editor de manifesto

Depois de carregar seu pacote de aplicativos no app Studio, você precisará concluir a configuração.

- Selecione o bloco para a guia recentemente importada no painel direito da página de boas-vindas do editor de manifesto.

Há uma lista de etapas no lado esquerdo do editor de manifestos e, à direita, uma lista de propriedades que precisam ter valores para cada uma dessas etapas. Muitas das informações foram fornecidas pelo *manifesto. JSON* , mas há alguns campos que você precisará atualizar:

#### <a name="details-app-details"></a>Detalhes: detalhes do aplicativo

- Em *identificação* selecione ***gerar*** para substituir a ID do espaço reservado pelo GUID necessário da guia.

- Em *informações do desenvolvedor* , atualize o campo **URL do site** com sua URL https do *ngrok* .

- Em *URLs de aplicativo* , atualize a **declaração de privacidade** e **termos de uso de campos de** URL com sua URL https do *ngrok* . Lembre-se de incluir os caminhos */Privacy* e */tou* no final das URLs.

#### <a name="capabilities-tabs"></a>Recursos: guias

Na seção *guias* :

- Na *guia equipe* selecione **Adicionar**.

- Na janela pop-up da guia equipe atualizar a *URL* de configuração `https://<yourngrokurl>/tab`para.

- Por fim, certifique-se de que a *configuração pode atualizar? Equipe*e caixas de *chat de grupo* são verificados e selecione **salvar**.

#### <a name="finish-domains-and-permissions"></a>Concluir: domínios e permissões

Na seção *domínios e permissões* , os *domínios do campo Tabs* devem conter a URL do NGROK sem o prefixo HTTPS- `<yourngrokurl>.ngrok.io/`.

#### <a name="test-and-distribute-test-and-distribute"></a>Testar e distribuir: testar e distribuir

>[!IMPORTANT]
>No campo **Descrição** à direita, você verá o seguinte aviso:
>
>&#9888; "**a matriz ' validDomains ' não pode conter um site de tunelamento...**"
>
>Este aviso pode ser ignorado durante o teste da guia.

Na seção *testar e distribuir* :

- Selecionar **Instalar**.

- Na janela pop-up *Adicionar a um campo de equipe ou chat* , insira sua equipe e selecione **instalar**.

- Na próxima janela pop-up, escolha o canal de equipe onde você deseja exibir a guia e selecione **Configurar**.

- Na janela pop-up final, selecione um valor para a página da guia (um ícone vermelho ou cinza) e selecione **salvar**.

Para exibir sua guia, navegue até a equipe na qual você a instalou e selecione-a na barra de guias. A página que você escolheu durante a configuração deve ser exibida.
