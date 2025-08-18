---
title: Migrar a implementação do Audience Manager do seu site do DIL do lado do cliente para o encaminhamento do lado do servidor
description: Saiba como migrar a implementação do Audience Manager (AAM) do seu site do DIL do lado do cliente para o encaminhamento do lado do servidor. Este tutorial se aplica se você tiver o AAM e o Adobe Analytics, se enviar ocorrências da página para o AAM usando o código do DIL (Data Integration Library) e se também enviar ocorrências da página para o Adobe Analytics.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# Migrar a implementação do Audience Manager do seu site do DIL do lado do cliente para o encaminhamento do lado do servidor {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Este tutorial se aplica se você tiver o Adobe Audience Manager (AAM) e o Adobe Analytics e estiver enviando uma ocorrência da página para o AAM usando o código do DIL ([!DNL Data Integration Library]) e também uma ocorrência da página para o Adobe Analytics. Como essas duas soluções existem e ambas fazem parte da Adobe Experience Cloud, você tem a oportunidade de seguir a prática recomendada de ativação do encaminhamento pelo lado do servidor, que permite que os servidores de coleta de dados do [!DNL Analytics] encaminhem dados de análise do site em tempo real para o Audience Manager, em vez de fazer com que o código do lado do cliente envie uma ocorrência adicional da página para o AAM. Este tutorial percorre as etapas para fazer a mudança da implementação mais antiga do DIL do lado do cliente para o método mais recente de encaminhamento do lado do servidor.

## Lado do cliente (DIL) vs. lado do servidor {#client-side-dil-vs-server-side}

Ao comparar e contrastar esses dois métodos de envio de dados do Adobe Analytics para o AAM, pode ser útil visualizar as diferenças na seguinte imagem:

![do lado do cliente para o lado do servidor](assets/client-side_vs_server-side_aam_implementation.png)

### Implementação do DIL no cliente {#client-side-dil-implementation}

