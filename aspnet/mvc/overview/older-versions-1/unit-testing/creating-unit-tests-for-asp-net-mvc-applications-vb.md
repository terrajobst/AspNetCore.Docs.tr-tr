---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: ASP.NET MVC uygulamaları (VB) için birim testleri oluşturma | Microsoft Docs
author: StephenWalther
description: Denetleyici eylemleri için birim testleri oluşturmayı öğrenin. Bu öğreticide, bir denetleyici eylemi bir parti döndürüp döndürmediğini test etme Stephen Walther gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>ASP.NET MVC uygulamaları (VB) için birim testleri oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

[PDF indirin](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Denetleyici eylemleri için birim testleri oluşturmayı öğrenin. Bu öğreticide, bir denetleyici eylemi belirli bir görünüm verir, belirli bir veri kümesi döndürür veya farklı türde bir eylem sonucunu döndürür olup olmadığını test etme Stephen Walther gösterir.


Bu öğreticinin amacı nasıl, denetleyicileri için birim testleri, ASP.NET MVC uygulamaları yazabilir göstermektir. Biz, üç farklı türde birim testleri oluşturmak nasıl ele almaktadır. Bir denetleyici eylemi tarafından döndürülen görünümü test etme, görünümü bir denetleyici eylemi tarafından döndürülen verileri test etme ve ikinci bir denetleyici eylemi yönlendiren bir denetleyici eylemi olup olmadığını test etme öğrenin.

## <a name="creating-the-controller-under-test"></a>Test denetleyicisi oluşturma

Test düşündüğünüz denetleyicisi oluşturarak başlayalım. Adlı denetleyicisi `ProductController`, listeleme 1'de yer alır.

**Kod 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

`ProductController` Adlı iki eylem yöntemleri içeren `Index()` ve `Details()`. Her iki eylem yöntemleri bir görünümü döndürün. Dikkat `Details()` Eylem Kimliği adlı bir parametre kabul eder

## <a name="testing-the-view-returned-by-a-controller"></a>Bir denetleyici tarafından döndürülen görünümü test etme

Test etmek istiyoruz düşünün desteklemediğini `ProductController` sağ görünümdeki döndürür. Emin olmak istiyoruz olduğunda `ProductController.Details()` eylem çağrıldığında, ayrıntı görünümü döndürülür. 2. listeleme test sınıfı tarafından döndürülen görünümü test etme için birim testi içerir `ProductController.Details()` eylem.

**Kod 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

Adlı bir test yöntemi listeleme 2 sınıfında içerir `TestDetailsView()`. Bu yöntem üç satır kod içerir. Kod ilk satırını yeni bir örneğini oluşturur `ProductController` sınıfı. İkinci satır kod denetleyicinin çağırır `Details()` eylem yöntemi. Son olarak, son satırının desteklemediğini görünümü tarafından döndürülen kodu denetimleri `Details()` Ayrıntılar görünümü bir eylemdir.

`ViewResult.ViewName` Özelliği, denetleyici tarafından döndürülen görünümün adını temsil eder. Bu özellik test etme hakkında bir büyük uyarı. Bir denetleyici görünüm döndürebilir iki yolu vardır. Bir denetleyici açıkça şuna benzer bir görünüm döndürebilirsiniz:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Alternatif olarak, görünümün adını şöyle denetleyici eylemini addan:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Bu denetleyici eylemi de adlı bir görünüm verir `Details`. Ancak, görünümün adını eylem adından algılanır. Görünüm adı test etmek isterseniz, açıkça Görünüm adı denetleyici eylemden döndürmelidir.

Her iki tuş bileşimini girerek birim testi listeleme 2'de çalıştırabilirsiniz **Ctrl-R, A** veya tıklatarak **Çözümdeki tüm Testleri Çalıştır** (bkz: Şekil 1) düğmesine tıklayın. Test başarılı olursa, Şekil 2 Test Sonuçları penceresinde görürsünüz.


[![Çözümde tüm Testleri Çalıştır](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Şekil 01**: Çözümdeki tüm testleri çalıştır ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![Başarılı!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Şekil 02**: başarılı! ([Tam boyutlu görüntüyü görüntülemek için tıklatın](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Bir denetleyici tarafından döndürülen görünüm verileri test etme

MVC denetleyicisi veri olarak adlandırılan bir şey kullanarak bir görünümüne geçirir *`View Data`*. Örneğin, çağırdığınızda, belirli bir ürünü ayrıntılarını görüntülemek istediğiniz düşünün `ProductController Details()` eylem. Bu durumda, bir örneğini oluşturabilirsiniz bir `Product` sınıfı (modelinizde tanımlı) ve örneğine geçirin `Details` yararlanarak Görünüm `View Data`.

Değiştirilen `ProductController` listeleme 3'te güncelleştirilmiş içeren `Details()` bir ürün döndüren eylem.

**Kod 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

İlk olarak, `Details()` eylemi yeni bir örneğini oluşturur `Product` dizüstü bilgisayar temsil eden sınıf. Sonraki, örneği `Product` sınıfı için ikinci parametre olarak geçirilir `View()` yöntemi.

Birim testleri yazma beklenen verileri olup olmadığını sınamak için Görünüm veri içeriyor. Dizüstü bilgisayar temsil eden bir ürün çağırdığınızda döndürülen olup olmadığına bakılmaksızın Birim listeleme 4 testlerindeki test `ProductController Details()` eylem yöntemi.

**4 listeleme – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Listeleme 4'te `TestDetailsView()` yöntemi çağırma tarafından döndürülen görünüm verilerini testleri `Details()` yöntemi. `ViewData` Üzerinde bir özellik olarak sunulan `ViewResult` çağırarak döndürülen `Details()` yöntemi. `ViewData.Model` Özelliği, görünüme iletilen ürün içerir. Test yalnızca görünüm verilerde bulunan ürün dizüstü bilgisayar adına sahip olduğunu doğrular.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Bir denetleyici tarafından döndürülen eylem sonucu test etme

Daha karmaşık bir denetleyici eylemi denetleyicisi eyleme geçirilen parametreler, eylem sonuçlarını değerlere bağlı olarak farklı türlerde döndürebilir. Bir denetleyici eylemi çeşitli türleri dahil olmak üzere eylem sonuçlarını döndürebilir bir `ViewResult`, `RedirectToRouteResult`, veya `JsonResult`.

Örneğin, değiştirilmiş `Details()` listeleme 5 eylemde döndürür `Details` geçerli bir ürün kimliği için eylem geçirdiğinizde görüntüleyin. Bir geçersiz ürün kimliği--değeri olan bir kimliği 1--sonra yönlendirilirsiniz daha az geçirirseniz `Index()` eylem.

**5 listeleme – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Davranışını sınayabilirsiniz `Details()` listeleme 6'daki birim testi eylemiyle. Listeleme 6'daki birim testi için yönlendirilir doğrular `Index` görüntülemek için bir kimlik değeri -1 ile geçirildiğinde `Details()` yöntemi.

**6 listeleme – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Çağırdığınızda `RedirectToAction()` denetleyici eylemi bir denetleyici eylem yöntemine döndürür bir `RedirectToRouteResult`. Test denetimleri olup olmadığını `RedirectToRouteResult` kullanıcı adlı bir denetleyici eylemi yönlendirir `Index`.

## <a name="summary"></a>Özet

Bu öğreticide, MVC denetleyici eylemleri için birim testleri oluşturmak nasıl öğrendiniz. İlk olarak, sağ görünümdeki bir denetleyici eylemi tarafından döndürülen olup olmadığını doğrulamak nasıl öğrendiniz. Nasıl kullanılacağı hakkında bilgi edindiniz `ViewResult.ViewName` özelliği bir görünümün adını doğrulayın.

Ardından, içeriği test nasıl incelenmesi `View Data`. Doğru ürün içinde döndürüldü olup olmadığını denetlemek öğrendiniz `View Data` bir denetleyici eylemi çağrıldıktan sonra.

Son olarak, eylem sonuçlarını farklı türde bir denetleyici eylemin getirdiği olup olmadığını nasıl sınayabilirsiniz açıklanmıştır. Bir denetleyici döndürüp döndürmediğini test etme hakkında bilgi edindiniz bir `ViewResult` veya `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Önceki](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
