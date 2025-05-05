---
title: Aumentar ROAS usando modelos algorítmicos (semelhantes)
description: O verdadeiro poder da modelagem semelhante ao Audience Manager acontece quando você busca expandir seu público-alvo de linha de base em relação a um novo conjunto de usuários de qualidade de fontes de dados de terceiros. Neste tutorial, aprenda as etapas criar um modelo com base nesses dados.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 0%

---

# Aumentar ROAS usando modelos algorítmicos (semelhantes) no Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

O verdadeiro poder do Audience Manager vem quando você busca expandir seu público-alvo de linha de base em relação a um novo conjunto de usuários de fontes de dados de terceiros e de qualidade. [!UICONTROL Modeling] Neste tutorial, aprenda as etapas necessárias para criar um modelo com base nesses dados.

## Habilitar fluxos de dados secundários ou de terceiros no Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Para usar dados secundários e de terceiros em um modelo semelhante, primeiro precisamos habilitar esses dados na interface do Audience Manager. O Adobe tem um grande número de provedores de dados secundários e de terceiros a partir dos quais você pode escolher. Eles estão disponíveis para você em uma interface de autoatendimento no AAM, por meio do Audience Marketplace. Navegue até o Audience Marketplace e navegue pelas possibilidades. O vídeo a seguir mostrará como fazer isso, incluindo como habilitar fluxos gratuitos de &quot;teste antes de comprar&quot;, para que você possa bloquear os dados que serão mais úteis para sua organização antes de confirmar os preços do provedor de dados.

Além disso, para ajudá-lo a pesquisar e decidir qual provedor de dados usar, um ótimo recurso é o [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificar ou criar uma característica ou segmento de usuário (conversão) ideal {#identify-create-an-ideal-user-conversion-trait-or-segment}

O que você está tentando fazer com que as pessoas façam no seu site? Qual é o seu evento de conversão? É claro que há muitas respostas diferentes para essa pergunta, dependendo do tipo/vertical do site e das metas organizacionais. Em qualquer caso, é comum no AAM criar uma característica para visitantes que atenderam a esses critérios.

No vídeo abaixo, mostrarei como criar uma característica de conversão, que você desejará que esteja em vigor ao continuar com este tutorial e criar um modelo semelhante.

Além disso, ao usar eventos do Adobe Analytics para criar características, há uma grande falha que você precisa ter em mente para não coletar mais usuários do que deveria na característica. Assista ao vídeo a seguir para uma grande revelação. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**OBSERVAÇÃO:** no vídeo acima, o exemplo que mostro pressupõe que você tenha o Adobe Analytics. Obviamente, isso pode não ser o caso. Se você tiver o Google Analytics (GA), temos um módulo que pode ser usado para enviar dados para AAM (consulte a [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=pt-BR)), e se sua atividade de conversão no site for enviada para o AAM por GA, você poderá criar sua característica de conversão a partir disso. Se você tiver uma solução de análise diferente (ou nenhuma solução de análise), ainda será possível enviar dados para o AAM por meio de nosso código de DIL e da função `submit`, etc. (consulte a [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=pt-BR)). Em seguida, crie a característica de conversão com base nos dados enviados quando a atividade de conversão for executada no site.

## Criar um modelo semelhante a partir de dados secundários ou de terceiros {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Após concluir as etapas acima, estamos prontos para criar um Modelo algorítmico (semelhante). À medida que estamos configurando o modelo, usaremos a característica de conversão como nossa característica base (visitantes-chave que queremos duplicar) e usaremos o fluxo de dados de terceiros ativado como nosso pool de pessoas do qual podemos extrair.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Uma prática recomendada importante {#an-important-best-practice}

Ao criar o modelo algorítmico no Audience Manager, obviamente queremos que o modelo seja o mais eficaz possível. Como o modelo está considerando todas as características das quais os membros da característica/segmento base fazem parte, não ajuda o modelo se TODAS as pessoas estiverem em uma característica/segmento. Portanto, se você tiver características supergenéricas (como todos que foram para o seu site ou todos que receberam qualquer anúncio de você etc.), verifique se a fonte de dados à qual eles pertencem NÃO está incluída nas fontes de dados no seu modelo. No caso de uso deste artigo, é improvável que você o faça, pois estamos nos concentrando em observar dados de terceiros para nossos novos sósias, mas vale a pena mencionar de qualquer maneira e se aplica a TODOS os seus modelos algorítmicos.

## Criar um [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

Em seguida, precisaremos criar um [!UICONTROL Algorithmic Trait], para que os resultados do modelo possam ser usados. Sem criar uma característica, o modelo é inútil. Assim, depois que o modelo for executado, certifique-se de ir para a caixa de diálogo de características e criar um [!UICONTROL Algorithmic Trait]. O vídeo a seguir aborda esse assunto e mostra algumas dicas.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Criar um segmento a partir dos dados do modelo e enviá-lo para o DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Depois de criar uma [!UICONTROL Algorithmic Trait], você pode criar um novo segmento para colocá-la, de modo que possa ativar os dados (não é possível ativar uma característica, mas criar um novo segmento com uma única característica com a [!UICONTROL Algorithmic Trait] nela, para poder ativar (usar) o segmento).

Depois de criar um segmento com essa característica algorítmica, você terá um público-alvo de clientes em potencial que se parecem com pessoas que já foram convertidas em seu site. Agora é possível mapear esse segmento para qualquer um dos destinos do DSP no Audience Manager. Você poderá direcionar seu marketing para esses semelhantes, que têm mais probabilidade de conversão em seu site do que apenas o público normal, aumentando seu Retorno do investimento em anúncios.
