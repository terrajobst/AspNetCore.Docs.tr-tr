---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Veri Görünümü ana sayfalar (VB) geçirme | Microsoft Docs
author: microsoft
description: Bu öğreticinin nasıl veri bir denetleyicisinden görünümü ana sayfaya geçirebilirsiniz açıklamak için hedeftir. Veri görünümüne m geçirme iki stratejileri inceleyeceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd7c5baacc00490720d1f82252d81e40c097c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="passing-data-to-view-master-pages-vb"></a>Görünüm ana sayfalar (VB) için veri geçirme
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> Bu öğreticinin nasıl veri bir denetleyicisinden görünümü ana sayfaya geçirebilirsiniz açıklamak için hedeftir. Bir görünüm ana sayfaya veri geçirme iki stratejileri inceleyeceğiz. İlk olarak, uygulamada etmek zordur sonuçları kolay bir çözüm tartışın. Ardından, biraz daha sürdürülebilir uygulama sonuçlarında ancak daha fazla ilk iş gerektiren çok daha iyi bir çözüm inceleyeceğiz.


## <a name="passing-data-to-view-master-pages"></a>Görünüm ana sayfalar için veri geçirme

Bu öğreticinin nasıl veri bir denetleyicisinden görünümü ana sayfaya geçirebilirsiniz açıklamak için hedeftir. Bir görünüm ana sayfaya veri geçirme iki stratejileri inceleyeceğiz. İlk olarak, uygulamada etmek zordur sonuçları kolay bir çözüm tartışın. Ardından, biraz daha sürdürülebilir uygulama sonuçlarında ancak daha fazla ilk iş gerektiren çok daha iyi bir çözüm inceleyeceğiz.

### <a name="the-problem"></a>Sorun

Film veritabanı uygulaması oluşturma ve uygulamanızdaki her sayfada film kategori listesi, görüntülemek istediğiniz düşünün (bkz: Şekil 1). Ayrıca, film kategori listesi, bir veritabanı tablosunda depolanır düşünün. Bu durumda, kategoriler veritabanından almak ve bir görünüm ana sayfa içinde film kategori listesi işlemek için anlamlı olacaktır.


[![Bir görünüm ana sayfasında film kategorileri görüntüleme](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Şekil 01**: bir görünüm ana sayfasında film kategorileri görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](passing-data-to-view-master-pages-vb/_static/image3.png))


Sorun aşağıda verilmiştir. Ana sayfaya film kategorilerde listesini nasıl aldığını? Model sınıflarınızı yöntemlerinin ana sayfasında doğrudan çağırmak için tempting. Diğer bir deyişle, verileri ana sayfanızın veritabanı sağa alınırken kodunu dahil etmek için tempting. Ancak, veritabanına erişmek için MVC denetleyicileri atlayarak sorunlarının bir MVC uygulaması oluşturmanın birincil avantajlarından biri olan temiz ayrımı ihlal ediyor.

Bir MVC uygulamasında MVC görünümlerinizde ve MVC modeline MVC denetleyicileri tarafından işlenecek arasındaki tüm etkileşim istiyor. Bu sorunları ayrılması daha sürdürülebilir, uyarlanabilir ve sınanabilir bir uygulamada sonuçlanır.

Bir MVC uygulamasında – bir görünüm ana sayfası dahil olmak üzere – bir görünüme iletilen tüm verileri bir denetleyici eylemi tarafından bir görünüme geçirilmesi gerekir. Ayrıca, veri görünümü verileri yararlanarak geçirilmesi gerekir. Bu öğreticinin geri kalanında içinde ı görünümü ana sayfa için Görünüm veri geçirme için iki yöntem inceleyin.

### <a name="the-simple-solution"></a>Basit çözüm

Görünüm veri görünümü ana sayfaya bir denetleyicisinden geçirme için basit çözümü ile başlayalım tıklatın. En basit çözüm, her denetleyici eylem ana sayfa için Görünüm verileri geçirmektir.

Denetleyici 1 listeleme göz önünde bulundurun. Adlı iki eylem sunan `Index()` ve `Details()`. `Index()` Eylem yöntemi, filmler veritabanı tablosunda her film döndürür. `Details()` Eylem yöntemi bir belirli film kategorideki her film döndürür.

**Kod 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Dikkat hem `Index()` ve `Details()` eylemlerini verileri görüntülemek için iki öğeyi ekleyin. `Index()` Eylem iki anahtar ekler: kategorileri ve filmler. Kategoriler anahtarı görünüm ana sayfa tarafından görüntülenen film Kategoriler listesini temsil eder. Film anahtar dizini görünüm sayfası tarafından görüntülenen filmler listesini temsil eder.

`Details()` Eylem ayrıca kategorileri ve filmler adlı iki anahtar ekler. Kategorileri anahtarının bir kez daha, görünüm ana sayfa tarafından görüntülenen film Kategoriler listesini temsil eder. Ayrıntılar görünümü sayfası tarafından görüntülenen belirli bir kategorideki filmler listesi filmler anahtarı temsil eder (bkz: Şekil 2).


