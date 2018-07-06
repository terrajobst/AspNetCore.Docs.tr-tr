---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: ASP.NET MVC denetleyicisine genel bakış (C#) | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Stephen Walther için ASP.NET MVC denetleyicileri sunar. Yeni denetleyicileri oluşturun ve eylem res farklı türde döndürmek öğrenin...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: f8ff1ed19e6357a9a4e98f755a677d05ae5c2ec4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821728"
---
<a name="aspnet-mvc-controller-overview-c"></a>ASP.NET MVC denetleyicisine genel bakış (C#)
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bu öğreticide, Stephen Walther için ASP.NET MVC denetleyicileri sunar. Yeni denetleyicileri oluşturun ve farklı türde eylem sonuçlarını döndürmek öğrenin.


Bu öğretici, ASP.NET MVC denetleyicileri, denetleyici eylemlerini ve eylem sonuçlarını konu açıklar. Bu öğreticiyi tamamladıktan sonra denetleyicileri ziyaretçi bir ASP.NET MVC Web sitesi ile etkileşim şeklini denetlemek için nasıl kullanılacağını anlayacaksınız.

## <a name="understanding-controllers"></a>Denetleyicileri anlama

MVC denetleyicileri karşı bir ASP.NET MVC Web sitesine yapılan isteklerini yanıtlamadan sorumludur. Her bir tarayıcı isteğini belirli bir denetleyiciye eşlenir. Örneğin, tarayıcınızın adres çubuğuna aşağıdaki URL'yi girin düşünün:

`http://localhost/Product/Index/3`

Bu durumda, ProductController adlı bir denetleyici çağrılır. ProductController tarayıcı isteğin yanıtını oluşturmaktan sorumlu. Örneğin, denetleyici, belirli bir görünüm tarayıcıya geri döndürebilir veya denetleyici kullanıcı başka bir denetleyiciye yönlendirebilir.

1 listeleme ProductController adlı basit bir denetleyici içerir.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Listeleme 1'den görebileceğiniz gibi yalnızca bir sınıf (Visual Basic .NET veya C# sınıfı) bir denetleyicisi değil. Temel System.Web.Mvc.Controller sınıfından türetilen bir sınıf denetleyicisidir. Bir denetleyici bu temel sınıftan devraldığı için bir denetleyici birçok kullanışlı yöntem ücretsiz devralır (Bu yöntemleri birazdan ele alır).

## <a name="understanding-controller-actions"></a>Denetleyici eylemlerini anlama

Bir denetleyici denetleyici eylemleri gösterir. Bir eylem, belirli bir URL'sini tarayıcınızın adres çubuğuna girdiğinizde çağrılan denetleyicisinde bir yöntemdir. Örneğin, aşağıdaki URL için bir istekte bulunmak düşünün:

`http://localhost/Product/Index/3`

Bu durumda, İNDİS() yöntem ProductController sınıf üzerinde çağrılır. İNDİS() yöntemi, denetleyici eylem örneğidir.

Bir denetleyici eylemi bir denetleyici sınıfının genel bir yöntemi olması gerekir. Varsayılan olarak, C# yöntemler, özel yöntemlerdir. Bir denetleyici sınıfına ekleyin herhangi bir genel yöntemini bir denetleyici eylem olarak otomatik olarak kullanıma sunulduğunu unutmayın (bir denetleyici eylemi evreni içinde hiç kimse tarafından yalnızca bir tarayıcı adres çubuğuna sağ URL yazarak çağrılabilir olduğundan bu hakkında dikkatli olmanız gerekir).

Bir denetleyici eylemi tarafından karşılanması gereken bazı ek gereksinimleri vardır. Bir denetleyici eylemi kullanılan bir yöntem aşırı yüklenemez. Ayrıca, bir denetleyici eylemi bir statik yöntem olamaz. Bir denetleyici eylemi neredeyse tüm yöntemi kullanabilirsiniz.

## <a name="understanding-action-results"></a>Eylem sonuçlarını anlama

Bir denetleyici eylemi olarak adlandırılan bir şey döndürür bir *eylem sonucu*. Eylem sonucu bir denetleyici eylemi bir tarayıcı isteğine yanıt olarak döndürür ' dir.

ASP.NET MVC çerçevesi, eylem sonuçlarını da dahil olmak üzere çeşitli türlerini destekler:

1. ViewResult - temsil HTML ve biçimlendirme.
2. EmptyResult - hiçbir sonucu temsil eder.
3. RedirectResult - yeni bir URL yeniden yönlendirme temsil eder.
4. JsonResult - AJAX uygulamada kullanılabilecek bir JavaScript nesne gösterimi sonucu temsil eder.
5. JavaScriptResult - JavaScript komut dosyasını temsil eder.
6. ContentResult - metin sonucu temsil eder.
7. FileContentResult - (ikili içerikle) indirilebilir bir dosyayı temsil eder.
8. FilePathResult - (yoluyla) indirilebilir bir dosyayı temsil eder.
9. FileStreamResult - (ile bir dosya akışı) indirilebilir bir dosyayı temsil eder.

Tüm bu eylem sonuçlarını temel ActionResult sınıfından devralır.

Çoğu durumda, bir denetleyici eylemi bir ViewResult döndürür. Örneğin, 2 listeleme dizin denetleyici eylemi bir ViewResult döndürür.

**2 - Controllers\BookController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Bir eylem bir ViewResult geri döndüğünde, tarayıcıya HTML döndürülür. Listeleme 2 İNDİS() yöntemi tarayıcıya dizin adlı bir görünüm verir.

Listeleme 2 İNDİS() eylemi bir ViewResult() döndürmeyen dikkat edin. Bunun yerine, denetleyici temel sınıfın View() yöntemi çağrılır. Normalde, bir eylem sonucu doğrudan gitmez. Bunun yerine, denetleyici temel sınıf aşağıdaki yöntemlerden birini arayın:

1. Görüntüleme - ViewResult eylem sonucunu döndürür.
2. Yeniden yönlendirme - RedirectResult eylem sonucunu döndürür.
3. RedirectToAction - RedirectToRouteResult eylem sonucunu döndürür.
4. RedirectToRoute - RedirectToRouteResult eylem sonucunu döndürür.
5. JSON - JsonResult eylem sonucunu döndürür.
6. JavaScriptResult - bir JavaScriptResult döndürür.
7. İçerik - ContentResult eylem sonucunu döndürür.
8. Dosya - yönteme bir FileContentResult, FilePathResult veya FileStreamResult parametreleri bağlı olarak döndürür.

Bu nedenle, tarayıcıya bir görünüme dönmek istiyorsanız, View() yöntemi çağırın. Bir denetleyici eylem kullanıcıya yönlendirmek istiyorsanız, RedirectToAction() yöntemi çağırın. Örneğin, Details() eylem listeleme 3'te bir görünüm görüntüler veya kullanıcı ID parametresine bir değer olup olmadığına bağlı olarak İNDİS() eyleme yeniden yönlendirir.

**3 - CustomerController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

ContentResult eylem sonucu özeldir. Eylem sonucu düz metin olarak döndürülecek ContentResult eylem sonucunu kullanabilirsiniz. Örneğin, 4 listeleme İNDİS() yöntemi HTML değil de, düz metin olarak bir ileti döndürür.

**4 - Controllers\StatusController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

StatusController.Index() eylem çağrıldığında, görünümü döndürülmez. Bunun yerine, ham metni "Hello World!" tarayıcıya döndürülür.

Ardından bir denetleyici eylemi eylem sonucunu değil - Örneğin, bir sonuç bir tarih veya bir tamsayı - döndürürse, sonuç bir ContentResult içinde otomatik olarak paketlenir. Örneğin, 5 listeleme WorkController İNDİS() eylemi çağrıldığında tarih bir ContentResult otomatik olarak döndürülür.

**5 - WorkController.cs listeleme**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Listeleme 5 İNDİS() eylemi, bir DateTime nesnesini döndürür. ASP.NET MVC çerçevesi, DateTime nesnesi bir dizeye dönüştürür ve tarih saat değeri bir ContentResult otomatik olarak sarmalar. Tarayıcı, tarih ve saat düz metin olarak alır.

## <a name="summary"></a>Özet

Bu öğreticide, ASP.NET MVC denetleyicileri, denetleyici eylemlerini ve denetleyici eylem sonuçlarını kavramlara tanıtmak için oluştu. Bu bölümde, ASP.NET MVC projesinde yeni denetleyicileri ekleme öğrendiniz. Ardından, etki alanı denetleyicisinin nasıl genel yöntemleri öğrendiğiniz evrenimize Hoş denetleyici eylemleri sunulur. Son olarak, farklı türde bir denetleyici eylemi döndürülen eylem sonuçlarını ele almıştık. Özellikle, bir ViewResult RedirectToActionResult ve ContentResult bir denetleyici eylemi dönüş nasıl ele almıştık.

> [!div class="step-by-step"]
> [Önceki](creating-an-action-vb.md)
> [İleri](creating-custom-routes-cs.md)
