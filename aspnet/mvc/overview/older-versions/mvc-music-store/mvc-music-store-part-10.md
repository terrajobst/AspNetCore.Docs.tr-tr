---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Bölüm 10: Gezinti ve Site tasarımı, sonuç son güncelleştirmeleri | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 10 gezinti ve S. son güncelleştirmeleri kapsayan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Bölüm 10: Gezinti ve Site tasarımı, sonuç son güncelleştirmeleri
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 10 gezinti ve Site tasarımı, sonuç son güncelleştirmeleri kapsar.


Ana işlevleri için sitemizi tamamladınız ancak biz site gezintisi, giriş sayfası ve deposu Gözat sayfası eklemek için bazı özellikler devam ediyor.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Alışveriş sepeti Özet kısmi görünümü oluşturma

Kullanıcının alışveriş sepeti öğelerin sayısı tüm site genelinde kullanıma sunmak istiyoruz.

![](mvc-music-store-part-10/_static/image1.png)

Biz kolayca bu bizim Site.master eklenen bir kısmi görünüm oluşturarak uygulayabilirsiniz.

Daha önce gösterildiği gibi ShoppingCart denetleyicisi kısmi görünüm döndüren bir CartSummary eylem yöntemini içerir:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

CartSummary kısmi görünüm oluşturmak için görünümler/ShoppingCart klasörü sağ tıklatın ve eklemek görünümü seçin. CartSummary görünüm adını ve aşağıda gösterildiği gibi "kısmi Görünüm Oluştur" onay kutusunu işaretleyin.

![](mvc-music-store-part-10/_static/image2.png)

CartSummary kısmi görünüm gerçekten basittir - yalnızca sepetinizde öğe sayısını gösteren ShoppingCart dizin görünümünün bir bağlantı. CartSummary.cshtml için tam kod aşağıdaki gibidir:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Sitedeki Site ana Html.RenderAction yöntemini kullanarak dahil olmak üzere herhangi bir sayfayı biz kısmi görünüm dahil edebilirsiniz. RenderAction bize ("CartSummary") eylem adı ve Denetleyici adı ("alışveriş sepeti") olarak aşağıda belirtmenizi gerektirir.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Bu sitenin Düzen eklemeden önce biz bizim Site.master güncelleştirmeleri tümünün aynı anda olabilmeniz Tarz menü da oluşturacağız.

## <a name="creating-the-genre-menu-partial-view"></a>Tarz menü kısmi görünümü oluşturma

Biz, çok kullanıcılarımızın tüm türler kullanılabilir bizim deposundaki listeleyen bir tarzını menü ekleyerek mağazayla gitmek için kolaylaştırabilir.

![](mvc-music-store-part-10/_static/image3.png)

Biz aynı izleyeceği adımları GenreMenu kısmi görünüm oluşturabilir ve ardından her ikisinin de Site asıl ekleyebiliriz. İlk olarak, aşağıdaki GenreMenu denetleyici eylemi StoreController ekleyin:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Bu eylem, sonraki oluşturacağız kısmi görünüm tarafından görüntülenen türler listesini döndürür.

*Not: Yalnızca kısmi bir görünümden kullanılacak Bu eylem istediğimizi belirten Bu denetleyici eylemi için [ChildActionOnly] özniteliği ekledik. Bu öznitelik için /Store/GenreMenu göz atarak çalıştırılmasını denetleyici eylemi engeller. Bu kısmi görünümler için gerekli değildir, ancak bizim denetleyici eylemleri biz düşündüğünüz olarak kullanılan emin olmak istiyoruz beri iyi bir uygulama olur. Biz de diğer görünümlerde eklenmiş olduğu gibi bu görünüm için Düzen kullanmamanız bilmeniz görünüm altyapısı sağlar görünümü yerine PartialView iade ettiğiniz.*

GenreMenu denetleyici eylemini sağ tıklayın ve aşağıda gösterildiği gibi Tarz görünüm veri sınıfını kullanarak kesin türü belirtilmiş GenreMenu adlı kısmi bir görünümü oluşturun.

![](mvc-music-store-part-10/_static/image4.png)

