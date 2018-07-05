---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: '10. Bölüm: Gezinti ve Site tasarımı, sonuç yönelik son güncelleştirmeler | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 10. Bölüm gezinti ve S. yönelik son güncelleştirmeler kapsayan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: a5b1d02d268530d6288ed2bc502679336d06d403
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366326"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>10. Bölüm: Gezinti ve Site tasarımı, sonuç yönelik son güncelleştirmeler
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 10. Bölüm gezinti ve Site tasarımı, sonuç yönelik son güncelleştirmeler kapsar.


Ana işlevleri için sitemizi tamamladınız, ancak yine de site gezintisi, giriş sayfası ve Store Gözat sayfası eklemek için bazı özellikleri sunuyoruz.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Alışveriş sepeti Özet kısmi görünümü oluşturma

Kullanıcının alışveriş sepetine öğe sayısı tüm site genelinde kullanıma sunmak istiyoruz.

![](mvc-music-store-part-10/_static/image1.png)

Biz kolayca bu bizim Site.master için eklenen bir kısmi görünüm oluşturarak uygulayabilirsiniz.

Daha önce gösterildiği gibi ShoppingCart denetleyicisi kısmi görünüm döndüren bir CartSummary eylem yöntemini içerir:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

CartSummary kısmi görünümü oluşturmak için görünümler/ShoppingCart klasörü sağ tıklatın ve eklemek görünümü seçin. CartSummary görünüm adını ve aşağıda gösterildiği gibi "kısmi Görünüm Oluştur" onay kutusunu işaretleyin.

![](mvc-music-store-part-10/_static/image2.png)

Gerçekten Basit CartSummary kısmi görünüm - arabasında öğe sayısını gösteren ShoppingCart dizin görünümünün yalnızca bir bağlantı. CartSummary.cshtml için tam kod aşağıdaki gibidir:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Biz sitedeki Site asıl Html.RenderAction yöntemi kullanarak dahil olmak üzere herhangi bir sayfa kısmi görünüm ekleyebilirsiniz. RenderAction ("CartSummary") eylem adı ve Denetleyici adı ("ShoppingCart") olarak aşağıda belirtmenizi gerektirir.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Bu düzen siteye eklemeden önce Site.master güncelleştirmelerimizin tümünü bir kerede yapabiliyoruz şekilde Tarz menü da oluşturacağız.

## <a name="creating-the-genre-menu-partial-view"></a>Tarz menü kısmi görünümü oluşturma

Bu kullanıcılarımız mağazası aracılığıyla tüm türleri kullanılabilir bizim deposundaki listeleyen bir türe menü ekleyerek gitmek için çok daha kolay yapabiliyoruz.

![](mvc-music-store-part-10/_static/image3.png)

Biz aynı izleyeceği adımları GenreMenu kısmi görünüm oluşturabilir ve ardından her iki Site asıl ekleyebiliriz. İlk olarak, aşağıdaki GenreMenu denetleyici eylemi için StoreController ekleyin:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Bu eylem, sonraki oluşturacağız kısmi görünüm tarafından görüntülenen türleri listesini döndürür.

*Not: [ChildActionOnly] özniteliği yalnızca kısmi bir görünümden kullanılmak üzere bu eylem istediğimizi belirten Bu denetleyici eylemi ekledik. Bu öznitelik için /Store/GenreMenu göz atarak çalıştırılmasını denetleyici eylemini engeller. Bu, kısmi görünümleri için gerekli değildir ancak biz amaçladığınız gibi denetleyicisi eylemlerimiz kullanılır emin olmak istiyoruz beri iyi bir uygulama olacaktır. Biz de diğer görünümlerde eklenmiş olduğu gibi bu görünüm için Düzen kullanmamalısınız bilmeniz görünüm altyapısı sağlayan görünümü yerine PartialView iade ettiğiniz.*

GenreMenu denetleyici eylemi sağ tıklayın ve bu tarz görünüm veri sınıfı aşağıda gösterildiği gibi kullanarak türü kesin olarak GenreMenu adlı bir kısmi görünüm oluşturun.

![](mvc-music-store-part-10/_static/image4.png)

