---
title: Usar as práticas recomendadas em páginas do SPA ao enviar dados para o AAM
description: Conheça as práticas recomendadas para enviar dados de aplicativos de página única (SPA) para o Adobe Audience Manager (AAM). Este artigo se concentra no uso de tags do Experience Platform, o método de implementação recomendado.
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: d4874d9f6d7a36bb81ac183eb8b853d893822ae0
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---

# Usar as práticas recomendadas em páginas do SPA ao enviar dados para o AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

Este documento descreve várias práticas recomendadas para enviar dados de aplicativos de página única (SPA) para o Adobe Audience Manager (AAM). Este artigo se concentra em usar o [!UICONTROL Experience Platform tags], o método de implementação recomendado.

## Notas iniciais

* Os itens abaixo assumirão que você está usando tags da Platform para implementar no seu site. As considerações ainda existem se você não estiver usando tags da Platform, mas seria necessário adaptá-las ao seu método de implementação.
* Todos os SPAs são diferentes, portanto, talvez seja necessário ajustar alguns dos itens a seguir para atender melhor aos seus requisitos, mas a Adobe deseja compartilhar algumas práticas recomendadas que você precisa considerar ao enviar dados de páginas de SPA para o Audience Manager.

## Diagrama simples do trabalho com SPAs e AAM em tags do Experience Platform (antigo Launch){#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![spa para aam nas tags](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>Como dito, este é um diagrama simplificado de como as páginas do SPA são tratadas em uma implementação do Adobe Audience Manager (sem o Adobe Analytics) usando tags da Platform. Como você pode ver, é relativamente simples, com a grande decisão sendo como você comunicará uma alteração de exibição (ou uma ação) às tags da Platform.

## Acionamento de tags na página SPA {#triggering-launch-from-the-spa-page}

Dois dos métodos mais comuns para acionar uma regra nas tags da Platform (e, portanto, enviar dados para a Audience Manager) são:

* Definindo eventos personalizados do JavaScript (veja o exemplo [AQUI](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) com o Adobe Analytics)
* Usando um [!UICONTROL Direct Call Rule]

Neste exemplo do Audience Manager, você usa um [!UICONTROL Direct Call rule] nas tags da Platform para acionar a ocorrência que entra no Audience Manager. Como você verá nas próximas seções, isso se torna útil ao configurar o [!UICONTROL Data Layer] como um novo valor, para que ele possa ser selecionado pelo [!UICONTROL Data Element] nas tags da Platform.

## Página de demonstração {#demo-page}

Esta é uma pequena página que demonstra como alterar um valor na camada de dados e enviá-lo para o Audience Manager, como pode ser feito em uma página de SPA. Essa funcionalidade pode ser modelada para alterações mais elaboradas necessárias. Você pode encontrar esta página de demonstração [AQUI](https://aam.enablementadobe.com/SPA-Launch.html).

## Configuração da camada de dados {#setting-the-data-layer}

Como mencionado, quando o novo conteúdo é carregado na página ou quando alguém executa uma ação no site, a camada de dados precisa ser definida dinamicamente no cabeçalho da página ANTES que as tags da Platform sejam chamadas e executem a [!UICONTROL rules], para que as tags da Platform possam coletar os novos valores da camada de dados e enviá-los para a Audience Manager.

Se você for para o site de demonstração listado acima e observar a fonte da página, verá:

* A camada de dados está no cabeçalho da página, antes da chamada para as tags da Platform
* A JavaScript no link de SPA simulado altera a [!UICONTROL Data Layer] e, em seguida, chama as marcas da Platform (a chamada `_satellite.track()`). Se você estava usando eventos personalizados do JavaScript em vez deste [!UICONTROL Direct Call Rule], a lição é a mesma. Primeiro, altere o [!DNL data layer] e, em seguida, chame as tags da Platform.

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## Recursos adicionais {#additional-resources}

* [Discussão sobre SPA nos fóruns do Adobe](https://forums.adobe.com/thread/2451022)
* [Sites de arquitetura de referência para mostrar como implementar SPA em tags da Platform](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Usando práticas recomendadas ao rastrear SPA no Adobe Analytics](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [Site de demonstração usado para este artigo](https://aam.enablementadobe.com/SPA-Launch.html)
