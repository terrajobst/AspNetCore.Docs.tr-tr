---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Ayrıntılar ve silme yöntemleri (C#) geliştirme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 55945eb373c79fd6ae018fe8f896dc5e6bbe7744
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871590"
---
<a name="improving-the-details-and-delete-methods-c"></a>Ayrıntılar ve silme yöntemleri (C#) artırma
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [C# sürümü](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


Öğreticinin bu bölümünde otomatik olarak oluşturulan bazı geliştirmeler hale getireceğiz `Details` ve `Delete` yöntemleri. Bu değişiklikler gerekli değildir, ancak yalnızca birkaç küçük bitle kod, uygulamanın kolayca geliştirebilirsiniz.

## <a name="improving-the-details-and-delete-methods"></a>Ayrıntılar ve silme yöntemleri artırma

Ne zaman iskele kurulmuş `Movie` denetleyicisi, ASP.NET MVC oluşturulan çalışan büyük, ancak bu yapılabilir birkaç küçük değişikliklerle daha sağlam kodu.

Açık `Movie` denetleyicisi ve değiştirme `Details` döndürerek yöntemi `HttpNotFound` film değil bulunduğunda. Aynı zamanda değiştirmeniz gerekir `Details` yöntem kendisine geçirilen kimliği için bir varsayılan değer ayarlayın. (Benzer değişiklik yapılan `Edit` yönteminde [Bölüm 6](examining-the-edit-methods-and-edit-view.md) Bu öğreticinin.) Bununla birlikte, dönüş türü değişikliği `Details` yönteminden `ViewResult` için `ActionResult`, çünkü `HttpNotFound` değil döndürme bir `ViewResult` nesnesi. Aşağıdaki örnek değiştirilmiş gösterir `Details` yöntemi.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Kod ilk veri kullanarak için arama yapmayı kolaylaştırır `Find` yöntemi. Kod doğrular yönteme oluşturduğumuz bir önemli güvenlik özelliğidir `Find` kodu şey denemeden önce yöntemi bir filmi buldu. Örneğin, bir bilgisayar korsanının siteye bağlantılardan tarafından oluşturulan URL değiştirerek hatalara `http://localhost:xxxx/Movies/Details/1` gibi bir `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir filmi temsil etmez başka bir değer). Null bir filmi denetle yapmazsanız, bu bir veritabanı hatası neden olabilir.

Benzer şekilde, değiştirme `Delete` ve `DeleteConfirmed` ID parametresi için varsayılan bir değer belirtin ve döndürülecek yöntemleri `HttpNotFound` film değil bulunduğunda. Güncelleştirilmiş `Delete` yöntemleri `Movie` denetleyicisi aşağıda gösterilmektedir.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Unutmayın `Delete` yöntemi verileri sil değil. Bir GET'e yanıt olarak bir silme işlemi isteği (veya bir düzenleme işlemi gerçekleştirilirken Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) güvenlik boşluğu açar. Bu konu hakkında daha fazla bilgi için Stephen Walther'ın blog girişine bakın [ASP.NET MVC ipucu #46 — güvenlik açıklarını oluşturduğundan bağlantılarını sil kullanmayan](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Verileri siler yöntemi adlandırılan `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için. İki yöntem imzaları aşağıda verilmiştir:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Ortak dil çalışma zamanı (CLR) benzersiz bir imza (aynı adı, farklı parametrelerin listesi) sağlamak için aşırı yüklenmiş yöntemler gerektirir. Ancak, burada, her ikisi de aynı imza gerektiren iki silme yöntemleri--biri--get ve POST için bir gerekir. (Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)

Bu sorunu anlamak sıralamak için şunları yapabilirsiniz. Farklı adlar yöntemleri vermek biridir. Ne kendisinin önceki örnekte yaptığımız olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak bağlanamayacak. Eklemek için örnekte bkz çözümdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Bir URL içeren bu etkin yönlendirme sistemi için eşleme gerçekleştirir <em>/Delete/</em>için bir POST isteği bulacaksınız `DeleteConfirmed` yöntemi.

Aynı ad ve imza sahip yöntemleri ile ilgili bir sorun önlemek için başka bir yol kullanılmamış bir parametre eklemek için POST yöntemini imza yapay olarak değiştirmektir. Örneğin, bir parametre türü bazı geliştiriciler ekleyin `FormCollection` POST yöntemine geçirilen ve ardından parametresi yalnızca kullanmayın:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Sonlandırmadan

Artık bir SQL Server Compact veritabanında veri depolayan tam bir ASP.NET MVC uygulaması sahipsiniz. Oluşturma, okuma, güncelleştirme, silme ve filmler için arama yapın.

![](improving-the-details-and-delete-methods/_static/image1.png)

Bu temel öğretici görünümlerle ilişkilendirme ve sabit kodlanmış verileri geçirmeden denetleyicileri yapmadan başlamanıza aldı. Ardından oluşturduğunuz ve bir veri modeli tasarlanmıştır. Kolay bir şekilde veri modelinden Entity Framework Code First bir veritabanı oluşturulur ve eylem yöntemleri ve görünümler temel CRUD işlemleri için ASP.NET MVC yapı iskelesi sistem otomatik olarak oluşturulur. Ardından, kullanıcıların veritabanında arama izin verecek bir arama formu de eklendi. Yeni bir sütun veri eklemek için veritabanı değişti ve oluşturmak ve bu yeni verileri görüntülemek için iki sayfa güncelleştirildi. Veri modeli öznitelikleri ile işaretleyerek doğrulama eklenen `DataAnnotations` ad alanı. Sonuçta elde edilen doğrulama istemci ve sunucu üzerinde çalışır.

Uygulamanızı dağıtmak istiyorsanız, ilk test yerel IIS 7 sunucunuzda uygulama için yararlıdır. Bu kullanabilirsiniz [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) ASP.NET uygulamaları için IIS ayarını etkinleştirmek için bağlantı. Aşağıdaki dağıtım bağlantılara bakın:

- [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/dd394698.aspx)
- [IIS etkinleştirme 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Web Uygulama projeleri dağıtımı](https://msdn.microsoft.com/library/dd394698.aspx)

I şimdi bizim orta düzey taşımanızı öneririz [bir ASP.NET MVC uygulaması için Entity Framework veri modeli oluşturma](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ve [MVC müzik deposu](../../mvc-music-store/mvc-music-store-part-1.md) keşfetmek için öğreticileri, [ASP.NET MSDN'de makaleleri](https://msdn.microsoft.com/library/gg416514(VS.98).aspx), birçok videolar ve kaynaklara göz atın ve [ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC hakkında daha fazla bilgi edinmek için! [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx) sorular sormak için harika bir yerdir.

Keyfini çıkarın!

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) ve [ @shanselman ](http://twitter.com/shanselman) Twitter'da) ve Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Önceki](adding-validation-to-the-model.md)
