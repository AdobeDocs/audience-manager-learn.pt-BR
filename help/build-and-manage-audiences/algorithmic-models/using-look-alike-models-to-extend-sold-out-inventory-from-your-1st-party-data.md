---
title: Usar modelos semelhantes para estender o inventário esgotado de dados primários
description: Neste tutorial, percorremos as etapas necessárias para configurar e usar modelos semelhantes, para que você possa criar novos públicos semelhantes e vendê-los como uma extensão para seu segmento de conversão.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Usar modelos semelhantes para estender o inventário esgotado de dados primários {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

Neste tutorial, abordaremos as etapas necessárias para configurar e usar o [!UICONTROL Models] semelhante, para que você possa criar novos públicos semelhantes e vendê-los como uma extensão para seu segmento de conversão.

## Detalhes do caso de uso {#use-case-details}

Você é um editor de conteúdo. Se você já tiver esgotado o estoque de conversores em seu site, você pode achar que a oportunidade termina lá. Insira o [!UICONTROL Models] semelhante à AAM. Ao usar esse recurso, você pode estender ainda mais o inventário esgotado e também vender públicos de pessoas que talvez ainda não tenham se convertido, mas que parecem/agem como pessoas que se converteram. Esse segmento de público-alvo normalmente venderia por menos do que os conversores reais, mas, no entanto, permite que você adicione ao seu resultado final fornecendo uma opção de público-alvo adicional para Anunciantes que desejam colocar anúncios em seu site. O benefício adicional desse caso de uso é que não custa nada executar esse modelo em seus dados primários.

As etapas deste tutorial serão as seguintes:

1. Identificar/criar uma característica ou segmento de usuário (conversão) ideal
1. Criar um modelo usando esta característica/segmento de conversão como o item base
1. Escolha [!UICONTROL First party] fonte(s) de dados no modelo e execute o modelo
1. Criar um [!UICONTROL Algorithmic Trait] a partir dos resultados do modelo e adicionar a característica a um segmento
1. Ofereça o segmento aos anunciantes interessados para estender as vendas do segmento de conversão

## Identificar ou criar uma característica ou segmento de usuário (conversão) ideal {#identify-create-an-ideal-user-conversion-trait-or-segment}

O que você está tentando fazer com que as pessoas façam no seu site? Qual é o seu evento de conversão? É claro que há muitas respostas diferentes para essa pergunta, dependendo do tipo/vertical do site e das metas organizacionais. Em qualquer caso, é comum no AAM criar uma característica para visitantes que atenderam a esses critérios.

Nesse caso de uso, isso já foi considerado, porque você esgotou o inventário para pessoas que são conversores. No entanto, para o propósito deste tutorial, é bom discuti-lo como referência para o restante do caso de uso.

Além disso, ao usar eventos para criar características, há uma grande falha que você precisa ter em mente para não coletar mais usuários do que deveria na característica. Assista ao vídeo a seguir para uma grande revelação. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**OBSERVAÇÃO:** no vídeo acima, o exemplo que mostro pressupõe que você tenha o Adobe Analytics. Obviamente, isso pode não ser o caso. Se você tiver o Google Analytics (GA), temos um módulo que pode ser usado para enviar dados para o AAM (consulte a [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=pt-BR)), e se sua atividade de conversão no site for enviada para o AAM pelo GA, você poderá criar sua característica de conversão a partir disso. Se você tiver uma solução de análise diferente (ou nenhuma solução de análise), ainda será possível enviar dados para a AAM por meio de nosso código DIL e da função `submit`, etc. (consulte a [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=pt-BR)). Em seguida, crie novamente a característica de conversão com base nos dados enviados quando a atividade de conversão for executada no site.

## Criar um modelo semelhante a partir de dados primários {#creating-a-look-alike-model-from-first-party-data}

Nesta etapa, vamos criar um Modelo Semelhante [!UICONTROL First Party]. Isso significa que não apenas usaremos uma característica/segmento de conversão primária para nossa característica/segmento base (isso seria normal para a maioria dos modelos de qualquer maneira), mas também somente observaremos o pool de dados primários para mais pessoas que se parecem com os conversores. Não observaremos dados secundários ou de terceiros.

Nesse caso de uso, isso é importante, porque estamos tentando criar um segmento de usuários em nosso site que se parecem com conversores, mas ainda não foram convertidos, para que possamos vender esse segmento semelhante para anunciantes interessados.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Criar uma característica algorítmica {#creating-an-algorithmic-trait}

Em seguida, precisaremos criar um [!UICONTROL Algorithmic Trait], para que os resultados do modelo possam ser usados. Sem criar uma característica, o modelo é inútil. Portanto, depois que o modelo for executado, certifique-se de ir para a caixa de diálogo de características e criar um [!UICONTROL Algorithmic Trait]. O vídeo a seguir aborda esse assunto e mostra algumas dicas.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Oferecer o [!UICONTROL Algorithmic Segment] aos anunciantes {#offering-the-algorithmic-segment-to-advertisers}

Depois de criar um [!UICONTROL Algorithmic Trait], você pode criar um novo segmento para colocá-lo em, de modo que possa ativar os dados (não é possível ativar uma característica, mas criar um novo segmento com uma única característica com o [!UICONTROL Algorithmic Trait] nele, de modo que possa ativar (usar) o segmento.

Depois de criar um segmento de visitantes primários com pontuação alta no modelo semelhante (ou seja, que se parecem com conversores, mas ainda não converteram), você pode oferecer esse segmento a anunciantes no site, mesmo depois de esgotar todo o inventário de conversores reais no site. Esta é uma ótima maneira de estender este público-alvo e continuar a ver a receita adicional usando o [!UICONTROL Models] semelhante no Audience Manager.
