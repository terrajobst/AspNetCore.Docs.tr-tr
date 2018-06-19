---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Silme yöntemleri ve ayrıntıları İnceleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 00f7e5d6679f1bd8875931e601c8151049f785ac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868782"
---
<a name="examining-the-details-and-delete-methods"></a>Silme yöntemleri ve ayrıntıları İnceleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.


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

Bu sorunu anlamak sıralamak için şunları yapabilirsiniz. Farklı adlar yöntemleri vermek biridir. Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak bağlanamayacak. Eklemek için örnekte bkz çözümdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Bir URL içeren bu etkin yönlendirme sistemi için eşleme gerçekleştirir <em>/Delete/</em>için bir POST isteği bulacaksınız `DeleteConfirmed` yöntemi.

Aynı ad ve imza sahip yöntemleri ile ilgili bir sorun önlemek için başka bir yaygın yapay olarak kullanılmayan bir parametre eklemek için POST yöntemini imza değiştirmek için yoludur. Örneğin, bir parametre türü bazı geliştiriciler ekleyin `FormCollection` POST yöntemine geçirilen ve ardından parametresi yalnızca kullanmayın:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Özet

Şimdi yerel bir DB veritabanında veri depolayan tam bir ASP.NET MVC uygulamasına sahip. Oluşturma, okuma, güncelleştirme, silme ve filmler için arama yapın.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Sonraki Adımlar

Yerleşik ve bir web uygulaması test sonra sonraki diğer kişilere Internet üzerinden kullanmak için kullanılabilir hale getirmek için adımdır. Bunu yapmak için bir web barındırma sağlayıcısına dağıtmak zorunda. Microsoft, 10 web siteleri için ücretsiz bir web barındırma sunar bir [Ücretsiz Windows Azure deneme sürümü hesabı](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). I my öğreticinin sonraki izleyin önermek [üyelik, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC uygulaması Windows Azure Web sitesine dağıtmak](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Zel Dykstra'nin orta düzey mükemmel bir öğreticidir [bir ASP.NET MVC uygulaması için Entity Framework veri modeli oluşturma](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) ve [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx) mükemmel yerleştirir sorular sormak için olan. İzleyin [bana](https://twitter.com/RickAndMSFT) my son öğreticileri güncelleştirmeleri alabilmek için Twitter'da.

Geri bildirim Hoş Geldiniz.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Önceki](adding-validation-to-the-model.md)
