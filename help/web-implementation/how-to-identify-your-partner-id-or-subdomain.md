---
title: Como identificar a ID ou o subdomínio do parceiro
description: Saiba como identificar sua ID de parceiro ou subdomínio ao implementar alguns recursos do Experience Cloud e sobre dois lugares em que você pode obter essa ID na interface do usuário do Audience Manager.
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Como identificar o subdomínio do Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Ao implementar alguns recursos do Experience Cloud, você precisa saber o que é o seu Audience Manager `Subdomain` (também conhecido como `client ID` ou `Partner ID`). Neste vídeo, mostraremos dois lugares onde você pode obter essas informações na interface do usuário do Audience Manager.

## Dando o final... {#giving-away-the-ending}

Caso prefira entrar e encontrá-lo sem assistir a este vídeo curto, você pode encontrar o `Partner Subdomain` em dois lugares na interface do usuário:

1. Se você já criou uma característica [!UICONTROL rule-based], clique em **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] está ao lado da característica na lista de características nessa pasta, e a URL incluirá seu subdomínio na URL.
1. Se você acessar a interface **[!UICONTROL Tools]** > **[!UICONTROL Tags]** e clicar em **[!UICONTROL Get code]** para o seu contêiner, o subdomínio será colocado no final da linha Akamai

Se você não consegue encontrá-lo rapidamente com essas referências rápidas, o vídeo é um compromisso de tempo curto. :)

>[!VIDEO](https://video.tv.adobe.com/v/40892/?quality=12&captions=por_br)

>[!IMPORTANT]
>
>Há uma ID numérica atribuída a cada cliente da Adobe Experience Cloud, geralmente chamada de &quot;PID&quot; ou ID do parceiro. Esta não é a ID sobre a qual estamos falando neste artigo e vídeo. Em vez disso, o &quot;subdomínio do parceiro&quot;, que às vezes é chamado de ID do parceiro, geralmente é uma versão do nome do cliente e é o subdomínio do servidor para o qual os dados são enviados. Por exemplo, se sua empresa é &quot;Bob&#39;s Knobs&quot; (todas as coisas doorknobs, é claro, haha), então é provável que seu subdomínio parceiro seria &quot;bobsknobs&quot;, enquanto o &quot;PID&quot; seria algo mais como &quot;12345&quot;. Normalmente, você não precisa saber seu PID, mas o subdomínio é importante para saber, para que você possa configurar a implementação do Audience Manager.
>
