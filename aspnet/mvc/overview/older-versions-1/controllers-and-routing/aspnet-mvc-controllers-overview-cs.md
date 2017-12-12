---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: "ASP.NET MVC denetleyicisi genel bakış (C#) | Microsoft Docs"
author: StephenWalther
description: "Bu öğreticide, Stephen Walther ASP.NET MVC denetleyicisi sunar. Yeni denetleyicileri oluşturun ve eylem res farklı türlerde iade hakkında bilgi edinin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9e4ca745fa068b1813e01b131d53a0199cc47d5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-c"></a>ASP.NET MVC denetleyicisi genel bakış (C#)
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bu öğreticide, Stephen Walther ASP.NET MVC denetleyicisi sunar. Yeni denetleyicileri oluşturun ve dönüş eylem sonuçlarını farklı türleri hakkında bilgi edinin.


Bu öğreticide ASP.NET MVC denetleyicileri, denetleyici eylemleri ve eylem sonuçlarını konu araştırır. Bu öğreticiyi tamamladıktan sonra denetleyicileri ziyaretçisi bir ASP.NET MVC Web sitesi ile etkileşime giren şeklini denetlemek için kullanılan nasıl anlayacaksınız.

## <a name="understanding-controllers"></a>Denetleyicileri anlama

MVC denetleyicileri karşı bir ASP.NET MVC Web isteklerini yanıtlamak için sorumludur. Her tarayıcı isteğini belirli bir denetleyiciye eşlenir. Örneğin, tarayıcınızın adres çubuğuna aşağıdaki URL'yi girin düşünün:

`http://localhost/Product/Index/3`

Bu durumda, ProductController adlı bir denetleyicisi çağrılır. ProductController tarayıcı isteğinin yanıtı oluşturmak için sorumludur. Örneğin, denetleyici belirli bir görünüm tarayıcıya geri döndürebilir veya denetleyicisi kullanıcı başka bir denetleyiciye yönlendirebilir.

Kod 1 ProductController adlı basit bir denetleyicisi içerir.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Listeleme 1'den görebileceğiniz gibi bir sınıf yalnızca (Visual Basic .NET veya C# sınıfı) denetleyicisidir. Temel System.Web.Mvc.Controller sınıfından türeyen bir sınıf denetleyicisidir. Bir denetleyici bu taban sınıfından devralıyor çünkü bir denetleyici çeşitli kullanışlı yöntemler ücretsiz devralır (biz bu yöntemler bir dakika içinde ele alınmıştır).

## <a name="understanding-controller-actions"></a>Denetleyici eylemlerini anlama

Bir denetleyici denetleyici eylemleri gösterir. Bir eylem, belirli bir URL tarayıcınızın adres çubuğuna girdiğinizde çağrılan bir denetleyicisinde bir yöntemdir. Örneğin, aşağıdaki URL için bir istek olun düşünün:

`http://localhost/Product/Index/3`

Bu durumda, İNDİS() yöntemi ProductController sınıf üzerinde çağrılır. İNDİS() yöntemi, denetleyici eylem örneğidir.

Bir denetleyici eylemi bir denetleyici sınıfının genel yöntem olması gerekir. C#, varsayılan olarak, özel yöntemler yöntemleridir. Bir denetleyici sınıfına ekleyin herhangi bir genel yöntemini bir denetleyici eylemi otomatik olarak sunulur unutmayın (bir denetleyici eylemi universe herhangi biri tarafından yalnızca bir tarayıcınızın adres çubuğuna sağ URL yazarak çağrılabilir olduğundan, bu konuda dikkatli olmalıdır).

Bir denetleyici eylemi tarafından karşılanması gereken bazı ek gereksinimleri vardır. Bir denetleyici eylemi kullanılan bir yöntem aşırı yüklenemez. Ayrıca, bir denetleyici eylemi bir statik yöntem olamaz. Başka bir denetleyici eylemi neredeyse tüm yöntemini kullanabilirsiniz.

## <a name="understanding-action-results"></a>Eylem sonuçlarını anlama

Bir denetleyici eylemi olarak adlandırılan bir şey döndürüyor bir *eylem sonucu*. Bir eylem sonucu bir denetleyici eylemi bir tarayıcı isteğine yanıt olarak döndürür ' dir.

ASP.NET MVC çerçevesi, eylem sonuçlarını dahil olmak üzere birkaç türlerini destekler:

1. ViewResult - temsil HTML ve biçimlendirme.
2. EmptyResult - sonuç temsil eder.
3. RedirectResult - yeni bir URL yeniden yönlendirme temsil eder.
4. JsonResult - AJAX uygulamada kullanılan bir JavaScript nesne gösterimi sonucunu temsil eder.
5. JavaScriptResult - JavaScript komut temsil eder.
6. ContentResult - metin sonucunu temsil eder.
7. FileContentResult - (ikili içerikle) indirilebilir bir dosyayı temsil eder.
8. FilePathResult - (yoluyla) indirilebilir bir dosyayı temsil eder.
9. FileStreamResult - (ile bir dosya akışı) indirilebilir bir dosyayı temsil eder.

Tüm bu eylem sonuçlarını temel ActionResult sınıfından devralınır.

Çoğu durumda, bir denetleyici eylemi bir ViewResult döndürür. Örneğin, dizin denetleyici eylemi listeleme 2'deki bir ViewResult döndürür.

**2 - Controllers\BookController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Bir eylem bir ViewResult geri döndüğünde, HTML tarayıcıya döndürülür. 2. listeleme İNDİS() yöntemi tarayıcıya dizin adlı bir görünümü döndürür.

Listeleme 2 İNDİS() eylemde bir ViewResult() döndürmüyor dikkat edin. Bunun yerine, denetleyici temel sınıfın View() yöntemi çağrılır. Normalde, bir eylem sonucu doğrudan döndürmeyin. Bunun yerine, denetleyici temel sınıfın aşağıdaki yöntemlerden birini arayın:

1. Görüntüleme - ViewResult eylem sonucunu döndürür.
2. Yeniden yönlendirme - RedirectResult eylem sonucunu döndürür.
3. RedirectToAction - RedirectToRouteResult eylem sonucunu döndürür.
4. RedirectToRoute - RedirectToRouteResult eylem sonucunu döndürür.
5. JSON - JsonResult eylem sonucunu döndürür.
6. JavaScriptResult - bir JavaScriptResult döndürür.
7. İçerik - ContentResult eylem sonucunu döndürür.
8. Dosya - FileContentResult, FilePathResult veya FileStreamResult parametreleri bağlı olarak yönteme geçirilen döndürür.

Bu nedenle, tarayıcıya bir görünüme dönmek istiyorsanız, View() yöntemini çağırın. Bir denetleyici eylem kullanıcıya yönlendirmek istiyorsanız, RedirectToAction() yöntemini çağırın. Örneğin, listeleme 3 Details() eylemde görüntüleyen bir görünüm ya da kullanıcı ID parametresi bir değer olup bağlı olarak İNDİS() eyleme yeniden yönlendirir.

**3 - CustomerController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

ContentResult eylem sonucu özeldir. Eylem sonucu düz metin olarak döndürmek için ContentResult eylem sonucunu kullanabilirsiniz. Örneğin, listeleme 4 İNDİS() yönteminde bir ileti HTML değil de, düz metin olarak döndürür.

**4 - Controllers\StatusController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

StatusController.Index() eylem çağrıldığında, görünümü döndürülmez. Bunun yerine, "Hello World!" ham metni tarayıcıya döndürülür.

Ardından bir denetleyici eylemi eylem sonucunu değil - Örneğin, bir sonuç bir tarih veya tamsayı - döndürürse, sonuç bir ContentResult otomatik olarak paketlenir. Örneğin, 5 listeleme WorkController İNDİS() eylemini çağrıldığında, tarih ContentResult otomatik olarak döndürülür.

**5 - WorkController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Listeleme 5 İNDİS() eylemde bir DateTime nesnesi döndürür. ASP.NET MVC çerçevesi DateTime nesnesi bir dizeye dönüştürür ve DateTime değeri bir ContentResult otomatik olarak sarmalar. Tarayıcı, tarih ve saat düz metin olarak alır.

## <a name="summary"></a>Özet

ASP.NET MVC denetleyicileri, denetleyici eylemleri ve denetleyici eylem sonuçlarını kavramlarını tanıtmak için bu öğreticinin amacı oluştu. Bu bölümde, ASP.NET MVC projesinde yeni denetleyicileri ekleme öğrendiniz. Ardından, bir denetleyicinin nasıl ortak yöntemlerini öğrenilen universe denetleyici eylemleri sunulur. Son olarak, farklı türdeki bir denetleyici eylemin getirdiği eylem sonuçlarını ele. Özellikle, bir ViewResult, RedirectToActionResult ve ContentResult denetleyicisi eylemden döndürmek nasıl ele.

>[!div class="step-by-step"]
[Önceki](creating-an-action-vb.md)
[sonraki](creating-custom-routes-cs.md)
