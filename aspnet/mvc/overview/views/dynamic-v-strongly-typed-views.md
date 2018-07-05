---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dinamik ve Kesin tür belirtilmiş görünümleri | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 7622ca8248374da27f4190075df5a6bfc32bb2e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389093"
---
<a name="dynamic-v-strongly-typed-views"></a>Dinamik ve Kesin türü belirtilmiş görünümler
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

ASP.NET MVC 3'te bir görünüm için bir denetleyiciden bilgi geçirmek için üç yolu vardır:

1. Kesin olarak belirlenmiş model nesnesi.
2. Dinamik tür olarak (kullanarak @model dinamik)
3. Görünüm paketini kullanma

Ben karşılaştırın ve dinamik ve kesin türü belirtilmiş görünümleri için basit bir MVC 3 üst Blog uygulaması yazmamış. Denetleyici basit bir listesini blogları ile başlar:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

IndexNotStonglyTyped() yönteminde sağ tıklayın ve bir Razor görünüm ekleyin.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Emin **kesin türü belirtilmiş görünüm oluşturmak** kutunun işaretli değil. Ekranda çok içermiyor:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Dinamik ve bir kesin türü belirtilmiş görünüm kullandığımızdan, IntelliSense bize yardımcı olmaz. Tamamlanan kodu aşağıda gösterilmiştir:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Kesin türü belirtilmiş görünüm şimdi ekleyeceğiz. Denetleyici için aşağıdaki kodu ekleyin:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Tam olarak aynı dönüş View(topBlogs) olduğuna dikkat edin; kesin olmayan türü belirtilmiş görünüm çağırın. İçine sağ tıklayın *StonglyTypedIndex()* seçip **Görünüm Ekle**. Bu süre seçin **Blog** seçin ve Model sınıfı **listesi** İskele şablon olarak.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Yeni Görünüm şablon içinde size IntelliSense desteği alın.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

C# projesi indirilebilir [burada](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
