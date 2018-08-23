---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Yineleme #3 – form doğrulaması ekleme (C#) | Microsoft Docs'
author: microsoft
description: Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de emai doğrulayın...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4115b3898415d63ffb122f3d0fea93022f2baa02
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757284"
---
<a name="iteration-3--add-form-validation-c"></a>Yineleme #3 – form doğrulaması ekleme (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Bir kişi yönetimi ASP.NET MVC uygulama (C#)
  

Bu öğretici serisinde, tamamlanması bir tüm kişi yönetimi uygulaması ekleriz. Kişi Yöneticisi uygulama kişilerin bir listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamanızı sağlar.

Birden çok yineleme üzerinde uygulama ekleriz. Her yineleme ile biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı amacı, her değişikliğin nedenini anlamak etkinleştirmektir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - uygulamanın güzel görünmesini olun. Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.

- Yineleme #3 - form doğrulaması ekleme. Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.

- Yineleme #4 - birbirine sıkı şekilde bağlı uygulama olun. Bu dördüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.

- Yineleme #5 - birim testleri oluşturun. Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.

- Yineleme #6 - test odaklı geliştirme kullanma. Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.

- Yineleme #7 - Ajax işlevselliği ekleme. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.


## <a name="this-iteration"></a>Bu yineleme

Kişi Yöneticisi uygulama bu ikinci yinelenmesinde temel form doğrulaması ekleriz. Biz, kişilerin kişi gerekli form alanları için değerler girmeden göndermesinin önlenmesine. Biz de telefon numarası ve e-posta adresleri (bkz. Şekil 1) doğrulayın.


