---
title: Aumente o ROAS usando modelos algorítmicos (semelhantes) no Audience Manager
description: O poder real da modelagem semelhante à Audience Manager é que você busca expandir sua audiência de referência em relação a um conjunto novo de usuários de qualidade de fontes de dados de terceiros e fontes de dados de terceiros. Neste tutorial, aprenda as etapas para criar um modelo a partir desses dados.
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1849
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---


# Aumente o ROAS usando algorítmico (semelhante) [!UICONTROL Models] no Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

O poder real da aparência Audience Manager [!UICONTROL Modeling] vem quando você busca expandir sua audiência de referência em relação a um conjunto novo de usuários de qualidade [!UICONTROL second party] e [!UICONTROL third party][!UICONTROL data sources]. Neste tutorial, aprenda as etapas necessárias para criar um [!UICONTROL model] a partir desses dados.

## Ativar [!UICONTROL Second Party] ou [!UICONTROL Third Party] Fluxos de dados do Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Para usar [!UICONTROL second party] e [!UICONTROL third party] os dados em uma aparência semelhante [!UICONTROL model], primeiro temos que habilitar esses dados na interface do Audience Manager. O Adobe tem um grande número de provedores de dados [!UICONTROL second party] e [!UICONTROL third party] de onde você pode escolher. Eles estão disponíveis para você em uma interface de autoatendimento em AAM, via Audience Marketplace. Navegue até o Audience Marketplace e navegue pelas possibilidades. O vídeo a seguir mostrará como fazer isso, incluindo como ativar fluxos gratuitos de &quot;tentar antes de comprar&quot;, para que você possa se bloquear nos dados que serão mais úteis para a sua organização antes de confirmar o preço do provedor de dados.

Além disso, para ajudá-lo a pesquisar e decidir qual provedor de dados usar, um grande recurso é o [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identifique/crie um usuário ideal (conversão) [!UICONTROL trait] ou [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

O que você está tentando fazer com que as pessoas façam em seu site? Qual é o seu evento de conversão? É claro que há muitas respostas diferentes para essa pergunta, dependendo do tipo/vertical do site e das metas organizacionais. Em todo o caso, é comum em AAM criar um sistema [!UICONTROL trait] para visitantes que tenham cumprido esses critérios.

No vídeo abaixo, mostrarei como criar uma conversão [!UICONTROL trait], que você desejará ter em vigor à medida que continuar neste tutorial e criar uma aparência semelhante [!UICONTROL model].

Além disso, ao usar eventos Adobe Analytics para criar [!UICONTROL traits], há um grande ganho que você precisa ter em mente, para que você não colete mais usuários do que deveria no [!UICONTROL trait]. Assista ao vídeo a seguir para ver a grande revelação. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** No vídeo acima, o exemplo que eu mostro presume que você tem Adobe Analytics. Obviamente, pode não ser esse o caso. Se você tiver Google Analytics (GA), nós teremos um módulo que você pode usar para enviar dados para AAM (consulte a [documentação](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), e se sua atividade de conversão em seu site for enviada para AAM pelo GA, então você poderá criar sua conversão [!UICONTROL trait] a partir disso. Se você tiver uma solução de análise diferente (ou nenhuma solução de análise), ainda poderá enviar dados para AAM por meio do código de DIL e da `submit` função, etc. (consulte a [documentação](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). Em seguida, crie a conversão [!UICONTROL trait] com base nos dados enviados quando a atividade de conversão for executada no site.

## Criar uma aparência semelhante [!UICONTROL Model] a partir de [!UICONTROL Second Party] ou [!UICONTROL Third Party] Dados {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Depois de concluir as etapas acima, estamos prontos para criar um Algoritmo (sósia) [!UICONTROL Model]. Enquanto estamos configurando o [!UICONTROL model], usaremos a conversão [!UICONTROL trait] como nossa base [!UICONTROL trait] (visitantes-chave que queremos duplicado), e usaremos o fluxo de [!UICONTROL third party] dados habilitado como nosso pool de pessoas para extrair.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Uma prática recomendada importante {#an-important-best-practice}

Ao criar o algoritmo [!UICONTROL model] no Audience Manager, obviamente queremos que o [!UICONTROL model] seja tão eficaz quanto possível. Como o [!UICONTROL model] está considerando todos os [!UICONTROL traits] membros da sua base [!UICONTROL trait]/[!UICONTROL segment] que fazem parte, isso não ajuda a [!UICONTROL model] se TODAS as pessoas estiverem em uma [!UICONTROL trait]/[!UICONTROL segment]. Portanto, se você tiver um super genérico [!UICONTROL traits] (como todos que foram ao seu site, ou todos que receberam qualquer anúncio seu, etc.), certifique-se de que o [!UICONTROL data source] que eles pertencem NÃO esteja incluído no [!UICONTROL data sources] seu [!UICONTROL model]. No caso de uso deste artigo, é improvável que você o faça, pois estamos focados em [!UICONTROL third party] dados para nossos novos sósias, mas vale a pena mencionar de qualquer forma, e se aplica a TODOS os algoritmos [!UICONTROL models].

## Criação de um algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Em seguida, teremos de criar um Algoritmo [!UICONTROL Trait], para que os resultados do [!UICONTROL model] projeto possam ser usados. Sem criar um [!UICONTROL trait], o modelo é inútil. Então, depois da [!UICONTROL model] corrida, certifique-se de entrar no [!UICONTROL trait] diálogo e criar um Algorítmico [!UICONTROL Trait]. O vídeo a seguir mostra algumas dicas.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Criar um [!UICONTROL Segment] dos [!UICONTROL Model] dados e enviá-lo para o DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Depois de criar um Algoritmo [!UICONTROL Trait], você pode criar um novo [!UICONTROL segment] para colocá-lo no, para que possa ativar os dados (não é possível ativar um [!UICONTROL trait], mas sim criar um novo[!UICONTROL trait] com o Algoritmo [!UICONTROL segment] nele, para que você possa ativar (usar) o [!UICONTROL Trait] [!UICONTROL segment]).

Depois de criar um [!UICONTROL segment] [!UICONTROL trait]deste algoritmo, você terá uma audiência de clientes potenciais que se parecem com pessoas que já foram convertidas em seu site. Agora você pode mapear isso [!UICONTROL segment] para qualquer DSP [!UICONTROL destinations] em Audience Manager. Você poderá público alvo seu marketing para esses sósias, que têm maior probabilidade de serem convertidos em seu site do que apenas o público normal, aumentando seu Retorno da despesa de publicidade. Boa sorte!