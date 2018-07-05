---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: (C#) görünüm ana sayfalarına veri geçirme | Microsoft Docs
author: microsoft
description: Bu öğreticide nasıl veri bir denetleyiciden görünüm ana sayfaya geçirebilirsiniz açıklamak için hedefidir. M görünümü verileri geçirmek için iki stratejileri inceleyeceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 02586f145de9c6d654c5d815291a340fd037cc6f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397752"
---
<a name="passing-data-to-view-master-pages-c"></a>(C#) görünüm ana sayfalarına veri geçirme
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> Bu öğreticide nasıl veri bir denetleyiciden görünüm ana sayfaya geçirebilirsiniz açıklamak için hedefidir. Görünüm ana sayfaya verileri geçirmek için iki stratejileri inceleyeceğiz. İlk olarak, bakımını yapmak zor bir uygulamada sonuçları bir kolayca çözüm ele alır. Ardından, biraz daha fazla ilk iş sonuçları daha sürdürülebilir bir uygulama içinde ancak gerektiren çok daha iyi bir çözüm inceleyeceğiz.


## <a name="passing-data-to-view-master-pages"></a>Görünüm ana sayfalarına veri geçirme

Bu öğreticide nasıl veri bir denetleyiciden görünüm ana sayfaya geçirebilirsiniz açıklamak için hedefidir. Görünüm ana sayfaya verileri geçirmek için iki stratejileri inceleyeceğiz. İlk olarak, bakımını yapmak zor bir uygulamada sonuçları bir kolayca çözüm ele alır. Ardından, biraz daha fazla ilk iş sonuçları daha sürdürülebilir bir uygulama içinde ancak gerektiren çok daha iyi bir çözüm inceleyeceğiz.

### <a name="the-problem"></a>Sorun

Bir film veritabanı uygulaması oluşturuyorsunuz ve uygulamanızdaki her sayfada film Kategoriler listesini görüntülemek istediğiniz Imagine (bkz. Şekil 1). Ayrıca, film kategori listesi bir veritabanı tablosunda depolandığını varsayın. Bu durumda, kategoriler veritabanından ve bir görünüm ana sayfa içinde film kategori listesi işlemek için anlamlı olacaktır.


[![Bir görünüm ana sayfasında film kategorilerini görüntüleme](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Şekil 01**: bir görünüm ana sayfasında film kategorileri görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](passing-data-to-view-master-pages-cs/_static/image3.png))


Sorun aşağıda verilmiştir. Ana sayfaya film kategorilerde listesini nasıl aldığını? Bu model sınıflarınızı yöntemlerinin ana sayfasında doğrudan çağırmak için daha cazip bir işlemdir. Diğer bir deyişle, daha cazip ana sayfanıza veritabanı sağdan veri almak için kod içerir. Ancak, veritabanına erişmek için MVC denetleyicileri atlayarak bir MVC uygulaması oluşturmanın birincil yararlarından biri olan ayrılmasına ihlal ediyor.

Bir MVC uygulamasında MVC görünümlerinizde ve MVC model, MVC denetleyicileri tarafından işlenecek arasındaki tüm etkileşim istersiniz. Bu ayrılması daha sürdürülebilir, uyarlanabilir ve test edilebilir uygulamada sonuçlanır.

Bir MVC uygulamasında görünümü – bir görünüm ana sayfası dahil olmak üzere – aktarılan tüm veriler bir denetleyici eylemi tarafından bir görünüme geçirilmelidir. Ayrıca, veri görünümü verileri avantajlarından yararlanarak geçirilmelidir. Bu öğreticinin geri kalanında içinde ben iki görünüm ana sayfa için Görünüm veri geçirme yöntemleri inceleyin.

### <a name="the-simple-solution"></a>Basit bir çözüm

Görünüm verilerini bir denetleyiciden görünüm ana sayfaya geçirme için basit çözüm başlayalım. Basit çözüm, her denetleyici eylem ana sayfa için Görünüm verileri geçirmektir.

Denetleyici 1 listeleme göz önünde bulundurun. Adlı iki eylem kullanıma sunduğu `Index()` ve `Details()`. `Index()` Eylem yöntemi, film veritabanı tablosu, her filmin döndürür. `Details()` Eylem yöntemi, belirli bir filmi kategorisinde her film döndürür.

**Kod 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Verileri görüntülemek için iki öğeyi İNDİS() hem Details() eylemler ekleme dikkat edin. İki anahtar İNDİS() eylem ekler: kategorileri ve filmler. Kategorileri anahtarı görünüm ana sayfa tarafından görüntülenen film kategori listesi temsil eder. Filmler anahtarı dizin görünümü sayfa tarafından görüntülenen filmler listesini temsil eder.

Details() eylem, adlandırılmış kategorileri ve filmler iki anahtar da ekler. Kategorileri anahtarı bir kez daha, görünüm ana sayfa tarafından görüntülenen film Kategoriler listesini temsil eder. Ayrıntılar görünümü sayfa tarafından görüntülenen belirli bir kategorideki filmler listesini filmler anahtarı temsil eder (bkz: Şekil 2).


[![Ayrıntılar görünümü](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Şekil 02**: ayrıntılarını görüntüleyin ([tam boyutlu görüntüyü görmek için tıklatın](passing-data-to-view-master-pages-cs/_static/image6.png))


Dizin görünümünün listeleme 2'de yer alır. Yalnızca görünüm verilerini filmler öğesinde tarafından temsil edilen filmler listesi üzerinden yinelenir.

**Kod 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

Görünüm ana sayfası listeleme 3'te yer alır. Görünüm ana sayfası yinelenir ve tüm kategorileri öğe tarafından temsil edilen görünüm verileri film kategorilerini işler.

**Kod 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Tüm veriler iletilir görünümü ve görünüm ana sayfası için Görünüm verilerine. Ana sayfaya veri iletmek için doğru şekilde olmasıdır.

Bu nedenle, bu çözüm ile sorun nedir? Bu çözüm KURU (yoksa yineleyin kendiniz) ilkesini ihlal sorunudur. Her denetleyici eylemi çok aynı verileri görüntülemek için film kategori listesi eklemeniz gerekir. Uygulamanızda yinelenen koduna sahip uygulamanızı korumak, uyum ve değiştirmek çok daha zor hale getirir.

### <a name="the-good-solution"></a>İyi bir çözümdür

Bu bölümde, veri görünümü ana sayfa için bir denetleyici eylemini geçirme için alternatif ve daha iyi bir çözüm inceleyeceğiz. Ana sayfaya film kategoriler her denetleyici eylemi ekleme yerine, film kategorileri ve görünüm verilerinin yalnızca bir kez ekleriz. Görünüm ana sayfa tarafından kullanılan tüm görünüm verilerini, bir uygulama denetleyicisi eklenir.

ApplicationController sınıfı listeleme 4'te yer alır.

**4 listeleme – `Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Listeleme 4'te uygulama denetleyiciyle ilgili fark etmişsinizdir üç şey vardır. İlk olarak, sınıfın temel System.Web.Mvc.Controller sınıfından devralan dikkat edin. Uygulama denetleyicisi sınıfı denetleyicisidir.

İkinci olarak, uygulama denetleyicisi sınıfı soyut bir sınıf olduğuna dikkat edin. Bir Özet sınıf bir somut sınıf tarafından uygulanan bir sınıftır. Bir soyut sınıf uygulama denetleyicisi olduğundan, doğrudan sınıfta tanımlanmış herhangi bir yöntem çağıramazsınız değil. Ardından uygulama sınıfı doğrudan çağırmak denerseniz bir kaynak bulunamıyor hata iletisi alırsınız.

Üçüncü olarak, uygulama denetleyicisi verilerini görüntülemek için film kategori listesi ekleyen bir oluşturucu içerdiğine dikkat edin. Uygulama denetleyicisinden devralan her denetleyici sınıfı otomatik olarak uygulama denetleyicinin oluşturucuyu çağırır. Uygulama denetleyicisinden devralan herhangi bir denetleyicisi herhangi bir işlem çağırmak her film kategoriler dahil görünüm verileri otomatik olarak.

Filmler denetleyicisi listeleme 5'te uygulama denetleyicisinden devralır.

**5 listeleme – `Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Yalnızca önceki bölümde açıklanan giriş denetleyicisine gibi filmler denetleyicisi adlı iki eylem yöntemleri sunar `Index()` ve `Details()`. Görünüm ana sayfa tarafından görüntülenen film kategori listesi değil bildirimi eklenen ya da veri görüntülemek için `Index()` veya `Details()` yöntemi. Denetleyici filmler uygulama denetleyicisinden devraldığından, film kategori listesi verisinin otomatik olarak eklenir.

Bu çözüm, bir görünüm ana sayfa için Görünüm veri eklemeye KURU (yoksa yineleyin kendiniz) ilkesini ihlal etmemesini dikkat edin. Verileri görüntülemek için film kategori listesi ekleme kodunu yalnızca tek bir konumda bulunur: uygulama denetleyicisi için oluşturucu.

### <a name="summary"></a>Özet

Bu öğreticide, görünüm verilerini bir denetleyiciden görünüm ana sayfaya geçirme için iki yaklaşım ele almıştık. İlk olarak, basit, yaklaşım sürdürülmesi zor ama incelenir. Bu bölümde, nasıl verilerini görüntülemek için bir görünüm ana sayfası her her denetleyici eylem uygulamanızda ekleyebileceğiniz almıştık. Biz KURU (yoksa yineleyin kendiniz) ilkesini ihlal ettiğinden bu hatalı bir yaklaşım ortaya koymuştur.

Ardından, verileri görüntülemek için bir görünüm ana sayfası için gerekli veri eklemek için çok daha iyi bir stratejiye incelenir. Her denetleyici eylem görünüm verilerini eklemek yerine bir uygulama denetleyicisi içinde yalnızca bir kez görünüm verileri ekledik. Bu şekilde, bir ASP.NET MVC uygulamasındaki bir görünüm ana sayfası için veri geçirirken, yinelenen kod önleyebilirsiniz.

> [!div class="step-by-step"]
> [Önceki](creating-page-layouts-with-view-master-pages-cs.md)
> [İleri](asp-net-mvc-views-overview-vb.md)