Sırasız bir listesini kullanarak aşağıdaki gibi öğeleri görüntülemek GenreMenu kısmi görünüm için Görünüm Kodu güncelleştirin.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Site, kısmi görünümleri görüntülemek için Düzen güncelleştiriliyor

Bizim kısmi görünümler Site düzenine ekleyebilirsiniz (/görünümler/paylaşılan/\_Layout.cshtml) Html.RenderAction() çağırarak. İçinde ikisini yanı sıra bazı ek biçimlendirme bunları görüntülemek için aşağıda gösterildiği gibi ekleyeceğiz:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Biz uygulamayı çalıştırdığınızda, artık sol gezinti bölmesinde tarz ve üst Sepeti özeti göreceğiz.

## <a name="update-to-the-store-browse-page"></a>Store Gözat sayfaya güncelleştirin

Store Gözat sayfanın işe yarar, ancak çok iyi görünmüyor. Biz (/Views/Store/Browse.cshtml içinde bulunur) kodu görüntüle aşağıdaki gibi güncelleştirerek daha iyi bir düzende albümleri gösterecek şekilde sayfayı güncelleştirebilirsiniz:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Burada yapıyoruz Html.ActionLink yerine Url.Action kullanabilir, böylece biz bağlantısını albüm resim eklemek için özel bir biçimlendirme uygulayabilirsiniz.

*Not: Biz bu albümleri için bir genel albümü kapak görüntülüyorsunuz. Bu bilgiler veritabanında depolanır ve Store Yöneticisi düzenlenebilir. Kendi resim eklemek Hoş Geldiniz.*

Şimdi biz için bir türe göz attığınızda, albüm resimleri içeren bir kılavuz gösterilen albümleri göreceğiz.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Üst satış albümleri göstermek için giriş sayfası güncelleştiriliyor

Satışları artırmak için giriş sayfasında albümleri satış bizim üst özellik istiyoruz. Bizim HomeController işleyen ve bazı ek grafikler, eklemek için bazı güncelleştirmeler oluşturacağız.

EntityFramework ilişkili bilebilmesi ilk olarak bir gezinti özelliği bizim albüm sınıfına ekleyeceğiz. Son birkaç satırı bizim **albüm** sınıfı artık şu şekilde görünmelidir:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Not: Bu kullanarak bir ekleme gerekir deyimini System.Collections.Generic ad alanına getirin.*

İlk olarak, bir storeDB alan ve diğer bizim denetleyicileri olduğu gibi deyimlerini MvcMusicStore.Models ekleyeceğiz. Ardından, veritabanımızdaki OrderDetails göre en çok satan albümleri bulmak için sorguları HomeController için aşağıdaki yöntemi ekleyeceğiz.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Bir denetleyici eylemi olarak kullanılabilir olmasını istemiyorsanız bu yana özel bir yöntem budur. Biz bunu HomeController kolaylık olması için de dahil, ancak iş mantığınızı ayrı hizmet sınıflarını uygun şekilde içine taşımak için önerilir.

Bu yerinde albümleri satış en çok kullanılan 5 sorgulamak ve bunları görünümüne dönmek için dizin denetleyici eylemi güncelleştirebilirsiniz.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Güncelleştirilmiş HomeController için tam kod, aşağıda gösterildiği gibi ' dir.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Son olarak, biz Model türü güncelleştiriliyor ve albüm listenin en altına ekleyerek albümleri listesini görüntüleyebilmesi bizim giriş dizini görünümünü güncelleştirme gerekir. Biz de sayfaya başlık ve bir yükseltme bölümü eklemek için bu fırsattan yararlanın.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Şimdi biz uygulamayı çalıştırdığınızda, güncelleştirilmiş giriş sayfamızı üst satış Albümler ve bizim promosyon ileti göreceğiz.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Sonuç

ASP.NET MVC kolaylaştırır olduğunu gördük veritabanı erişimi, üyelik, AJAX, Gelişmiş bir Web sitesi oluşturmak için vs. oldukça hızlı bir şekilde. Umarım Bu öğretici, kendi ASP.NET MVC uygulamaları oluşturmaya başlamak için ihtiyacınız olan araçları verdiği!


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-9.md)
