---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Details ve Delete metotlarını (VB) geliştirme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: bc13aeacfe67031151faacc2bd78891a58c8d6a4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754487"
---
<a name="improving-the-details-and-delete-methods-vb"></a>Details ve Delete metotlarını (VB) geliştirme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/improving-the-details-and-delete-methods.md) Bu öğreticinin.


Öğreticinin bu bölümünde bazı iyileştirmeler otomatik olarak oluşturulan yapacaksınız `Details` ve `Delete` yöntemleri. Bu değişiklikler gerekli değildir, ancak yalnızca birkaç küçük bit kod ile kolayca uygulama geliştirebilirsiniz.

## <a name="improving-the-details-and-delete-methods"></a>Details ve Delete metotlarını geliştirme

Ne zaman iskele kurulmuş `Movie` denetleyici, ASP.NET MVC harika çalışan, ancak bu yapılabilir ile yalnızca birkaç küçük değişiklikler daha sağlam kod oluşturulur.

Açık `Movie` denetleyicisi ve değiştirme `Details` döndürerek yöntemi `HttpNotFound` değil bir filmi bulunduğunda. Ayrıca değiştirmelisiniz `Details` ona iletilen kimliği için varsayılan değer ayarlamak için yöntemi. (Benzer değişiklik yapılan `Edit` yönteminde [6. bölüm](examining-the-edit-methods-and-edit-view.md) Bu öğreticinin.) Ancak, dönüş türünü değiştirmek `Details` yönteminden `ViewResult` için `ActionResult`, çünkü `HttpNotFound` yöntemi döndürmüyor bir `ViewResult` nesne. Aşağıdaki örnek, değiştirilmiş gösterir `Details` yöntemi.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Kod ilk kullanarak verileri için arama yapmayı kolaylaştırır `Find` yöntemi. Kod doğrular yönteme oluşturduğumuz bir önemli güvenlik özelliği olan `Find` yöntemi kod şey denemeden önce bir filmi buldu. Örneğin, bir bilgisayar korsanının hataları siteye bağlantılardan tarafından oluşturulan URL değiştirerek neden olabilirdi `http://localhost:xxxx/Movies/Details/1` gibi bir şey `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir film temsil etmez başka bir değer). Null bir filmi denetle Aksi takdirde, bu bir veritabanı hatası sonuçlanabilir.

Benzer şekilde, değiştirme `Delete` ve `DeleteConfirmed` ID parametresi için varsayılan bir değer belirtin ve döndürmek için yöntemleri `HttpNotFound` değil bir filmi bulunduğunda. Güncelleştirilmiş `Delete` yöntemleri `Movie` denetleyicisi aşağıda gösterilmektedir.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Unutmayın `Delete` yöntemi verileri silme değil. Bir GET'e yanıt olarak bir silme işlemi gerçekleştirme isteği (veya bir düzenleme işlemini gerçekleştirirken bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturun) bir güvenlik boşluğu açılır. Bu konu hakkında daha fazla bilgi için Stephen Walther'ın blog girişine bakın [ASP.NET MVC ipucu #46; bunlar güvenlik açıkları oluşturduğundan Sil bağlantılarını kullanmayın](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Verilerini siler yöntemi adlı `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için. İki yöntem imzaları aşağıda verilmiştir:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Ortak dil çalışma zamanı (CLR) benzersiz bir imza (aynı ada, parametrelerin farklı listesi) sağlamak için aşırı yüklenmiş yöntemler gerektirir. Bununla birlikte, burada her ikisi de aynı imzaya gerektiren iki silme yöntemleri--bir get--ve sonrası için gerekir. (Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)

Bu sıralamak için birkaç şey yapabilirsiniz. Yöntemleri farklı adlar vermek için biridir. Ne yaptığı önceki örnekte yaptığımız olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak saptayamazdınız. Eklenecek olan örnekte gördüğünüz çözümüdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Böylece içeren bir URL bu etkili bir şekilde yönlendirme sistemi eşleme gerçekleştirir <em>/Delete/</em>için bir POST isteği bulabilirsiniz `DeleteConfirmed` yöntemi.

Aynı adlara ve imzaları olan yöntemleri ile ilgili bir sorun önlemek için başka bir yolu kullanılmayan bir parametre içerecek şekilde POST metodun imzası sınırlarına değiştirmektir. Örneğin, bazı geliştiriciler bir parametre türü ekleyin `FormCollection` POST yöntemine geçirilir ve ardından yalnızca parametresini kullanmayın:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Sonlandırmadan

Artık bir SQL Server Compact veritabanına veri depolayan tam bir ASP.NET MVC Uygulamam var. Oluşturma, okuma, güncelleştirme, silme ve filmler için arama yapın.

![](improving-the-details-and-delete-methods/_static/image1.png)

Bu temel bir öğretici videodan bunları görünümlerle ilişkilendirme ve geçici olarak kodlanmış veri geçirme denetleyicileri yapmadan başlamanıza aldı. Ardından oluşturduğunuz ve bir veri modelini geliştirmiştir. Hareket halinde veri modelinden Entity Framework Code First bir veritabanı oluşturulur ve ASP.NET MVC yapı iskelesi sistem temel CRUD işlemleri için görünümleri ve eylem yöntemlerine otomatik olarak oluşturulur. Ardından, veritabanında arama kullanıcıların bir arama formu de eklendi. Yeni bir veri sütununu dahil etmek için veritabanı değiştirildi ve ardından oluşturmak ve bu yeni verileri görüntülemek için iki sayfa güncelleştirilir. Veri modeli öznitelikleri ile işaretleyerek doğrulama eklediğiniz `DataAnnotations` ad alanı. Elde edilen doğrulama istemci ve sunucu üzerinde çalışır.

Uygulamanızı dağıtmak istiyorsanız, ilk testi yerel IIS 7 sunucunuzda uygulama için yararlıdır. Bu [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) ASP.NET uygulamaları için IIS ayarını etkinleştirmek için bağlantı. Aşağıdaki dağıtım bağlantılara bakın:

- [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/dd394698.aspx)
- [Etkinleştirme IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Web Uygulama projeleri dağıtımı](https://msdn.microsoft.com/library/dd394698.aspx)

Bizim orta düzey geçmek için artık öneriyoruz [bir ASP.NET MVC uygulaması için bir Entity Framework veri modeli oluşturma](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ve [MVC müzik Store](../../mvc-music-store/mvc-music-store-part-1.md) keşfetmek için öğreticiler, [ASP.NET MSDN makalelerini](https://msdn.microsoft.com/library/gg416514(VS.98).aspx), birçok videoları ve kaynaklara göz atın ve [ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC hakkında daha fazla bilgi edinmek için! [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx) sorular sormak için harika bir yerdir.

Keyfini çıkarın!

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) ve [ @shanselman ](http://twitter.com/shanselman) Twitter'da) ve Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Önceki](adding-validation-to-the-model.md)