[![Yeni Proje iletişim kutusu](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Şekil 01**: doğrulama formuyla ([tam boyutlu görüntüyü görmek için tıklatın](iteration-3-add-form-validation-cs/_static/image2.png))


Bu yineleme, doğrudan denetleyici eylemleri için doğrulama mantığını ekleriz. Genel olarak, bu doğrulama eklemek için bir ASP.NET MVC uygulaması için önerilen yöntem değildir. Bir uygulama s Doğrulama mantığı ayrı bir yerleştirmek için daha iyi bir yaklaşım olan [hizmet katmanı](http://martinfowler.com/eaaCatalog/serviceLayer.html). Bir sonraki yinelemede biz uygulamanın daha sürdürülebilir hale getirmek için Kişi Yöneticisi uygulama yeniden düzenleyin.

Bu yineleme, basit bir anlatım gözetildiği için biz tüm doğrulama kodları el ile yazın. Doğrulama kodu kendimize yazmak yerine, doğrulama çerçevesinin avantajlarından sürebilir. Örneğin, ASP.NET MVC uygulamanızın Doğrulama mantığı uygulamak için Microsoft Kurumsal kitaplığı doğrulama uygulama blok (VAB) kullanabilirsiniz. Doğrulama uygulama bloğu hakkında daha fazla bilgi için bkz:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Doğrulama oluşturma görünümü ekleme

Oluştur görünümünün için doğrulama mantığını ekleyerek başlayın s olanak tanır. Biz Oluştur görünümünün Visual Studio ile oluşturulmuş için Neyse ki, Oluştur görünümünün zaten tüm doğrulama iletileri görüntülemek için gerekli kullanıcı arabirimi mantığı içerir. Oluştur görünümünün listeleme 1'de yer alır.

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Hemen HTML formun görünen Html.ValidationSummary() yardımcı yöntemine çağrı dikkat edin. Doğrulama hatası iletilerinin varsa, bu yöntem doğrulama iletilerinin bir madde işaretli listede görüntüler.

Bildirim, ayrıca, her bir form alanından yanında görünen Html.ValidationMessage() çağrıları. ValidationMessage() yardımcı bir tek doğrulama hata iletisi görüntüler. Bir doğrulama hatası olduğunda listeleme 1 söz konusu olduğunda, bir yıldız işareti görüntülenir.

Son olarak, Yardımcısı tarafından görüntülenen özelliği ile ilişkilendirilen bir doğrulama hatası olduğunda Html.TextBox() yardımcı bir geçişli stil sayfası sınıfı otomatik olarak işler. Adlı bir sınıf Html.TextBox() Yardımcısı işler **giriş doğrulama hatası**.

Site.css adlı bir stil sayfası içerik klasöründe yeni bir ASP.NET MVC uygulaması oluşturduğunuzda, otomatik olarak oluşturulur. Bu stil sayfası doğrulama hatası iletilerinin görünümünü ilgili CSS sınıfları için aşağıdaki tanımları içerir:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

Alan doğrulama hatası sınıf Html.ValidationMessage() Yardımcısı tarafından işlenmiş çıktı stilini belirlemek için kullanılır. Giriş doğrulama hatası sınıf Html.TextBox() Yardımcısı tarafından işlenen (giriş) metin stilini belirlemek için kullanılır. Doğrulama Özeti hata sınıf Html.ValidationSummary() Yardımcısı tarafından işlenen sırasız liste stilini belirlemek için kullanılır.

> [!NOTE] 
> 
> Doğrulama hatası iletilerinin görünümünü özelleştirmek için bu bölümde açıklanan stil sayfası sınıfları değiştirebilirsiniz.


## <a name="adding-validation-logic-to-the-create-action"></a>Doğrulama mantığını ekleme Eylem oluştur

Şu anda, biz iletileri oluşturmak için mantıksal yazmadığınızdan Oluştur görünümünün hiçbir zaman doğrulama hatası iletilerini görüntüler. Doğrulama hatası iletilerini görüntülemek için ModelState için hata iletileri eklemeniz gerekir.

> [!NOTE] 
> 
> UpdateModel() yöntemi hata iletileri ModelState için bir özellik için bir form alanının değerini atama bir hata olduğunda otomatik olarak ekler. "Apple" dize DateTime değerleri kabul eden bir doğum tarihi özelliğe atanacak çalışırsanız, örneğin, ardından UpdateModel() yöntemi hata ModelState için ekler.


Listeleme 2 değiştirilmiş Create() yöntemi veritabanına yeni kişi eklenmeden önce ilgili kişi sınıf özelliklerini doğrulama yeni bir bölüm içerir.

**2 - Controllers\ContactController.cs (doğrulama ile Oluştur) listeleme**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

Doğrula bölümüne dört farklı doğrulama kuralları zorlar.

- FirstName özelliği bir uzunluk sıfırdan büyük olmalıdır (ve boşluklardan oluşamaz)
- Soyadı özelliği bir uzunluk sıfırdan büyük olmalıdır (ve boşluklardan oluşamaz)
- Telefon özelliği, bir değer varsa (0'dan büyük bir uzunluğuna sahip) sonra telefon özelliği, normal bir ifadeyle eşleşen gerekir.
- E-posta özelliği değeri varsa (0'dan büyük bir uzunluğuna sahip) sonra e-posta özelliği bir normal ifadesi ile eşleşmelidir.

Doğrulama kuralı ihlal olduğunda, bir hata iletisi AddModelError() yönteminin yardımıyla ModelState için eklenir. ModelState için bir ileti eklediğinizde, bir özelliğin adını ve bir doğrulama hata iletisinin metni sağlar. Bu hata iletisi tarafından Html.ValidationSummary() ve Html.ValidationMessage() yardımcı yöntemler Görünümü'nde görüntülenir.

Doğrulama kuralları yürütüldükten sonra ModelState IsValid özelliğini denetlenir. ModelState için herhangi bir doğrulama hata iletisinin eklendiğinde IsValid özelliği false döndürür. Doğrulama başarısız olursa form oluştur hata iletileri ile yeniden görüntülenir.

> [!NOTE] 
> 
> Normal ifade depoyu telefon numarası ve e-posta adresinden doğrulamak için normal ifadeleri aldım [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Doğrulama mantığını düzenleme eylemini ekleme

Edit() eylem, bir kişiyi güncelleştirir. Tam olarak aynı doğrulama Create() eylem olarak gerçekleştirmek Edit() eylem gerekir. Böylece Create() hem Edit() Eylemler aynı doğrulama yöntemi çağrısı aynı doğrulama kodunu çoğaltmak yerine, biz kişi denetleyicisi yeniden düzenlemeniz gerekir.

Değiştirilen kişi denetleyicisi sınıfı listeleme 3'te yer alır. Bu sınıf Create() ve Edit() Eylemler içinde çağrılan yeni ValidateContact() yöntemi vardır.

**3 - Controllers\ContactController.cs listeleme**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Özet

Bu yineleme, kişi yöneticisi uygulamamıza temel form doğrulaması ekledik. Bizim Doğrulama mantığı, kullanıcıların yeni kişi gönderme veya var olan bir kişi FirstName ve LastName özellikleri için değerler sağlamadan düzenleme engeller. Ayrıca, kullanıcıların geçerli bir telefon numaraları ve e-posta adreslerini sağlamanız gerekir.

Bu yineleme, doğrulama mantığı Kişi Yöneticisi uygulamamıza kolay yolu ekledik. Ancak, bizim Doğrulama mantığı bizim denetleyicisi mantığı eklemenize karıştırma sorunları bizim için uzun vadede oluşturacaksınız. Uygulamamızı Bakım ve değişiklik zaman içinde daha zor olacaktır.

Bir sonraki yinelemede biz bizim Doğrulama mantığı ve veritabanına erişim mantığı bizim denetleyicileri dışında yeniden düzenleme. Biz, bize daha fazla gevşek bağlantılı ve daha sürdürülebilir bir uygulama oluşturmak etkinleştirmek için çeşitli yazılım tasarım ilkeleri avantajlarından yararlanmak.

> [!div class="step-by-step"]
> [Önceki](iteration-2-make-the-application-look-nice-cs.md)
> [İleri](iteration-4-make-the-application-loosely-coupled-cs.md)
