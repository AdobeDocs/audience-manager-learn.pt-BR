---
title: Atualização para o Adobe Audience Manager DIL versão 8.0 (ou superior)
description: Este artigo fornecerá etapas e recomendações sobre como atualizar o código do Adobe Audience Manager (AAM) Data Integration Library (DIL) para a versão 8.0 ou posterior. Isso se refere à implementação do DIL "no lado do cliente", não ao encaminhamento do lado do servidor de dados do Adobe Analytics, e abordará o DTM, o Launch by Adobe e implementações sem uma solução de gerenciador de tags da Adobe.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# Atualização para o DIL versão 8.0 (ou superior) do Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Este artigo fornecerá etapas e recomendações sobre como atualizar o código do Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) para a versão 8.0 ou posterior. Isso se refere à implementação do DIL &quot;no lado do cliente&quot;, não ao encaminhamento do lado do servidor de dados do Adobe Analytics, e abordará o DTM, o Launch by Adobe e implementações sem uma solução de gerenciador de tags da Adobe.

## Visão geral {#overview}

O código [!DNL Data Integration Library] (DIL) da Audience Manager permite implementar o AAM em seu site*. Ao implementar versões anteriores do DIL, não era necessário ter o Experience Cloud ID Service (ECID) da Adobe implementado também (embora fosse uma ótima ideia). A partir do DIL versão 8.0, há uma dependência forte na ECID versão 3.3 ou posterior. Se você implementar o DIL 8.0 ou posterior sem a ECID 3.3 ou com uma versão anterior, ocorrerá um erro e ela não funcionará. Como há várias maneiras de implementar o AAM, criamos esta página para fornecer algumas etapas a serem seguidas, bem como algumas recomendações. Abaixo, você encontrará essas etapas e recomendações divididas por plataforma/método de implementação. Mais informações sobre o DIL estão disponíveis na [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=pt-BR).

* Conforme dito na descrição desta página, isso cobrirá apenas implementações do DIL &quot;do lado do cliente&quot;, usadas por clientes do AAM que não têm o Adobe Analytics. Se tiver o Adobe Analytics, você deve usar o método de encaminhamento do lado do servidor para implementar o AAM. Este método está descrito na [documentação](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=pt-BR).

## Métodos e elementos duplicados e obsoletos {#duplicate-and-deprecated-elements-and-methods}

Em versões anteriores do DIL e da ECID, havia métodos duplicados (métodos que fazem a mesma função no DIL e na ECID), o que causou confusão em relação a qual usar. Normalmente, você precisava usar os dois e combiná-los, e essa mensagem não era bem comunicada aos nossos clientes. A partir do DIL 8.0, esses métodos e elementos duplicados foram descontinuados no DIL e é recomendável usar a versão ECID.

Por exemplo:

* Ao usar o [!DNL DIL.create], alguns elementos foram descontinuados e você deve usar os elementos ECID. Estes elementos são chamados na [[!DNL DIL.create] documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html?lang=pt-BR).
* O método de nível de instância [!DNL idSync] também foi descontinuado, conforme chamado na [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html?lang=pt-BR) do método.

## Sincronização de ID com uma ID do cliente {#id-syncing-with-a-customer-id}

No AAM, você pode sincronizar sua UUID (ID de usuário exclusiva anônima) na máquina com uma ID do cliente, para poder carregar dados offline sobre esse cliente e vinculá-los ao comportamento online dele, para obter uma melhor compreensão dos clientes. No passado, isso foi feito de uma das duas seguintes maneiras:

* O método de nível de instância [!DNL idSync]
* O elemento [!DNL declaredId] em [!DNL DIL.create]

Se você tem usado um desses métodos mais antigos para sincronizar com uma ID do cliente, é altamente recomendável atualizar para usar o método [!DNL setCustomerIDs], que faz parte do Serviço ECID. Mais informações sobre [!DNL setCustomerIDs] estão disponíveis na [documentação](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=pt-BR) do método.

