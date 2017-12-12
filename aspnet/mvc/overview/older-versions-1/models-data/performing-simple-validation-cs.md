---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: "Basit bir doğrulama (C#) gerçekleştirme | Microsoft Docs"
author: StephenWalther
description: "Bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmeyi öğrenin. Bu öğreticide, model durumu ve doğrulama HTML Yardımcısı Stephen Walther tanıtır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 005872308d9d4d8ac7feb12dd5ab1fc463d0140e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-c"></a>Basit bir doğrulama (C#) gerçekleştirme
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmeyi öğrenin. Bu öğreticide, model durumu ve doğrulama HTML Yardımcıları Stephen Walther tanıtır.


Bu öğretici bir ASP.NET MVC uygulaması içindeki doğrulama nasıl gerçekleştirebileceğiniz açıklamak için hedefidir. Örneğin, birisi gerekli bir alan için bir değer içermeyen bir form göndermelerini engellemek nasıl öğrenin. Model durumu ve doğrulama HTML Yardımcıları kullanmayı öğrenin.

## <a name="understanding-model-state"></a>Model durumu anlama

Doğrulama hataları göstermek için model durumu - veya daha doğru bir şekilde model durumu Sözlüğü-'nı kullanın. Örneğin, ürün sınıfı bir veritabanına eklemeden önce bir ürün sınıf özelliklerini listeleme 1 Create() eylemde doğrular.


I bir denetleyiciye doğrulama veya veritabanı mantığı eklemek öneren değil. Bir denetleyici yalnızca uygulama akış denetimi ile ilgili mantığının içermelidir. Biz örneği basit tutmak için bir kısayol sürüyor.


**1 - Controllers\ProductController.cs listeleme**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

Listeleme 1'de ürün sınıfın adını, açıklamasını ve unitsInStock özelliklerini doğrulanır. Bu özelliklerin herhangi bir doğrulama testi başarısız olursa bir hata (denetleyici sınıfının ModelState özelliği tarafından temsil edilen) model durumu sözlüğüne eklenir.

Model durumu hataları varsa ModelState.IsValid özelliği false döndürür. Bu durumda, yeni bir ürün oluşturmak için HTML formu görünürler. Aksi takdirde, doğrulama hatası varsa, yeni ürün veritabanına eklenir.

## <a name="using-the-validation-helpers"></a>Doğrulama Yardımcıları kullanma

ASP.NET MVC çerçevesi iki doğrulama Yardımcıları içerir: Html.ValidationMessage() Yardımcısı ve Html.ValidationSummary() Yardımcısı. Bu iki Yardımcıları bir görünümde doğrulama hata iletilerini görüntülemek için kullanın.

ASP.NET MVC yapı iskelesi tarafından otomatik olarak oluşturulan oluşturma ve düzenleme görünümlerinde Html.ValidationMessage() ve Html.ValidationSummary() Yardımcıları kullanılır. Oluştur görünümünün oluşturmak için aşağıdaki adımları izleyin:

1. Ürün denetleyicisi Create() eylemde sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (bkz: Şekil 1).
2. İçinde **Görünüm Ekle** iletişim kutusunda, etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak** (bkz: Şekil 2).
3. Gelen **görüntülemek veri sınıfı** açılır listesinde, ürün sınıfı seçin.
4. Gelen **görüntülemek içerik** açılır listesi, select oluşturma.
5. Tıklatın **Ekle** düğmesi.


Bir görünüm eklemeden önce uygulamanızın oluşturduğunuzdan emin olun. Sınıfların listesi Aksi halde, görünmez **görüntülemek veri sınıfı** açılır liste.


[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**Şekil 01**: bir görünüm ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-simple-validation-cs/_static/image2.png))


[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**Şekil 02**: kesin türü belirtilmiş bir görünüm oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-simple-validation-cs/_static/image4.png))


Bu adımları tamamladıktan sonra listeleme 2'de Oluştur görünümünün alın.

**2 - Views\Product\Create.aspx listeleme**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

Listeleme 2'de Html.ValidationSummary() Yardımcısı hemen HTML formu çağrılır. Bu yardımcı doğrulama hata iletilerinin listesini görüntülemek için kullanılır. Html.ValidationSummary() yardımcı hataları madde işaretli listede işler.

Html.ValidationMessage() yardımcı her HTML form alanlarının yanındaki olarak adlandırılır. Bu yardımcı bir hata iletisi sağ form alanının yanındaki görüntülemek için kullanılır. Bir hata olduğunda listeleme 2 söz konusu olduğunda, bir yıldız işareti Html.ValidationMessage() yardımcı görüntüler.