Sırasız bir listesini kullanarak öğeleri görüntülemek GenreMenu kısmi görünüm için Görünüm kodunu güncelleştirin.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Site, kısmi görünümleri görüntülemek için düzeni güncelleştiriliyor

Bizim kısmi görünümler Site düzenine ekleyebilirsiniz (/görünümler/paylaşılan/\_Layout.cshtml) Html.RenderAction() çağırarak. Bunların her iki yanı sıra bunları görüntülemek için bazı ek biçimlendirme aşağıda gösterildiği gibi ekleyeceğiz:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Biz uygulamayı çalıştırdığınızda, artık sol gezinti bölmesinde bir tarzını ve üst Sepeti özeti göreceğiz.

## <a name="update-to-the-store-browse-page"></a>Mağaza Gözat sayfasına güncelleştir

Mağaza Gözat sayfa işlevseldir, ancak çok iyi görünmüyor. Biz (/Views/Store/Browse.cshtml içinde bulunur) görünüm kodu gibi güncelleştirerek daha iyi bir düzende albümleri gösterecek şekilde sayfayı güncelleştirebilirsiniz:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Burada yapıyoruz Html.ActionLink yerine Url.Action biz bağlantısını albüm resim eklemek için özel biçimlendirme uygulayabilmeniz için aşağıdakileri kullanın.

*Not: Biz bu albümleri için bir genel albüm kapak görüntülüyorsunuz. Bu bilgileri veritabanında depolanır ve mağaza yöneticisi düzenlenebilir. Kendi resim eklemek Hoş Geldiniz.*

Şimdi biz bir tarzını göz attığınızda, albüm resimle kılavuzda gösterilen albümleri göreceğiz.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Üst satış albümleri göstermek için giriş sayfası güncelleştiriliyor

Satış artırmak için giriş sayfasındaki albümleri satış bizim üst özellik istiyoruz. Bazı güncelleştirmeler işleyen ve bazı ek grafiklerde eklemek için bizim HomeController için vermiyoruz.

Böylece ilişkili EntityFramework bilir ilk olarak, bir gezinti özelliği bizim albüm sınıfına ekleyeceğiz. Son birkaç satırlık bizim **albüm** sınıfı şimdi şöyle görünmelidir:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Not: Bu kullanarak bir ekleme gerektirir System.Collections.Generic ad alanında getirmek için deyimi.*

İlk olarak, bir storeDB alan ve diğer bizim denetleyicileri olduğu gibi deyimleri MvcMusicStore.Models ekleyeceğiz. Ardından, üst satış albümleri Sipariş Ayrıntıları göre bulmak için Veritabanımıza sorgular HomeController için aşağıdaki yöntemi ekleyeceğiz.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Bir denetleyici eylemi olarak kullanılabilir olmasını istemiyorsanız bu yana özel bir yöntem budur. Biz bunu basitleştirmek için HomeController dahil, ancak ayrı bir hizmet sınıfları uygun olarak iş mantığınızı taşınıp önerilir.

Yerinde albümleri satış üst 5 sorgulamak ve görünümüne dönmek için dizin denetleyici eylemi güncelleştirebilirsiniz.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Tam güncelleştirilmiş HomeController aşağıda gösterildiği gibi kodudur.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Son olarak, biz Model türü güncelleştirme ve albüm listenin altına ekleyerek albümleri listesini görüntüleyebilmesi bizim giriş dizini görünümünü güncelleştirme gerekir. Biz de sayfaya başlık ve bir yükseltme bölümü eklemek için bu fırsatı kullanacaksınız.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Şimdi biz uygulamayı çalıştırdığınızda, güncelleştirilmiş giriş sayfamızı üst satış Albümler ve bizim promosyon iletisi göreceğiz.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Sonuç

ASP.NET MVC kolaylaştırır olmadığını gördük veritabanı erişimiyle, üyelik, AJAX, Gelişmiş bir Web sitesi oluşturmak için vb. oldukça hızlı bir şekilde. Umarız Bu öğreticiyi kendi ASP.NET MVC uygulamaları oluşturmaya başlamak için ihtiyacınız olan araçları verdiği!


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-9.md)
