---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Details ve Delete metotlarını İnceleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: c68d7a022f14bdcf2a08696691d2b6ef888a8d54
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576483"
---
<a name="examining-the-details-and-delete-methods"></a>Details ve Delete metotlarını İnceleme
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Öğreticinin bu bölümünde, otomatik olarak oluşturulan inceleyeceğiz `Details` ve `Delete` yöntemleri.

## <a name="examining-the-details-and-delete-methods"></a>Details ve Delete metotlarını İnceleme

Açık `Movie` denetleyicisi ve incelemek `Details` yöntemi.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Bu eylem yöntemine oluşturulan MVC yapı iskelesi altyapısı yöntemi çağıran bir HTTP isteği gösteren bir yorum ekler. Bu durumda, bir `GET` üç URL kesimleri, istekle `Movies` denetleyicisi `Details` yöntemi ve bir `ID` değeri.

Kod ilk kullanarak verileri için arama yapmayı kolaylaştırır `Find` yöntemi. Kod doğrular yönteme yerleşik bir önemli güvenlik özelliği olduğu `Find` yöntemi kod şey denemeden önce bir filmi buldu. Örneğin, bir bilgisayar korsanının hataları siteye bağlantılardan tarafından oluşturulan URL değiştirerek neden olabilirdi `http://localhost:xxxx/Movies/Details/1` gibi bir şey `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir film temsil etmez başka bir değer). Null film işaretlemediyseniz null film bir veritabanı hataya neden olur.

İnceleme `Delete` ve `DeleteConfirmed` yöntemleri.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

HTTP GET Not `Delete` yöntemi belirtilen film silme değil, size gönderebileceği bir filmi görünümünü döndürür (`HttpPost`) silme işlemi. Bir GET'e yanıt olarak bir silme işlemi gerçekleştirme isteği (veya bir düzenleme işlemini gerçekleştirirken bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturun) bir güvenlik boşluğu açılır. Bu konu hakkında daha fazla bilgi için Stephen Walther'ın blog girişine bakın [ASP.NET MVC ipucu #46; bunlar güvenlik açıkları oluşturduğundan Sil bağlantılarını kullanmayın](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Verilerini siler yöntemi adlı `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için. İki yöntem imzaları aşağıda verilmiştir:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Ortak dil çalışma zamanı (CLR) aşırı yüklenmiş yöntemler benzersiz parametre imzası (yöntemi aynı ada ancak farklı parametre listesi) olmasını gerektirir. Bununla birlikte, burada her ikisi de aynı parametre imzasına sahip iki silme yöntemleri--bir get--ve sonrası için gerekir. (Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)

Bu sıralamak için birkaç şey yapabilirsiniz. Yöntemleri farklı adlar vermek için biridir. Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak saptayamazdınız. Eklenecek olan örnekte gördüğünüz çözümüdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Böylece içeren bir URL bu etkili bir şekilde yönlendirme sistemi eşleme gerçekleştirir */Delete/* için bir POST isteği bulabilirsiniz `DeleteConfirmed` yöntemi.

Aynı adlara ve imzaları olan yöntemleri ile ilgili bir sorun önlemek için başka bir yaygın yapay olarak kullanılmayan bir parametre eklemek için POST yöntemini imzasını değiştirmek için yoludur. Örneğin, bazı geliştiriciler bir parametre türü ekleyin `FormCollection` POST yöntemine geçirilir ve ardından yalnızca parametresini kullanmayın:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Özet

Şimdi yerel bir DB veritabanına veri depolayan tam bir ASP.NET MVC Uygulamam var. Oluşturma, okuma, güncelleştirme, silme ve filmler için arama yapın.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Sonraki Adımlar

Oluşturulan ve bir web uygulamasını test sonra sonraki adım, Internet üzerinden kullanmak için diğer kişilerin kullanımına sağlamaktır. Bunu yapmak için bir web barındırma sağlayıcısına dağıtmak zorunda. Microsoft'un sunduğu en fazla 10 web siteleri için ücretsiz bir web barındırma bir [ücretsiz Azure deneme hesabı](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). My öğreticinin sonraki izleyin ı önerin [bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulamasını Azure'a dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Tom Dykstra'nın orta düzey mükemmel bir öğreticidir [bir ASP.NET MVC uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) ve [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx) olan soru sormak için harika bir yerleştirir. İzleyin [bana](https://twitter.com/RickAndMSFT) my en yeni öğreticiler güncelleştirmeleri alabilmeniz için Twitter'da.

Geri bildirim Hoş Geldiniz.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Önceki](adding-validation.md)
