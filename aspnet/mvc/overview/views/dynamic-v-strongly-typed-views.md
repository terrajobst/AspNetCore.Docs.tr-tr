---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "Dinamik v. Görünümleri'kesin türü belirtilmiş | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a>Dinamik v. Kesin türü belirtilmiş görünümleri
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

ASP.NET MVC 3'te bir görünüm için bir denetleyicisinden bilgi aktarmak için üç yolu vardır:

1. Kesin türü belirtilmiş model nesnesi.
2. Dinamik tür olarak (kullanarak @model dinamik)
3. Görünüm paketini kullanma

Karşılaştırmak ve dinamik ve kesin türü belirtilmiş görünümleri karşıtlık için basit bir MVC 3 üst Blog uygulaması yazılmış. Denetleyici bloglar basit bir listesi ile başlar:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

IndexNotStonglyTyped() yönteminde sağ tıklayın ve Razor görünüm ekleyin.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Emin olun **kesin türü belirtilmiş görünüm oluşturmak** kutusu işaretli değildir. Sonuçta elde edilen görünümü çok içermiyor:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Biz dinamik ve kesin türü belirtilmiş görünümleri kullandığımızdan, IntelliSense bize yardımcı değil. Tamamlanan kodu aşağıda gösterilmiştir:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Kesin türü belirtilmiş görünüm şimdi ekleyeceğiz. Denetleyici için aşağıdaki kodu ekleyin:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Tam olarak aynı dönüş View(topBlogs) olduğuna dikkat edin; olmayan-kesin türü belirtilmiş görünüm çağırın. İçinde sağ tıklayın *StonglyTypedIndex()* seçip **Görünüm Ekle**. Bu süre seçin **Blog** Model sınıfı ve seçin **listesi** İskele şablon olarak.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

İçinde yeni şablonu görüntüleme IntelliSense desteği alın.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

C# projesinde indirilen [burada](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
