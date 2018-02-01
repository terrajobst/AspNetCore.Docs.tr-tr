---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: ASP.NET Web API OData 5.3 yenilikler | Microsoft Docs
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>ASP.NET Web API OData 5.3 yenilikler nelerdir?
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web API OData 5.3 için Yenilikler açıklanmaktadır.

- [İndir](#download)
- [Belgeler](#documentation)
- [OData çekirdek kitaplıkları](#corelib)
- [Yeni Özellikler](#newf)
- [Bilinen sorunlar ve yeni değişiklikler](#known-issues)
- [Hata düzeltmeleri](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır. Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET Web API OData ilgili diğer belgeler bulabilirsiniz [ASP.NET web sitesi](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData çekirdek kitaplıkları

OData v4 için Web API artık ODataLib sürüm 6.5.0 kullanır.

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Yeni özellikleri ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>$ $Levels genişletmek için destek

$Levels kullanabileceğiniz sorgu seçeneği $ sorguları genişletin. Örneğin:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Bu sorgu eşdeğerdir:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Açık varlık türleri için destek

Bir *açmak türü* tür tanımında bildirilen herhangi bir özellik yanı sıra Dinamik Özellikler içeren bir stuctured türüdür. Açık türleri, veri modelleri için esneklik eklemenize olanak sağlar. Daha fazla bilgi için xxxx bakın.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Açık türlerinde dinamik koleksiyon özellikleri için destek

Daha önce dinamik bir özelliğe tek bir değer olması gerekiyordu. 5.3 içinde dinamik özellikleri koleksiyonu değerlere sahip olabilir. Örneğin, aşağıdaki JSON yükündeki, `Emails` dinamik bir özelliktir ve dize türü koleksiyonudur:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Karmaşık türler için devralma desteği

Şimdi karmaşık türler temel türünden devralabilirsiniz. Örneğin, bir OData hizmeti aşağıdaki karmaşık türler tanımlayabilirsiniz:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Bu örneğin EDM şöyledir:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Daha fazla bilgi için bkz: [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET Web API OData 5.3 önemli değişiklikler açıklanmaktadır.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Sorgu Seçenekleri

Sorun: $levels ile genişletin kullanarak iç içe geçmiş $ yanlış genişletme derinliğini max sonuçlarında =.

Örneğin, aşağıdaki isteği verilen:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Varsa `MaxExpansionDepth` 5'tir, bu sorgu 6 bir genişletme derinliğini neden olur.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Hata düzeltmeleri ve küçük özellik güncelleştirmeleri

Bu sürüm çeşitli hata düzeltmeleri ve küçük özellik içerir güncelleştirmeler. Tam listesini burada bulabilirsiniz:

- [Hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

Biz bu sürümde yapılan bir [hata düzeltmesi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) AllowedFunctions numaralandırmalar, bazı. Bu sürüm, herhangi bir hata düzeltmeleri veya yeni özellikler sahip değil.