Se você usar esse método para inserir dados do Adobe Analytics no AAM, terá duas ocorrências provenientes de suas páginas da Web: uma vai para [!DNL Analytics] e outra vai para o AAM (após copiar os dados do [!DNL Analytics] na página da Web. [!UICONTROL Segments] são retornados do AAM para a página, onde podem ser usados para personalização e assim por diante. Essa é uma implementação herdada e não é mais recomendada.

Além de não seguir as práticas recomendadas, as desvantagens de usar esse método incluem:

* Duas ocorrências vindas da página em vez de apenas uma
* o encaminhamento pelo lado do servidor é necessário para o compartilhamento em tempo real de públicos da AAM para [!DNL Analytics], portanto, as implementações do lado do cliente não permitem esse recurso (e possivelmente outros recursos no futuro)

É recomendável mudar para um método de encaminhamento do lado do servidor de implementação do AAM.

### Implementação do encaminhamento pelo lado do servidor {#server-side-forwarding-implementation}

Como mostrado na imagem acima, uma ocorrência vem da página da Web para o Adobe Analytics. [!DNL Analytics] então encaminha esses dados para o AAM em tempo real, e os visitantes são avaliados em características do AAM e [!UICONTROL segments], como se a ocorrência tivesse vindo diretamente da página.

[!UICONTROL Segments] são retornados na mesma ocorrência em tempo real de volta para [!DNL Analytics], que encaminha a resposta à página da Web para personalização, e assim por diante.

Não há desvantagem de tempo para migrar para o encaminhamento pelo lado do servidor. A Adobe recomenda que qualquer pessoa que tenha o Audience Manager e o [!DNL Analytics] use esse método de implementação.

## Você tem duas tarefas principais {#you-have-two-main-tasks}

Há um bocado de informação nesta página, e é tudo importante, é claro. No entanto, tudo **se resume a duas coisas principais que você precisa fazer**:

1. Altere o código DIL do lado do cliente para código de encaminhamento do lado do servidor
1. Inverter a opção no [!DNL Analytics] [!DNL Admin Console] para iniciar o encaminhamento real de dados (por [!UICONTROL report suite])

Se você ignorar qualquer uma dessas tarefas, o encaminhamento pelo lado do servidor não funcionará corretamente. Etapas e dados adicionais foram adicionados a este documento para ajudá-lo a executar essas duas etapas corretamente para sua configuração.

## Opções de implementação {#implementation-options}

À medida que você migra do encaminhamento pelo lado do cliente para o do lado do servidor, uma das tarefas que você terá é alterar o código para o novo código de encaminhamento do lado do servidor. Isso é feito usando uma das seguintes opções:

* Tags do Adobe Experience Platform - opção de implementação recomendada pela Adobe para propriedades da Web. Você perceberá que essa é uma tarefa fácil, já que as tags da Platform fizeram todo o trabalho árduo para você.
* Na página - Você também pode colocar o novo código SSF diretamente na função `doPlugins` dentro do arquivo `appMeasurement.js`, se não estiver (ainda) usando o Adobe Launch
* Outros gerenciadores de tags - Eles podem ser tratados da mesma forma que a opção anterior (Na página), pois você ainda colocará o código SSF em `doPlugins`, onde quer que o outro gerenciador de tags armazene o código [!DNL AppMeasurement]

Examinaremos cada um desses itens abaixo na seção _Atualizando o código_.

## Etapas de implementação {#implementation-steps}

As etapas a seguir descrevem a implementação.

### Etapa 0: Pré-requisito: Experience Cloud ID Service (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

O principal pré-requisito para migrar para o encaminhamento pelo lado do servidor é ter o Serviço da Experience Cloud ID implementado. Isso é feito com mais facilidade se você estiver usando o Experience Platform Launch, nesse caso, basta instalar a extensão ECID e ela fará o resto.

Se você estiver usando um TMS que não seja da Adobe ou nenhum TMS, implemente a ECID para executar **antes** qualquer outra solução da Adobe. Consulte a [documentação da ECID](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=pt-BR) para obter mais detalhes. O único outro pré-requisito é relacionado às versões de código. Portanto, como você simplesmente aplica as versões mais recentes do código nas etapas a seguir, tudo ficará bem.

>[!NOTE]
>
>Leia todo este documento antes de implementar. A seção &quot;Intervalos&quot; abaixo tem informações importantes sobre *quando* você deve implementar cada parte, incluindo a ECID (se ela ainda não estiver implementada).

### Etapa 1: Registrar opções usadas atualmente do código DIL {#step-record-currently-used-options-from-dil-code}

À medida que você se prepara para mudar do código DIL do lado do cliente para o encaminhamento do lado do servidor, a primeira etapa é identificar tudo o que você está fazendo com o código DIL, incluindo configurações personalizadas e dados enviados para o AAM. Os aspectos a serem observados e considerados incluem:

* Variáveis [!DNL Analytics] normais, usando o módulo DIL `siteCatalyst.init` - Você não precisa se preocupar com essa, pois seu trabalho é enviar as variáveis [!DNL Analytics] normais, e isso acontece devido ao simples encaminhamento pelo lado do servidor habilitado.
* Subdomínio do Parceiro - Na função `DIL.create`, anote o parâmetro `partner`. Isso é conhecido como &quot;subdomínio do parceiro&quot;, ou, às vezes, &quot;ID do parceiro&quot;, e será necessário ao inserir o novo código de encaminhamento do lado do servidor.
* [!DNL Visitor Service Namespace] - Também conhecido como &quot;[!DNL Org ID]&quot; ou &quot;[!DNL IMS Org ID]&quot;, você também precisará disso ao configurar o novo código de encaminhamento do lado do servidor. Anote-o.
* containerNSID, uuidCookie e outras opções avançadas - Anote as opções avançadas adicionais que você estiver usando para que também possa defini-las no código de encaminhamento do lado do servidor.
* Variáveis de página adicionais - Se outras variáveis estiverem sendo enviadas para o AAM a partir da página (além das variáveis [!DNL Analytics] normais manipuladas pelo siteCatalyst.init), será necessário anotá-las para que possam ser enviadas por meio do encaminhamento pelo lado do servidor (alerta do spoiler: via [!DNL contextData] variáveis).

### Etapa 2: atualizar o código {#step-updating-the-code}

Em [Opções de implementação](#implementation-options) (acima), várias opções são fornecidas em relação a como e onde você está implementando o encaminhamento pelo lado do servidor. Para que essa seção seja eficaz, precisamos dividi-la nessas seções (com duas delas combinadas). Vá para o método desta seção que melhor descreve suas necessidades.

#### Tags do Adobe Experience Platform {#launch-by-adobe}

Assista ao vídeo abaixo para saber mais sobre como mover as opções de implementação do código DIL do lado do cliente para o encaminhamento do lado do servidor no Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;Na página&quot; ou gerenciador de tags que não seja da Adobe {#on-the-page-or-non-adobe-tag-manager}

Assista ao vídeo abaixo para saber mais sobre como mover opções de implementação do código DIL do lado do cliente para o encaminhamento do lado do servidor no código [!DNL AppMeasurement], que reside em um arquivo ou em um sistema de gerenciamento de tags que não seja da Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Etapa 3: Habilitar o encaminhamento (por [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Até agora, neste tutorial, gastamos todo o nosso tempo na alternância do código do DIL do lado do cliente para o encaminhamento do lado do servidor. Isso é bom, porque é a parte mais difícil. Esta seção, embora você queira vê-la como super fácil, é tão importante quanto atualizar o código. Neste vídeo, você verá como inverter a opção que permite o encaminhamento real de dados do Analytics para o Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**OBSERVAÇÃO:** Conforme dito no vídeo, lembre-se de que levará até 4 horas para que a ativação do encaminhamento seja totalmente implementada no back-end do Experience Cloud.

## Tempo {#timing}

Como lembrete, há duas tarefas principais para migrar do DIL do lado do cliente para o encaminhamento do lado do servidor:

1. Atualização do código
1. Invertendo a opção no [!DNL Analytics] [!DNL Admin Console]

Mas a questão é, qual você faz primeiro? Isso importa? OK, desculpe, foram duas perguntas. Mas as respostas são... depende, e sim, ele *pode* matéria. Como isso é vago? Vamos analisar isso. Mas, primeiro, uma pergunta adicional que pode surgir se você for uma grande organização com vários sites: Eu preciso fazer tudo de uma vez? Aquela é um pouco mais fácil. Não. Você pode fazer isso peça por peça.

### Um mergulho um pouco mais profundo {#a-little-deeper-dive}

O motivo pelo qual o tempo e a ordem são importantes é por causa de como o encaminhamento _realmente_ funciona, que pode ser resumido nos seguintes fatos técnicos:

* Se você tiver o Experience Cloud ID Service (ECID) implementado e a opção no [!DNL Analytics] [!DNL Admin Console] (&quot;a opção&quot;) estiver ativada, os dados SERÃO encaminhados do [!DNL Analytics] para a AAM, mesmo que você ainda não tenha atualizado o código.
* Se você não tiver a ECID implementada, os dados não serão encaminhados, mesmo se o switch estiver ativado e o código de encaminhamento do lado do servidor estiver instalado.
* O código de encaminhamento do lado do servidor (seja nas tags da Platform ou na página) realmente lida com a resposta e é necessário para concluir a migração.
* Lembre-se de que a opção de encaminhamento pelo lado do servidor está habilitada pelo [!UICONTROL report suite], mas que o código é manipulado pela propriedade nas tags da Platform, ou pelo arquivo [!DNL AppMeasurement] se você não usar tags da Platform.

### Práticas recomendadas {#best-practices}

Com base nesses detalhes técnicos, veja as recomendações para o momento do que fazer e quando:

#### Se você AINDA NÃO tiver o ECID implementado {#if-you-do-not-have-ecid-yet-implemented}

1. Inverta a chave em [!DNL Analytics] para cada [!UICONTROL report suite] que você habilitará para o encaminhamento pelo lado do servidor.

   1. O encaminhamento ainda não começa porque você não tem uma ECID.

1. Por site, atualize seu código do DIL do lado do cliente para o encaminhamento do lado do servidor (pode estar nas tags da Platform ) ou na página, conforme discutido em outra seção acima).

   1. O encaminhamento agora flui (já que você adicionou a ECID), e você também deve receber uma resposta JSON adequada ao seu sinal do [!DNL Analytics] (consulte a seção Validação e solução de problemas abaixo para obter mais detalhes).

#### Se você tiver a ECID implementada {#if-you-do-have-ecid-implemented}

1. Prepare e planeje para estar pronto para atualizar seu código do DIL para o encaminhamento pelo lado do servidor PER [!UICONTROL report suite] que você habilitará para o encaminhamento pelo lado do servidor:

   1. Inverta a chave em [!DNL Analytics] para habilitar o encaminhamento pelo lado do servidor.

      1. O encaminhamento SERÁ iniciado porque você tem a ECID ativada.

   1. Assim que possível, atualize seu código do DIL do lado do cliente para o encaminhamento de lado único (isso pode estar nas tags da Platform ou na página, conforme discutido em outra seção acima).

      1. Você deve receber uma resposta JSON adequada ao seu sinal do [!DNL Analytics] (consulte a seção [Validação e solução de problemas](#validation-and-troubleshooting) abaixo para obter mais detalhes).

>[!NOTE]
>
>É importante fazer essas duas etapas o mais próximo possível uma da outra, pois, entre as etapas 1 e 2 acima, você terá uma duplicação de dados que entram no AAM. Em outras palavras, o encaminhamento por lado único terá começado a enviar dados do [!DNL Analytics] para o AAM e, como o código do DIL ainda está na página, também haverá uma ocorrência indo diretamente da página para o AAM, dobrando os dados. Assim que você atualizar o código do DIL para o encaminhamento do lado do servidor, isso será atenuado.

>[!NOTE]
>
>Se você preferir ter uma pequena discrepância nos dados, em vez de uma pequena duplicação de dados, é possível alternar a ordem das etapas 1 e 2 acima. Mover o código do DIL para o encaminhamento do lado do servidor interromperia o fluxo de dados no AAM até que você pudesse virar a opção para ativar o encaminhamento do lado do servidor para o [!UICONTROL report suite]. Normalmente, os clientes preferem uma pequena duplicação de dados a não conseguir colocar os visitantes em características e [!UICONTROL segments].

#### Tempo de migração quando você tem muitos sites e [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Este tópico é brevemente abordado nas seções anteriores, na medida em que a estratégia principal pode ser resumida do seguinte modo:

Migre um site/[!UICONTROL report suite] (ou grupo de sites/[!UICONTROL report suites]) de cada vez.

No entanto, isso pode ser complicado com base em alguns cenários possíveis:

* Você tem um site que contém vários [!UICONTROL report suites] distintos
* Você tem um [!UICONTROL report suite] que inclui vários sites (como um [!UICONTROL report suite] global)
* Você usa uma propriedade de tags da Platform para abranger vários sites
* Você tem equipes de desenvolvimento diferentes para sites diferentes

Por causa desses itens, pode ficar um pouco complicado. As melhores coisas que posso sugerir são:

* Reserve algum tempo para criar uma estratégia de migração para o encaminhamento pelo lado do servidor, com base nas explicações acima
* Com base no fato de que uma única propriedade nas tags da Platform (ou um único arquivo [!DNL AppMeasurement]) normalmente mapeia para 1 ou 2 [!UICONTROL report suites] distintos, você provavelmente poderá fazer um plano que funcione nesses grupos distintos um por um, atualizando sua empresa para o encaminhamento pelo lado do servidor
* Se você estiver trabalhando com a Adobe Consulting, converse com eles sobre o seu plano de migração, para que possam ajudar conforme necessário

## Validação e solução de problemas {#validation-and-troubleshooting}

A forma principal de validar que o encaminhamento pelo lado do servidor está em execução é observar a resposta a qualquer uma das ocorrências do Adobe Analytics que vierem do aplicativo.

Se você não estiver fazendo o encaminhamento de dados pelo lado do servidor do [!DNL Analytics] para o Audience Manager, então não há resposta para o sinal do [!DNL Analytics] (fora um pixel com dimensões 2x2). No entanto, se você estiver fazendo o encaminhamento pelo lado do servidor, há itens que você pode verificar na solicitação e resposta do [!DNL Analytics] que informam que o [!DNL Analytics] está se comunicando corretamente com o Audience Manager, encaminhando a ocorrência e obtendo uma resposta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>Cuidado com o falso &quot;Sucesso&quot;. Se houver uma resposta e tudo parecer estar funcionando, verifique se você tem o objeto `stuff` na resposta. Caso contrário, você poderá ver uma mensagem que diz `"status":"SUCCESS"`. Por mais louco que pareça, isso prova que NÃO está funcionando corretamente.
>
>Caso esteja lendo isso, significa que você concluiu a atualização de código nas tags da Platform para [!DNL AppMeasurement], mas que o encaminhamento no [!DNL Analytics] [!DNL Admin Console] ainda não foi concluído. Nesse caso, é necessário verificar se você habilitou o encaminhamento pelo lado do servidor no [!DNL Analytics] [!DNL Admin Console] para o [!UICONTROL report suite]. Se tiver ativado e ainda não tiver passado quatro horas, aguarde, pois pode demorar para fazer todas as alterações necessárias no back-end.


![falso sucesso](assets/falsesuccess.png)

Para obter mais informações sobre o encaminhamento pelo lado do servidor, consulte a [documentação](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=pt-BR).
