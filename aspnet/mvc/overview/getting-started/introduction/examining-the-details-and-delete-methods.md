---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: "Silme yöntemleri ve ayrıntıları İnceleme | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 6d7d0fe5bd2f6a6bd7f9c7ca04a8f142223ccf8e
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2017
---
<a name="examining-the-details-and-delete-methods"></a>Silme yöntemleri ve ayrıntıları İnceleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

Öğreticinin bu bölümünde, otomatik olarak oluşturulan inceleyeceğiz `Details` ve `Delete` yöntemleri.

## <a name="examining-the-details-and-delete-methods"></a>Silme yöntemleri ve ayrıntıları İnceleme

Açık `Movie` denetleyicisi ve inceleyin `Details` yöntemi.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Bu eylem yöntemine oluşturulan MVC yapı iskelesi altyapısı yöntemini çağıran bir HTTP isteği gösteren bir açıklama ekler. Bu durumda olduğu bir `GET` üç URL kesimleri istekle `Movies` denetleyicisi `Details` yöntemi ve `ID` değeri.

Kod ilk veri kullanarak için arama yapmayı kolaylaştırır `Find` yöntemi. Yönteme yerleşik bir önemli güvenlik kodu doğrular özelliğidir `Find` kodu şey denemeden önce yöntemi bir filmi buldu. Örneğin, bir bilgisayar korsanının siteye bağlantılardan tarafından oluşturulan URL değiştirerek hatalara `http://localhost:xxxx/Movies/Details/1` gibi bir `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir filmi temsil etmez başka bir değer). Null film denetlemiyordu, boş bir filmi bir veritabanı hatası neden olur.

İncelemek `Delete` ve `DeleteConfirmed` yöntemleri.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Unutmayın `HTTP Get``Delete` yöntemi belirtilen film Sil değil, burada gönderebilirsiniz film bir görünümünü verir (`HttpPost`) silme... Bir GET'e yanıt olarak bir silme işlemi isteği (veya bir düzenleme işlemi gerçekleştirilirken Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) güvenlik boşluğu açar. Bu konu hakkında daha fazla bilgi için Stephen Walther'ın blog girişine bakın [ASP.NET MVC ipucu #46 — güvenlik açıklarını oluşturduğundan bağlantılarını sil kullanmayan](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Verileri siler yöntemi adlandırılan `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için. İki yöntem imzaları aşağıda verilmiştir:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Ortak dil çalışma zamanı (CLR) benzersiz parametre imzası (aynı yöntem adı ancak farklı parametrelerin listesi) sağlamak için aşırı yüklenmiş yöntemler gerektirir. Ancak, burada, her ikisi de aynı parametre imzaya sahip iki silme yöntemleri--biri--get ve POST için gerekir. (Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)

Bu sorunu anlamak sıralamak için şunları yapabilirsiniz. Farklı adlar yöntemleri vermek biridir. Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak bağlanamayacak. Eklemek için örnekte bkz çözümdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Bir URL içeren bu etkin yönlendirme sistemi için eşleme gerçekleştirir */Delete/* için bir POST isteği bulacaksınız `DeleteConfirmed` yöntemi.

Aynı ad ve imza sahip yöntemleri ile ilgili bir sorun önlemek için başka bir yaygın yapay olarak kullanılmayan bir parametre eklemek için POST yöntemini imza değiştirmek için yoludur. Örneğin, bir parametre türü bazı geliştiriciler ekleyin `FormCollection` POST yöntemine geçirilen ve ardından parametresi yalnızca kullanmayın:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Özet

Şimdi yerel bir DB veritabanında veri depolayan tam bir ASP.NET MVC uygulamasına sahip. Oluşturma, okuma, güncelleştirme, silme ve filmler için arama yapın.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Sonraki Adımlar

Yerleşik ve bir web uygulaması test sonra sonraki diğer kişilere Internet üzerinden kullanmak için kullanılabilir hale getirmek için adımdır. Bunu yapmak için bir web barındırma sağlayıcısına dağıtmak zorunda. Microsoft, 10 web siteleri için ücretsiz bir web barındırma sunar bir [ücretsiz Azure deneme hesabı](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604). I my öğreticinin sonraki izleyin önermek [bir Güvenli ASP.NET MVC uygulaması üyeliği, OAuth ve SQL veritabanı ile Azure'a dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Zel Dykstra'nin orta düzey mükemmel bir öğreticidir [bir ASP.NET MVC uygulaması için Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) ve [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx) mükemmel yerleştirir sorular sormak için olan. İzleyin [bana](https://twitter.com/RickAndMSFT) my son öğreticileri güncelleştirmeleri alabilmek için Twitter'da.

Geri bildirim Hoş Geldiniz.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter:[@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter:[@shanselman](https://twitter.com/shanselman)

>[!div class="step-by-step"]
[Önceki](adding-validation.md)
