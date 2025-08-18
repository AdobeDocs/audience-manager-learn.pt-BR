---
title: Suporte ao IAB TCF 2.2
description: Saiba mais sobre o Plug-in do Audience Manager para a TCF do IAB e como ele funciona com o objeto de aceitação da Adobe e seu Provedor de gerenciamento de consentimento (CMP).
feature: Data Governance & Privacy
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: f9708e705d95b43084ff11e342dc54ff11d6326c
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 0%

---

# Suporte IAB TCF 2.2 no Audience Manager {#iab-tcf-support-in-audience-manager}

O Adobe fornece o meio de gerenciar e comunicar as opções de privacidade de seus usuários por meio da funcionalidade de aceitação e pelo plug-in do Audience Manager para o suporte à Estrutura de transparência e consentimento 2.2 (TCF 2.2) do IAB. Este artigo trabalha em conjunto com a documentação para ajudar você a entender o Plug-in do Audience Manager para a TCF do IAB e como ele funciona em conjunto com o objeto de Opt-in da Adobe e seu Provedor de gerenciamento de consentimento (CMP). Para saber mais sobre o IAB, visite o site em [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Primeira etapa: Entender a aceitação da Experience Cloud ID {#first-step-understand-ecid-s-opt-in}

Para entender como trabalhar com a TCF do IAB, primeiro você deve entender a funcionalidade [!DNL Opt-in], que faz parte da biblioteca do Serviço da Experience Cloud ID (ECID). Se você não estiver familiarizado com o funcionamento da aceitação, consulte [este artigo útil](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) primeiro. Você também deve consultar a [documentação](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html) sobre a aceitação. Depois de passar por esses recursos, retorne a esta página e continue.

## O plug-in do Audience Manager para IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Agora que você tem pelo menos uma compreensão básica de como o serviço de Opt-in funciona, a Audience Manager pode criar camadas para ele no suporte ao [!DNL IAB Transparency and Consent Framework (TCF)], que é feito por meio de um plug-in no objeto de Opt-in.

O plug-in do Audience Manager para a TCF do IAB estende a funcionalidade do Opt-in e permite que os clientes da AAM avaliem, honrem e encaminhem as opções de privacidade do usuário para parceiros downstream de acordo com a TCF do IAB. Ele fornece um padrão que os controladores de dados (ou seja, você como cliente da Adobe) e os fornecedores (DMPs, DSPs, SSPs, Ad Servers etc.) podem usar para entender o consentimento em todo o cenário de consentimento.

## Habilitar IAB TCF {#enabling-iab-tcf}

Habilitar o Plug-in do Audience Manager para a TCF do IAB é fácil se você estiver usando o Adobe Experience Platform Launch, pois é uma caixa de seleção simples, como mostrado no vídeo curto abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

Como alternativa, se você não estiver usando o Launch, poderá usar `isIabContext=true` para habilitá-lo quando instanciar o Visitante do Experience Cloud. Isso inicia o fluxo da TCF do IAB, ou seja, adiciona outra etapa para a coleta de consentimento, usando a TCF do IAB para consultar a cadeia de caracteres da TC do IAB e a fornece de volta ao Opt-in, que, por sua vez, se comunica com as soluções da Experience Cloud.

## Sequência de caracteres IAB TC {#iab-tcf-consent-string}

Um dos padrões que o IAB fornece é uma &quot;string de consentimento&quot; (também conhecida como &quot;DaisyBit&quot;), que são realmente duas listas reunidas:

1. Finalidades: **O que** está sendo consentido?
1. Fornecedores: **A quem** está sendo dado o consentimento?

### Finalidades {#purposes}

Com a TCF 2.2 do IAB, há dez &quot;finalidades&quot; para as quais coletar o consentimento (o que os fornecedores podem fazer com os dados do visitante). A Adobe Audience Manager não exige todos os dez, mas apenas consentimento para os seguintes fins, além do consentimento do fornecedor:

* **Finalidade 1:** Armazenar e/ou acessar informações em um dispositivo;
* **Finalidade 10:** desenvolver e melhorar produtos;
* **Propósito Especial 1:** garanta a segurança, evite fraudes e depure.

Esta é a primeira parte da sequência TC do IAB e é registrada como 1&#39;s e 0&#39;s, ditando se essa finalidade/atividade é aprovada ou não.

>[!NOTE]
>
>De acordo com os regulamentos do IAB, o Propósito Especial 1 (Garantir segurança, evitar fraudes e depurar) é sempre aceito e os usuários não podem se opor a ele.

### Fornecedores {#vendors}

Outra parte da cadeia de caracteres IAB TC é uma longa lista de várias centenas de fornecedores, para que os visitantes possam receber uma lista de fornecedores aplicáveis que têm tags no site e podem escolher quais fornecedores usar. Os fornecedores mantêm seu lugar na lista. Por exemplo, o número do fornecedor da Adobe Audience Manager nessa lista é 565. Se esse número na lista tiver um &quot;1&quot;, a Audience Manager poderá fazer as finalidades aprovadas na frente da lista. Se o ponto do AAM tiver um &quot;0&quot;, ele não poderá fazer nada com os dados.

**Para que a Audience Manager forneça uma interface para que os clientes usem a TCF do IAB para escolher essas finalidades e fornecedores, ou para aprovar/desaprovar todas as atividades, você deve usar um parceiro de CMP que esteja registrado com a TCF do IAB ou compilar um que ofereça suporte à TCF do IAB e esteja registrado com a TCF do IAB.**

## Opt-in: tradução entre aplicativos IAB e Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

Um dos benefícios de usar a TCF do IAB é que as finalidades padrão listadas acima provavelmente dão ao usuário final mais uma ideia do que estão aprovando do que uma lista de soluções da Adobe. Os usuários finais podem não saber o que significa &quot;aprovar&quot; o Audience Manager ou o [!DNL Target], mas &quot;armazenar e/ou acessar informações em um dispositivo&quot; ou &quot;desenvolver e melhorar produtos&quot; provavelmente é mais fácil de entender e consentir.

Para que o Audience Manager seja aprovado (ou seja, Para que a tradução das finalidades do IAB para Opt-in dê um voto &quot;sim&quot; ao AAM, as finalidades 1 e 10, conforme listadas acima, precisam receber o consentimento do usuário final. Se qualquer uma dessas opções não for aprovada ou se um fornecedor não for aprovado, o AAM não executará disparos de pixels nem definirá cookies. Também é bom saber que muitos clientes simplesmente optam por fornecer ao usuário final uma interface do usuário &quot;tudo ou nada&quot;, o que, é claro, permitiria ou proibiria o uso do Audience Manager (e de outras soluções da Experience Cloud).

Há algumas informações excelentes na [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en) sobre como o plug-in do Audience Manager para o fluxo da TCF do IAB se aplica aos casos de uso do Editor e do Anunciante.

## IAB: Envio de consentimento em downstream {#iab-sending-consent-downstream}

Quando o Plug-in do Audience Manager para a TCF do IAB é usado, as opções de consentimento do usuário também são enviadas às sincronizações de ID no nível da plataforma (terceiros) para parceiros presentes na Lista global de fornecedores, para que o parceiro tenha as informações de consentimento do usuário e possa agir também em relação a elas. Essas informações são enviadas em duas variáveis:

* gdpr = 1
* gdpr_consent = [cadeia de consentimento codificada]

O problema é que, se o usuário estiver no contexto do IAB e não fornecer consentimento (ou fornecer consentimento negativo), o Audience Manager não coletará a cadeia de caracteres do IAB TC e, como tal, descartará as chamadas. Então, nesse caso... não há transmissão de consentimento a jusante.

## Demonstração {#demo}

No vídeo abaixo, veja como os cookies e os beacons da ECID e das soluções são afetados pelas seleções de opções do usuário do IAB.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

Para obter informações mais detalhadas sobre o Plug-in do Audience Manager para TCF do IAB 2.2, incluindo como implementar e testar, casos de uso e fluxo de trabalho, consulte a [documentação](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html).
