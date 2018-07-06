---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: ASP.NET Web API OData 5.3 sürümündeki yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: c185e7ef9bfe6e21601ab61c418c63c8a81b680a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827035"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>ASP.NET Web API OData 5.3 sürümündeki yenilikler
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

Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur. NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticilerde ve diğer belgelerde ASP.NET Web API OData hakkında bulabilirsiniz [ASP.NET web sitesi](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData çekirdek kitaplıkları

OData v4 için Web API artık ODataLib sürüm 6.5.0 kullanır.

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Yeni özellikleri ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>$ $Levels genişletmek için destek

$Levels kullanabileceğiniz sorgu seçeneği $ sorguları genişletin. Örneğin:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Bu sorgu için eşdeğerdir:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Açık varlık türleri için destek

Bir *açık tür* ek tür tanımında bildirilen herhangi bir özelliği olarak dinamik özellikleri içeren bir stuctured türüdür. Açık türler, veri modelleri için esneklik eklemenize olanak sağlar. Daha fazla bilgi için xxxx bakın.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Dinamik koleksiyon özelliklerinde açık türler için destek

Daha önce bir dinamik özellik tek bir değer olması gerekiyordu. 5.3 içinde dinamik özellikler koleksiyonu değerlere sahip olabilir. Örneğin, aşağıdaki JSON yükü olarak `Emails` dinamik bir özelliktir ve dize türü koleksiyonudur:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Karmaşık türleri için devralma desteği

Artık karmaşık türler, temel türünden devralınabilir. Örneğin, bir OData hizmeti aşağıdaki karmaşık türler tanımlayabilirsiniz:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Bu örneğin EDM şu şekildedir:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Daha fazla bilgi için [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET Web API OData 5.3 bozucu değişiklikler açıklanmaktadır.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Sorgu Seçenekleri

Sorun: $levels ile genişletin kullanarak iç içe geçmiş $ bir yanlış genişletme derinliğini, en büyük sonuç =.

Örneğin, aşağıdaki isteği verilen:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Varsa `MaxExpansionDepth` 5'tir, bu sorgu 6 bir genişletme derinliğini neden olur.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Hata düzeltmeleri ve alt özellik güncelleştirmeleri

Bu sürüm çeşitli hata düzeltmeleri ve küçük özelliği içerir güncelleştirmeleri. Tam listesini burada bulabilirsiniz:

- [Hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

Bu sürümde yaptık bir [hata düzeltmesi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) AllowedFunctions numaralandırmalar bazılarının. Bu sürüm, herhangi bir hata düzeltmeleri veya yeni özellikleri içermez.
