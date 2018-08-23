---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Basit doğrulama (VB) gerçekleştirme | Microsoft Docs
author: StephenWalther
description: Bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmeyi öğreneceksiniz. Bu öğreticide, model durumu ve doğrulama HTML Yardımcısı Stephen Walther sunar...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d0bd6917bab61b17d1cafcf0cd9eb1983275dc8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755959"
---
<a name="performing-simple-validation-vb"></a>Basit doğrulama (VB) gerçekleştirme
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmeyi öğreneceksiniz. Bu öğreticide, model durumu ve doğrulama HTML Yardımcıları Stephen Walther tanıtır.


Bu öğreticide bir ASP.NET MVC uygulaması içindeki doğrulama nasıl gerçekleştirebileceğiniz açıklamak için hedefidir. Örneğin, birisi, gerekli bir alan için bir değer içermeyen bir form göndermesinin önlenmesine hakkında bilgi edinin. Model durumu ve doğrulama HTML Yardımcıları kullanmayı öğrenin.

## <a name="understanding-model-state"></a>Model durumu anlama

Doğrulama hataları temsil eden bir model durumu - veya daha doğru bir şekilde ve model durumu sözlüğündeki-'nı kullanın. Örneğin, ürün sınıfı için bir veritabanı eklemeden önce bir ürün sınıf özelliklerini listeleme 1 Create() eylemi doğrular.


Ben bir denetleyiciye doğrulama veya veritabanı mantığınızı eklemek öneren değil. Bir denetleyici yalnızca mantık uygulaması akış denetimi ile ilgili içermelidir. Örneği basit tutmak için bir kısayol sürüyor.


**1 - Controllers\ProductController.vb listeleme**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Listeleme 1'de ürün sınıfın adını, açıklamasını ve unitsInStock özelliklerini doğrulanır. Bu özelliklerin herhangi bir doğrulama testi başarısız olursa bir hata (denetleyici sınıfının ModelState özelliği tarafından gösterilen) model durumu sözlüğüne eklenir.

Model durumu hataları varsa ModelState.IsValid özelliği false döndürür. HTML formu yeni bir ürün oluşturmaya yönelik bu durumda görünürler. Aksi takdirde, doğrulama hatası varsa, yeni ürün veritabanına eklenir.

## <a name="using-the-validation-helpers"></a>Doğrulama yardımcılarını kullanma

ASP.NET MVC çerçevesi iki doğrulama Yardımcıları içerir: Html.ValidationMessage() Yardımcısı ve Html.ValidationSummary() Yardımcısı. Doğrulama hatası iletilerini görüntülemek için Görünümü'nde bu iki Yardımcıları kullanın.

Html.ValidationMessage() ve Html.ValidationSummary() Yardımcıları, ASP.NET MVC yapı iskelesi tarafından otomatik olarak oluşturulan oluşturma ve düzenleme görünümlerinde kullanılır. Oluştur görünümünün oluşturmak için aşağıdaki adımları izleyin:

1. Menü seçeneği ürün denetleyicisi Create() eylemi sağ tıklayıp **Görünüm Ekle** (bkz. Şekil 1).
2. İçinde **Görünüm Ekle** iletişim kutusunda etiketli onay **kesin türü belirtilmiş görünüm oluşturmak** (bkz: Şekil 2).
3. Gelen **görüntülemek veri sınıfı** açılan listesinde, ürün sınıfı seçin.
4. Gelen **içeriği görüntüle** açılır listesi, select oluşturma.
5. Tıklayın **Ekle** düğmesi.


Uygulamanızı bir görünüm eklemeden önce oluşturduğunuzdan emin olun. Sınıf listesi Aksi takdirde görünmez **görüntülemek veri sınıfı** açılır liste.


[![Yeni Proje iletişim kutusu](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Şekil 01**: Görünüm ekleme ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-vb/_static/image2.png))


[![Yeni Proje iletişim kutusu](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Şekil 02**: kesin türü belirtilmiş görünüm oluşturmak ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-vb/_static/image4.png))


Bu adımları tamamladıktan sonra Oluştur görünümünün listeleme 2'de sahip olursunuz.

**2 - Views\Product\Create.aspx listeleme**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Listeleme 2'de Html.ValidationSummary() Yardımcısı hemen HTML form üzerinde çağrılır. Bu yardımcı, doğrulama hata iletilerinin listesini görüntülemek için kullanılır. Bir madde işaretli liste hataları Html.ValidationSummary() Yardımcısı işler.

Html.ValidationMessage() Yardımcısı her HTML form alanlarının yanındaki olarak adlandırılır. Bu yardımcı, bir form alanının yanında şu hata iletisi görüntülemek için kullanılır. Bir hata olduğunda listeleme 2 söz konusu olduğunda, bir yıldız işareti Html.ValidationMessage() Yardımcısı görüntüler.