**Dica Rápida:** Ao usar um dos métodos acima, você fez referência à AAM [!UICONTROL Data Source] com a ID [!UICONTROL Data Source] (também conhecida como &quot;DPID&quot;). Ao atualizar para [!DNL setCustomerIDs], será necessário usar &quot;[!UICONTROL Data Source]&quot; do AAM [!UICONTROL Integration Code]. Ele ainda aponta para o mesmo [!UICONTROL Data Source], mas é apenas um identificador diferente. Isso é mostrado no vídeo abaixo.

>[!VIDEO](https://video.tv.adobe.com/v/33336/?quality=12&captions=por_br)

As seguintes seções listam as etapas e recomendações para atualizar para o DIL 8.0 com base no seu método de implementação:

## Atualização para o DIL 8.0 em tags do Adobe Experience Platform {#updating-to-dil-in-experience-platform-launch}

Etapas básicas para atualização para o DIL 8.0

1. Se estiver usando DIL anterior ao 8.0, antes de atualizar, acesse a configuração do DIL na extensão do AAM e anote todas as opções avançadas que estiver usando (a serem usadas em uma etapa subsequente).
1. Atualize sua extensão do AAM para a versão 8.0 ou superior.
1. Verifique se a extensão do Experience Cloud ID Service é versão 3.3.0 ou superior.
1. Para quaisquer métodos/elementos obsoletos (como `disableIDSyncs`) que estivessem na sua extensão do AAM anterior à 8.0 ou no código personalizado do DIL, ative os métodos ECID na extensão ECID.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. Publique as alterações.

>[!VIDEO](https://video.tv.adobe.com/v/33337/?quality=12&captions=por_br)

## Atualização para o DIL 8.0 no Adobe DTM {#updating-to-dil-in-adobe-dtm}

1. Atualize sua ferramenta AAM para a versão 8.0 ou superior. Essa configuração de versão está na seção &quot;Geral&quot; da ferramenta AAM.
1. Para todos os métodos/elementos obsoletos (como `disableIDSyncs`) que estavam no código personalizado da Ferramenta AAM anterior ao 8.0 para DIL, anote-os (para que você possa adicioná-los à ferramenta ECID) e remova-os do [!DNL DIL code] personalizado na ferramenta AAM.
1. Atualize sua extensão do Experience Cloud ID Service para a versão 3.3.0 ou superior
1. Adicione as opções avançadas à ferramenta ECID que você removeu do código personalizado da ferramenta AAM.
1. Publicar as alterações

## Atualização para o DIL 8.0 sem solução da Adobe Tag Management {#additional-resources}

Se você estiver atualizando seu código diretamente na página, poderá apenas substituir itens mais antigos por itens mais novos, exceto quando for necessário mover métodos/elementos do DIL para a ECID, conforme discutido acima. Nesse caso, basta substituir o método/elemento antigo no local do DIL pelo método/elemento da ECID no local da ECID.

O mesmo vale para os gerenciadores de tags que não são da Adobe. Sempre que você tiver as versões antigas nessa solução de gerenciamento de tags, substitua-a pelo novo código, conforme descrito nas etapas a seguir.

1. Atualize sua biblioteca do DIL para a versão mais recente (8.0 ou superior). Você precisará obter o código do DIL mais recente no Atendimento ao cliente da Adobe Consulting ou Adobe, pois ele não está disponível atualmente em um local público. Em seguida, basta substituir o código antigo da biblioteca DIL pelo novo código da biblioteca DIL e seguir para a próxima etapa (não pare agora ou você terá problemas).
1. Instale o [!DNL ECID Service] ou atualize sua versão existente para a 3.3.0 ou posterior. Você pode baixar a versão mais recente do Serviço da Experience Cloud ID [da nossa página do GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Se precisar de ajuda, consulte a [documentação](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=pt-BR) ou fale com um consultor da Adobe.

1. Verifique se todos os métodos ou elementos obsoletos que estão no código personalizado do DIL foram movidos para os métodos ECID:

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [Documentação](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=pt-BR)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [Documentação](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=pt-BR)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [Documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html?lang=pt-BR)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [Documentação](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=pt-BR)