Şekil 3'te sayfa eksik alanlar ve geçersiz değerler ile form gönderildiğinde doğrulama Yardımcılar tarafından işlenen hata iletileri gösterir.


[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**Şekil 03**: Create VIEW sorunları gönderilen ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-simple-validation-cs/_static/image6.png))


HTML görünümünü bir doğrulama hatası olduğunda alanları ayrıca değiştirildiğinde giriş dikkat edin. Html.TextBox() yardımcı işler bir *sınıfı "giriş doğrulama hata" =* Html.TextBox() Yardımcısı tarafından işlenen özelliği ilişkili doğrulama hatası olduğunda özniteliği.

Doğrulama hataları görünümünü kontrol etmek için kullanılan, üç, geçişli stil sayfası sınıfları şunlardır:

- Giriş-doğrulama hata - uygulanan &lt;giriş&gt; Html.TextBox() Yardımcısı tarafından işlenen etiketi.
- alan-doğrulama hata - uygulanan &lt;span&gt; Html.ValidationMessage() Yardımcısı tarafından işlenen etiketi.
- doğrulama-Özet-hataları - uygulanan &lt;ul&gt; Html.ValidationSumamry() Yardımcısı tarafından işlenen etiketi.

Bu geçişli stil sayfası sınıfları değiştirmek ve bu nedenle içerik klasöründe bulunan Site.css dosyasını değiştirerek doğrulama hataları görünümünü değiştirin.

> [!NOTE] 
> 
> CSS ilgili doğrulama adları alınıyor HtmlHelper sınıfı salt okunur statik özellikler içeren sınıf. Bu statik özellikleri ValidationInputCssClassName, ValidationFieldCssClassName ve ValidationSummaryCssClassName olarak adlandırılır.


## <a name="prebinding-validation-and-postbinding-validation"></a>Doğrulama ve Postbinding doğrulama prebinding

Ürün oluşturma için HTML form gönderme ve fiyat alan ve unitsInStock alan için herhangi bir değer için geçersiz bir değer girerseniz, Şekil 4'te görüntülenen doğrulama iletileri elde edersiniz. Bu doğrulama hata iletilerinin alınacağı yeri?


[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**Şekil 04**: Prebinding doğrulama hataları ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-simple-validation-cs/_static/image8.png))


Doğrulama hatası iletilerinin - HTML form alanlarını bir sınıfa bağlanır ve form alanlarını sınıfa bağlı sonra bu oluşturulan önce oluşturulanların gerçekte iki tür vardır. Diğer bir deyişle, vardır prebinding doğrulama hataları ve postbinding doğrulama hataları.

1. listeleme ürün denetleyicisi tarafından sunulan Create() eylem ürün sınıfının bir örneği kabul eder. Create yöntemi imzası şöyle görünür:

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

Form oluştur HTML form alanlardan değerlerini productToCreate sınıf için bir model bağlayıcı adlı bir şey tarafından bağlıdır. Bir form özelliği için bir form alanı bağlanamıyor varsayılan model bağlayıcısını bir hata iletisi model durumuna otomatik olarak ekler.

Varsayılan model bağlayıcısını "apple" dizesi ürün sınıfın fiyat özelliği bağlanamaz. Ondalık özelliği için bir dize atayamazsınız. Bu nedenle, model bağlayıcı hata model durumuna ekler.

Varsayılan model bağlayıcısını null değerlere kabul etmiyorum bir özellik için de bir null değer atanamaz. Özellikle, model bağlayıcı unitsInStock özelliğine bir null değer atanamaz. Bir kez daha, model bağlayıcı Vazgeçmeden ve model durumuna bir hata iletisi ekler.

Hata iletileri prebinding bunlar görünümünü özelleştirmek istiyorsanız, bu iletiler için kaynak dizeleri oluşturmanız gerekir.

## <a name="summary"></a>Özet

ASP.NET MVC çerçevesi doğrulama temel mekanikleri tanımlamak için bu öğreticinin amacı oluştu. Model durumu ve doğrulama HTML Yardımcıları nasıl kullanılacağı hakkında bilgi edindiniz. Biz de prebinding ve doğrulama postbinding arasında ayrım açıklanmıştır. Diğer öğreticiler, biz doğrulama kodunuzu denetleyicilerinizi dışında ve model sınıflarınızı halinde taşıma için çeşitli stratejiler ele alacağız.

>[!div class="step-by-step"]
[Önceki](displaying-a-table-of-database-data-cs.md)
[sonraki](validating-with-the-idataerrorinfo-interface-cs.md)