Şekil 3'te sayfanın eksik alanlar geçersiz değerler ile form gönderildiğinde, doğrulama Yardımcılar tarafından işlenen hata iletilerini gösterir.


[![Yeni Proje iletişim kutusu](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Şekil 03**: sorunları gönderilen görünüm oluştur ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-vb/_static/image6.png))


HTML görünümünü bir doğrulama hatası olduğunda alanlar da değiştirilmez giriş dikkat edin. Html.TextBox() yardımcı işleme bir *sınıfı "giriş doğrulama hata" =* Html.TextBox() Yardımcısı tarafından işlenen özelliği ilişkili doğrulama hatası olduğunda özniteliği.

Doğrulama hataları görünümünü kontrol etmek için kullanılan üç basamaklı stil sayfası sınıfları şunlardır:

- Giriş-doğrulama-hata - uygulanan &lt;giriş&gt; Html.TextBox() Yardımcısı tarafından işlenen etiketi.
- alan-doğrulama-hata - uygulanan &lt;span&gt; Html.ValidationMessage() Yardımcısı tarafından işlenen etiketi.
- Doğrulama-Özeti-hata - uygulanan &lt;ul&gt; Html.ValidationSumamry() Yardımcısı tarafından işlenen etiketi.

Bu geçişli stil sayfası sınıfları değiştirebilir ve bu nedenle içerik klasöründe yer alan Site.css dosyasını değiştirerek doğrulama hatalarını görünümünü değiştirebilirsiniz.

> [!NOTE] 
> 
> CSS ilgili doğrulama adları alınırken HtmlHelper sınıfı salt okunur statik özelliklerini içeren sınıflar. Bu statik özellikleri ValidationInputCssClassName ValidationFieldCssClassName ve ValidationSummaryCssClassName adlandırılır.


## <a name="prebinding-validation-and-postbinding-validation"></a>Doğrulama ve Postbinding doğrulama prebinding

Bir ürün oluşturmaya HTML formu göndermeden ve Fiyat alanının ve unitsInStock alan için hiçbir değer için geçersiz bir değer girin, Şekil 4'te görüntülenen doğrulama iletilerinin elde edersiniz. Burada bu doğrulama hata iletileri gelir?


[![Yeni Proje iletişim kutusu](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Şekil 04**: Prebinding doğrulama hataları ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-vb/_static/image8.png))


Doğrulama hata iletilerinin - HTML form alanlarını bir sınıfa bağlanır ve form alanlarını sınıfa bağlı sonra bu oluşturulan önce oluşturulanların aslında iki türü vardır. Diğer bir deyişle, vardır prebinding doğrulama hatalarını ve postbinding doğrulama hataları.

Listeleme 1 ürün denetleyicisi tarafından kullanıma sunulan Create() eylem ürün sınıfının bir örneği kabul eder. Oluşturma metodun imzası şöyle görünür:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

HTML form alanlarını oluşturma formundaki değerlerini bir model bağlayıcı olarak adlandırılan bir şey tarafından productToCreate sınıfa bağlıdır. Varsayılan model bağlayıcısını bir hata iletisi model durumuna bir form alanı için bir form özelliği bağlanamıyor olduğunda otomatik olarak ekler.

Varsayılan model bağlayıcısını, ürün sınıfın fiyat özelliği için dize "apple" bağlanamıyor. Bir dize ondalık bir özelliğe atanamaz. Bu nedenle, model bağlayıcı hata model durumuna ekler.

Varsayılan model bağlayıcısını de değer Nothing değeri hiçbir şey kabul etmeyen bir özelliğe atanamaz. Özellikle, model bağlayıcı değeri hiçbir şey unitsInStock özelliğine atama yapılamıyor. Bir kez daha, model bağlayıcı Vazgeçmeden ve bir hata iletisini model durumuna ekler.

Hata iletileri prebinding bu görünümünü özelleştirmek istiyorsanız, bu iletileri görmek için kaynak dizeleri oluşturmak gerekir.

## <a name="summary"></a>Özet

ASP.NET MVC çerçevesi doğrulama temel mekanikleri açıklamak için bu öğreticinin amacı oluştu. Model durumu ve doğrulama HTML Yardımcıları kullanmayı öğrendiniz. Ayrıca prebinding ve doğrulama postbinding arasında ayrım ele almıştık. Diğer öğreticileri, doğrulama kodunuzu denetleyicilerinizi ve model sınıflarınızı içine taşımak için çeşitli stratejileri açıklayacağız.

> [!div class="step-by-step"]
> [Önceki](displaying-a-table-of-database-data-vb.md)
> [İleri](validating-with-the-idataerrorinfo-interface-vb.md)
