---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Yineleme #3 – form doğrulaması ekleme (VB) | Microsoft Docs'
author: microsoft
description: Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de emai doğrulayın...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d4a1806742caf13f4778ef3588710136814c993
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368725"
---
<a name="iteration-3--add-form-validation-vb"></a>Yineleme #3 – form doğrulaması ekleme (VB)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetimi ASP.NET MVC uygulama (VB)
  

Bu öğretici serisinde, tamamlanması bir tüm kişi yönetimi uygulaması ekleriz. Kişi Yöneticisi uygulama kişilerin bir listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamanızı sağlar.

Birden çok yineleme üzerinde uygulama ekleriz. Her yineleme ile biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı amacı, her değişikliğin nedenini anlamak etkinleştirmektir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - uygulamanın güzel görünmesini olun. Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.

- Yineleme #3 - form doğrulaması ekleme. Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.

- Yineleme #4 - birbirine sıkı şekilde bağlı uygulama olun. Bu üçüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.

- Yineleme #5 - birim testleri oluşturun. Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.

- Yineleme #6 - test odaklı geliştirme kullanma. Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.

- Yineleme #7 - Ajax işlevselliği ekleme. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.


## <a name="this-iteration"></a>Bu yineleme

Kişi Yöneticisi uygulama bu ikinci yinelenmesinde temel form doğrulaması ekleriz. Biz, kişilerin kişi gerekli form alanları için değerler girmeden göndermesinin önlenmesine. Biz de telefon numarası ve e-posta adresleri (bkz. Şekil 1) doğrulayın.


[![Yeni Proje iletişim kutusu](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Şekil 01**: doğrulama formuyla ([tam boyutlu görüntüyü görmek için tıklatın](iteration-3-add-form-validation-vb/_static/image2.png))


Bu yineleme, doğrudan denetleyici eylemleri için doğrulama mantığını ekleriz. Genel olarak, bu doğrulama eklemek için bir ASP.NET MVC uygulaması için önerilen yöntem değildir. Bir uygulama s Doğrulama mantığı ayrı bir yerleştirmek için daha iyi bir yaklaşım olan [hizmet katmanı](http://martinfowler.com/eaaCatalog/serviceLayer.html). Bir sonraki yinelemede biz uygulamanın daha sürdürülebilir hale getirmek için Kişi Yöneticisi uygulama yeniden düzenleyin.

Bu yineleme, basit bir anlatım gözetildiği için biz tüm doğrulama kodları el ile yazın. Doğrulama kodu kendimize yazmak yerine, doğrulama çerçevesinin avantajlarından sürebilir. Örneğin, ASP.NET MVC uygulamanızın Doğrulama mantığı uygulamak için Microsoft Kurumsal kitaplığı doğrulama uygulama blok (VAB) kullanabilirsiniz. Doğrulama uygulama bloğu hakkında daha fazla bilgi için bkz:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Doğrulama oluşturma görünümü ekleme

Oluştur görünümünün için doğrulama mantığını ekleyerek başlayın s olanak tanır. Biz Oluştur görünümünün Visual Studio ile oluşturulmuş için Neyse ki, Oluştur görünümünün zaten tüm doğrulama iletileri görüntülemek için gerekli kullanıcı arabirimi mantığı içerir. Oluştur görünümünün listeleme 1'de yer alır.

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

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

**2 - Controllers\ContactController.vb (doğrulama ile Oluştur) listeleme**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

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

**3 - Controllers\ContactController.vb listeleme**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Özet

Bu yineleme, kişi yöneticisi uygulamamıza temel form doğrulaması ekledik. Bizim Doğrulama mantığı, kullanıcıların yeni kişi gönderme veya var olan bir kişi FirstName ve LastName özellikleri için değerler sağlamadan düzenleme engeller. Ayrıca, kullanıcıların geçerli bir telefon numaraları ve e-posta adreslerini sağlamanız gerekir.

Bu yineleme, doğrulama mantığı Kişi Yöneticisi uygulamamıza kolay yolu ekledik. Ancak, bizim Doğrulama mantığı bizim denetleyicisi mantığı eklemenize karıştırma sorunları bizim için uzun vadede oluşturacaksınız. Uygulamamızı Bakım ve değişiklik zaman içinde daha zor olacaktır.

Bir sonraki yinelemede biz bizim Doğrulama mantığı ve veritabanına erişim mantığı bizim denetleyicileri dışında yeniden düzenleme. Biz, bize daha fazla gevşek bağlantılı ve daha sürdürülebilir bir uygulama oluşturmak etkinleştirmek için çeşitli yazılım tasarım ilkeleri avantajlarından yararlanmak.

> [!div class="step-by-step"]
> [Önceki](iteration-2-make-the-application-look-nice-vb.md)
> [İleri](iteration-4-make-the-application-loosely-coupled-vb.md)