[![Ayrıntılar görünümü](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Şekil 02**: ayrıntıları görüntüleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](passing-data-to-view-master-pages-vb/_static/image6.png))


Dizin görünümünün listeleme 2'de yer alır. Bunu yalnızca görünüm verilerini filmler öğesi tarafından temsil edilen filmler listesini dolaşır.

**Kod 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

Görünüm ana sayfa listeleme 3'te yer alır. Görünüm ana sayfa tekrarlanan ve tüm kategorileri öğesiyle görünüm verileri temsil film kategorilerini işler.

**Kod 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Tüm veriler iletilir görünümü ve görünüm ana sayfa için Görünüm verilerine. Ana sayfaya veri iletmek için doğru bir şekilde olmasıdır.

Bu nedenle, bu çözüm ile yanlış nedir? Bu çözüm KURU (yok yineleyin kendiniz) ilkesini ihlal sorunudur. Her denetleyici eylemi çok aynı kategori listesi, verileri görüntülemek için film eklemeniz gerekir. Yinelenen kodu, uygulamanızda olan uygulamanızı korumak, uyum ve değiştirmek çok daha zor hale getirir.

### <a name="the-good-solution"></a>İyi çözümü

Bu bölümde, veri görünümü ana sayfaya denetleyicisi eylemden geçirme için alternatif ve daha iyi bir çözüm inceleyeceğiz. Ana sayfaya film kategoriler her denetleyici eylemi yerine, biz film kategorileri görünüm verilerine yalnızca bir kez ekleme. Görünüm ana sayfa tarafından kullanılan tüm görünüm verilerini bir uygulama denetleyicisi eklenir.

ApplicationController sınıfı listeleme 4'te yer alır.

ApplicationController sınıfı listeleme 4'te yer alır.

**4 listeleme – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Uygulama denetleyicisi listeleme 4 hakkında dikkat etmelidir üç nokta vardır. İlk olarak, taban System.Web.Mvc.Controller sınıftan sınıfı dikkat edin. Uygulama denetleyici sınıfı denetleyicisidir.

İkinci olarak, uygulama denetleyicisi sınıfı MustInherit sınıfı olduğuna dikkat edin. MustInherit sınıfı somut bir sınıf tarafından uygulanan bir sınıftır. Uygulama denetleyicisi MustInherit sınıfı olduğundan, doğrudan sınıfında tanımlanan herhangi bir yöntem çağıramazsınız değil. Daha sonra uygulama sınıfı doğrudan çağırma çalışırsanız, bir kaynak bulunamıyor hata iletisi alırsınız.

Üçüncü uygulama denetleyicisi verileri görüntülemek için film kategorileri listesi ekler bir oluşturucu içerdiğine dikkat edin. Uygulama denetleyicisinden devralır her denetleyici sınıfı uygulama denetleyicinin Oluşturucusu otomatik olarak çağırır. Uygulama denetleyicisinden devralır denetleyicisinde herhangi bir eylem çağrısı her film kategorileri dahil görünüm verileri otomatik olarak.

Film denetleyicisi listeleme 5'te uygulama denetleyicisinden devralır.

**5 listeleme – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Film denetleyicisi yalnızca önceki bölümde tartışılan giriş denetleyicisi gibi adlı iki eylem yöntemlerini gösterir `Index()` ve `Details()`. Görünüm ana sayfa tarafından görüntülenen film kategorileri listesinde olmayan bildirim eklenen ya da veri görüntülemek için `Index()` veya `Details()` yöntemi. Film denetleyicisi uygulama denetleyicisinden devralındığından film kategori listesi, verileri otomatik olarak görüntülemek için eklenir.

Bir görünüm ana sayfa için Görünüm verileri eklemek için bu çözümün KURU (yok yineleyin kendiniz) ilkeyi ihlal değil dikkat edin. Verileri görüntülemek için film kategori listesi ekleme kodunu yalnızca tek bir yerde bulunur: uygulama denetleyicisi Oluşturucusu.

### <a name="summary"></a>Özet

Bu öğreticide, bir görünüm ana sayfaya bir denetleyicisinden görünüm verilerini geçirme için iki yaklaşım açıklanmıştır. İlk olarak, ancak zor bir yaklaşım korumak basit bir incelendi. İlk bölümde, nasıl bir görünüm ana sayfa için Görünüm verileri her her denetleyici eylemi uygulamanızda ekleyebileceğiniz açıklanmıştır. KURU (yok yineleyin kendiniz) ilkesini ihlal ettiğinden bu hatalı bir yaklaşım olduğunu sonuçları.

Ardından, bir görünüm ana sayfa verileri görüntülemek için gerekli verileri eklemek için çok daha iyi bir stratejiye incelendi. Görünüm verilerini her denetleyici eylem eklemek yerine, bir uygulama denetleyicisi içinde yalnızca bir kez görünüm verileri eklediğimiz. Bu şekilde, bir ASP.NET MVC uygulamasındaki bir görünüm ana sayfasına veri geçirilirken yinelenen kod önleyebilirsiniz.

> [!div class="step-by-step"]
> [Önceki](creating-page-layouts-with-view-master-pages-vb.md)
