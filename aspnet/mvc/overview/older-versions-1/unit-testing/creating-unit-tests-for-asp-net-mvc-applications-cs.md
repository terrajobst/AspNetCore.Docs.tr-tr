---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: ASP.NET MVC uygulamaları için (C#) birim testleri oluşturma | Microsoft Docs
author: StephenWalther
description: Denetleyici eylemleri için birim testleri oluşturmayı öğrenin. Bu öğreticide, Stephen Walther bir denetleyici eylemi bir parti döndürüp döndürmediğini test gerçekleştirerek...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 08de8a57860886a8f633cacbaae1d63fe08a5a02
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754654"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>ASP.NET MVC uygulamaları için (C#) birim testleri oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

[PDF'yi indirin](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> Denetleyici eylemleri için birim testleri oluşturmayı öğrenin. Bu öğreticide, Stephen Walther bir denetleyici eylemi belirli bir görünüm verir, belirli bir veri kümesini döndürür ya da farklı türde bir eylem sonucunu döndürür test etmeyi göstereceğiz.


Bu öğreticinin amacı nasıl denetleyicileri için birim testleri, ASP.NET MVC uygulamaları yazabileceğiniz göstermektir. Üç farklı türde birim testleri oluşturmak nasıl ele alır. Denetleyici eylem tarafından döndürülen görünümü test etme, denetleyici eylem tarafından döndürülen görünüm verileri test etme ve bir denetleyici eylemi için ikinci bir denetleyici eylemi yönlendiren olup olmadığını test etme bilgi edinin.

## <a name="creating-the-controller-under-test"></a>Test denetleyicisi oluşturma

Test etmek istediğiniz denetleyiciye oluşturarak başlayalım. Adlı denetleyicisi `ProductController`, listeleme 1'de yer alır.

**Kod 1 – `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

`ProductController` Adlı iki eylem yöntemleri içeren `Index()` ve `Details()`. Her iki eylem yöntemleri bir görünüm döndürür. Dikkat `Details()` Eylem Kimliği adlı bir parametre kabul eder

## <a name="testing-the-view-returned-by-a-controller"></a>Bir denetleyici tarafından döndürülen görünümü test etme

Test etmek istediğimiz Imagine olup olmadığını `ProductController` sağ döndürür. Emin olmak istiyoruz olduğunda `ProductController.Details()` eylem çağrılır, ayrıntı görünümü döndürülür. Listeleme 2'deki test sınıfı tarafından döndürülen görünümü test etme için bir birim testini içeren `ProductController.Details()` eylem.

**Kod 2 – `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

Adlı bir test yöntemi listeleme 2 sınıfında içerir `TestDetailsView()`. Bu yöntem, üç satır kod içerir. Kodun ilk satırını yeni bir örneğini oluşturur `ProductController` sınıfı. İkinci kod satırını denetleyicinin çağırır `Details()` eylem yöntemi. Son olarak, son satırının olup olmadığını görünümü tarafından döndürülen kodu denetimleri `Details()` Ayrıntılar görünümünü bir eylemdir.

`ViewResult.ViewName` Özelliği, denetleyici tarafından döndürülen görünümün adını temsil eder. Bu özelliği test etme hakkında bir büyük uyarı. Bir denetleyici görünüm döndürebilir, iki yolu vardır. Bir denetleyici, böyle bir görünüm açıkça döndürebilirsiniz:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

Alternatif olarak, görünümün adını böyle denetleyici eylemini addan:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

Bu denetleyici eylem ayrıca adlı bir görünüm verir `Details`. Ancak, görünümün adını eylem adı algılanır. Ardından Görünüm adını test etmek isterseniz, denetleyici eylem Görünüm adı açıkça döndürmelidir.

Her iki tuş bileşimini girerek listeleme 2'de birim testini çalıştırabilirsiniz **Ctrl-R, A** veya tıklayarak **Çözümdeki tüm Testleri Çalıştır** (bkz. Şekil 1) düğmesi. Test başarılı olursa, Şekil 2'deki Test Sonuçları penceresinde görürsünüz.


[![Çözümdeki tüm Testleri Çalıştır](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**Şekil 01**: Çözümdeki tüm testleri çalıştır ([tam boyutlu görüntüyü görmek için tıklatın](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![Başarılı!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**Şekil 02**: başarılı! ([Tam boyutlu görüntüyü görmek için tıklatın](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Görünüm verilerini test denetleyicisi tarafından döndürülen

MVC denetleyicisi adlı kullanarak veri görünümüne geçirir *`View Data`*. Örneğin, çağırdığınızda, belirli bir ürünün ayrıntılarını görüntülemek istediğiniz Imagine `ProductController Details()` eylem. Örneği bu durumda, oluşturabileceğiniz bir `Product` sınıfı (modelinizde tanımlı) ve örneğine geçme `Details` yararlanarak görünümü `View Data`.

Değiştirilmiş `ProductController` listeleme 3'te güncelleştirilmiş içerir `Details()` bir ürün döndüren eylem.

**Kod 3 – `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

İlk olarak, `Details()` eylem yeni bir örneğini oluşturur `Product` bir dizüstü bilgisayar nesnesini temsil eden sınıf. Ardından, örneğini `Product` sınıfı, ikinci parametre olarak geçirilir `View()` yöntemi.

Birim testleri yazabileceğiniz beklenen verileri olup olmadığını sınamak için görünümdeki veriler içeriyor. Çağırdığınızda, bir dizüstü bilgisayar temsil eden bir ürün döndürülür olup olmadığını listeleyen 4 testlerinde birim testi `ProductController Details()` eylem yöntemi.

**4 listeleme – `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

Listeleme, 4'te `TestDetailsView()` yöntemi çağırarak döndürülen görünüm verileri testleri `Details()` yöntemi. `ViewData` Üzerinde bir özelliği olarak sunulan `ViewResult` çağırarak döndürülen `Details()` yöntemi. `ViewData.Model` Görünüme iletilen ürün özelliği içerir. Test, yalnızca görünüm verileri ürün dizüstü bilgisayar adına sahip olduğunu doğrular.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Bir denetleyici tarafından döndürülen eylem sonucu test etme

Daha karmaşık bir denetleyici eylemi, denetleyici eylem için geçirilen parametrelerin eylem sonuçlarını değerlere bağlı olarak farklı türde döndürebilir. Bir denetleyici eylemi çeşitli türleri dahil olmak üzere, eylem sonuçlarını döndürebilir bir `ViewResult`, `RedirectToRouteResult`, veya `JsonResult`.

Örneğin, değiştirilen `Details()` eylem listeleme 5 döndürür `Details` geçerli bir ürün kimliği eyleme başarıyla sonuçlandıktan sonra görüntüleyin. 1--seçeneğini yönlendirilirsiniz daha az bir geçersiz ürün kimliği--bir kimlik değeri geçirirseniz `Index()` eylem.

**5 listeleme – `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

Davranışını sınayabilirsiniz `Details()` listeleme 6'daki birim testi ile eylem. Listeleme 6'daki birim testi için yönlendirilirsiniz doğrular `Index` görüntülemek için -1 değerine sahip bir kimliği geçirildiğinde `Details()` yöntemi.

**6 listeleme – `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

Çağırdığınızda `RedirectToAction()` denetleyici eylemi bir denetleyici eylem yöntemine döndürür bir `RedirectToRouteResult`. Test denetimleri olmadığını `RedirectToRouteResult` kullanıcı adında bir denetleyici eylemi yönlendireceği `Index`.

## <a name="summary"></a>Özet

Bu öğreticide, MVC denetleyici eylemleri için birim testleri oluşturma öğrendiniz. İlk olarak, sağ denetleyici eylem tarafından döndürülen olup olmadığını doğrulamak hakkında bilgi edindiniz. Nasıl kullanacağınızı öğrendiniz `ViewResult.ViewName` özelliği bir görünümün adını doğrulayın.

Ardından, içeriği nasıl test incelenirken `View Data`. Doğru ürün içinde döndürülen olup olmadığını denetlemek öğrendiniz `View Data` bir denetleyici eylemi çağrıldıktan sonra.

Son olarak, eylem sonuçlarını farklı türde bir denetleyici eylemi döndürülen olup olmadığını nasıl sınayabilirsiniz ele almıştık. Bir denetleyici döndürüp döndürmediğini test öğrendiniz bir `ViewResult` veya `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Next](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
