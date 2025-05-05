---
title: Migração do servidor de rastreamento para o encaminhamento do lado do servidor no nível do conjunto de relatórios
description: Saiba como ativar o encaminhamento pelo lado do servidor de dados do Adobe Analytics para o Audience Manager em um nível de conjunto de relatórios, em vez de em um nível de servidor de rastreamento.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# Migração do servidor de rastreamento para o encaminhamento do lado do servidor no nível do conjunto de relatórios {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Este artigo e vídeo mostrarão como habilitar o encaminhamento pelo lado do servidor de Dados do [!DNL Analytics] para o Audience Manager no nível [!UICONTROL report suite] em vez de no nível [!UICONTROL tracking server].

## Introdução {#introduction}

Se você tiver o Adobe Audience Manager E o Adobe Analytics, poderá implementar o encaminhamento pelo lado do servidor dos dados do [!DNL Analytics] para o Audience Manager. Isso significa que, em vez de sua página enviar duas ocorrências (uma para [!DNL Analytics] e uma para Audience Manager), ela pode enviar uma ocorrência para [!DNL Analytics], e o [!DNL Analytics] encaminhará esses dados para Audience Manager.

Se você já tiver esse recurso ativo e em execução, e se ele tiver sido habilitado/implementado antes de outubro de 2017, o encaminhamento pelo lado do servidor poderá se basear no seu [!UICONTROL Tracking Server], que deve ser habilitado pelo Atendimento ao cliente do Adobe ou pela Adobe Consulting. A partir de outubro de 2017, agora você pode configurar o encaminhamento pelo lado do servidor sozinho e fazer isso em nível de conjunto de relatórios (encaminhamento por conjunto de relatórios). Há benefícios significativos para isso, que são discutidos abaixo.

## [!UICONTROL Tracking server] encaminhamento {#tracking-server-forwarding}

Seu [!UICONTROL tracking server] é o local para onde você está enviando seus dados do [!DNL Analytics] e também o domínio no qual a solicitação de imagem e o cookie são gravados. Ele deve ser definido no DTM ou [!DNL Experience Platform Launch], ou no arquivo [!DNL AppMeasurement.js], e normalmente terá esta aparência, com seu site ou nome comercial substituindo &quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Se o encaminhamento pelo lado do servidor estiver configurado para ser encaminhado no nível [!UICONTROL tracking server], qualquer ocorrência que estiver sendo enviada para este [!UICONTROL tracking server] (SE o Serviço de ID de Experience Cloud também estiver habilitado) será encaminhada para o Audience Manager. Isso tinha que ser ativado pelo Atendimento ao cliente do Adobe ou pela Adobe Consulting. Eles também são os que podem desabilitá-lo, DEPOIS que você alternar para o encaminhamento de [!UICONTROL report suite], conforme descrito abaixo.

Se você não tiver certeza se o [!DNL tracking server forwarding] está habilitado para você, entre em contato com o Atendimento ao Cliente do Adobe ou com a Adobe Consulting e eles poderão avisá-lo.

## Encaminhamento pelo lado do servidor de nível [!UICONTROL Report-suite] {#report-suite-level-server-side-forwarding}

Um dos maiores benefícios de mudar do encaminhamento [!UICONTROL tracking server] para o encaminhamento [!UICONTROL report suite] é que agora você poderá usar &quot;Audience Analytics&quot;, que é a capacidade de encaminhar Audience Manager [!UICONTROL segments] de volta para o Adobe Analytics para análise detalhada de segmentos. Este excelente recurso NÃO terá suporte se você ainda estiver no encaminhamento do [!UICONTROL tracking server] e não no encaminhamento do [!UICONTROL report suite]. Consulte mais informações sobre o Audience Analytics na [documentação](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=pt-BR).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Dica importante {#additional-resources}

Conforme mencionado no vídeo acima, depois que todos os [!UICONTROL report suites] estiverem definidos para encaminhar para o Audience Manager, você deverá entrar em contato com o Atendimento ao cliente do Adobe ou com a Adobe Consulting e desativar o encaminhamento do [!UICONTROL tracking server]. Não é uma emergência para você fazer isso, porque ter [!UICONTROL tracking server] encaminhamento e [!UICONTROL report suite] encaminhamento não resulta em ocorrências duplicadas. No entanto, é prática recomendada ativar apenas o encaminhamento [!UICONTROL report suite].

Se você deixar o encaminhamento do [!UICONTROL tracking server] ativado, ele não apenas encaminhará os dados do [!UICONTROL report suites] que você não deseja encaminhar, mas no futuro, depois que você (e todos na sua empresa) esquecerem que o encaminhamento do [!UICONTROL tracking server] está ativado, você poderá pensar que os dados não estão sendo encaminhados para um [!UICONTROL report suite] específico. Isso ocorre porque ele não está ativado no nível do conjunto de relatórios, mas os dados ainda estão sendo encaminhados devido ao [!UICONTROL tracking server]. Em seguida, você perderá tempo e dinheiro descobrindo por que ele está encaminhando e também pagando por chamadas de servidor AAM que você não esperava. Portanto, é recomendável desabilitar o encaminhamento de [!UICONTROL tracking server] assim que tiver todos os [!UICONTROL report suites] definidos para encaminhamento que façam sentido para as suas necessidades comerciais.
