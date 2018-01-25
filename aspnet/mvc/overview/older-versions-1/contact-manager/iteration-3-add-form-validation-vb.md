---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: "Yineleme #3 – Ekle form doğrulama (VB) | Microsoft Docs"
author: microsoft
description: "Üçüncü yinelemede temel form doğrulama ekleyin. Biz, kişilerin gerekli form alanları tamamlamadan bir form gönderme engelleyin. Biz de emai doğrula..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: e9ed182fb58addd8c5dadbe6e3d09c391840ca00
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="iteration-3--add-form-validation-vb"></a>Yineleme #3 – Ekle form doğrulama (VB)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indirme](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Üçüncü yinelemede temel form doğrulama ekleyin. Biz, kişilerin gerekli form alanları tamamlamadan bir form gönderme engelleyin. Biz de e-posta adresleri ve telefon numaralarını doğrulayın.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetim ASP.NET MVC uygulaması (VB) oluşturma
  

Bu öğretici serisinde tamamlamak için tüm kişi yönetim uygulamanın başından oluşturun. Kişi Yöneticisi uygulama kişi listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamak sağlar.

Biz uygulamayı birden çok kez oluşturun. Her bir yineleme, biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı, her değişiklik nedeni anlamak etkinleştirmek için hedefidir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Contact Manager en basit yolu olası oluşturuyoruz. Temel veritabanı işlemleri için destek eklediğimiz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - iyi Ara uygulama olun. Bu yinelemede biz ana görünüm sayfası, ASP.NET MVC varsayılan değiştirme ve geçişli stil sayfası uygulama görünümünü geliştirir.

- Yineleme #3 - form doğrulama ekleyin. Üçüncü yinelemede temel form doğrulama ekleyin. Biz, kişilerin gerekli form alanları tamamlamadan bir form gönderme engelleyin. Biz de e-posta adresleri ve telefon numaralarını doğrulayın.

- Yineleme #4 - gevşek uygulama olun. Bu üçüncü yinelemede biz Bakım ve ilgili kişi Yöneticisi uygulaması değişiklik kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, uygulamamız havuz deseni ve bağımlılık ekleme düzeni kullanmak üzere yeniden.

- Yineleme #5 - birim testleri oluşturma. Beşinci yinelemede biz uygulamamız korumak ve birim testleri ekleyerek değiştirmek kolaylaştırır. Biz bizim veri modeli sınıflarını mock ve bizim denetleyicileri ve Doğrulama mantığı için birim testleri oluşturma.

- Yineleme #6 - teste dayalı geliştirme kullanın. Bu altıncı yinelemede yeni işlevsellik uygulamamız için birim testleri ilk yazma ve birim testleri karşı kod yazma ekleriz. Bu yinelemede kişi grupları ekleyin.

- Yineleme #7 - Ajax işlevselliği ekleyin. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.


## <a name="this-iteration"></a>Bu yineleme

Bu ikinci yinelemede Contact Manager uygulamanın, temel form doğrulama ekleyin. Biz, kişilerin kişi gerekli form alanları için değerleri girmeden göndermelerini engellemek. Biz de telefon numarası ve e-posta adresleri (bkz: Şekil 1) doğrulayın.


[![Yeni Proje iletişim kutusu](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Şekil 01**: doğrulama formuyla ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-3-add-form-validation-vb/_static/image2.png))


Bu yinelemede doğrudan denetleyici eylemleri için doğrulama mantığını ekleriz. Genel olarak, bu doğrulama ASP.NET MVC uygulama eklemek için önerilen yöntem değildir. Daha iyi bir yaklaşım ayrı bir uygulama s doğrulama mantığını yerleştirmektir [hizmet katmanı](http://martinfowler.com/eaaCatalog/serviceLayer.html). Sonraki yinelemede biz uygulamayı daha rahat yapmak için ilgili kişi Yöneticisi uygulaması yeniden düzenleyin.

Bu yinelemede şeyler basit tutmak için size tüm doğrulama kodu el ile yazmak. Doğrulama kodu kendisini yazmak yerine, biz doğrulama çerçevesinden ele geçirebilir. Örneğin, ASP.NET MVC uygulamanız için doğrulama mantığını uygulamak için Microsoft Kurumsal kitaplığı doğrulama uygulama blok (VAB) kullanabilirsiniz. Doğrulama uygulama bloğu hakkında daha fazla bilgi için bkz:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Oluştur görünümünün doğrulama ekleme

Oluştur görünümünün doğrulama mantığını ekleyerek başlayın s olanak tanır. Biz Oluştur görünümünün Visual Studio ile oluşturulmuş için Neyse ki, Oluştur görünümünün zaten tüm doğrulama iletileri görüntülemek için gerekli kullanıcı arabirimi mantığı içerir. Oluştur görünümünün listeleme 1'de yer alır.

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

Alan doğrulama hatası sınıf Html.ValidationMessage() Yardımcısı tarafından oluşturulan çıkış stilini belirlemek için kullanılır. Giriş doğrulama hatası sınıf Html.TextBox() Yardımcısı tarafından işlenen metin (giriş) stilini belirlemek için kullanılır. Doğrulama Özeti hataları sınıf Html.ValidationSummary() Yardımcısı tarafından işlenen sırasız liste stilini belirlemek için kullanılır.

> [!NOTE] 
> 
> Doğrulama hatası iletilerinin görünümünü özelleştirmek için bu bölümde açıklanan stil sayfası sınıfları değiştirebilirsiniz.


## <a name="adding-validation-logic-to-the-create-action"></a>Doğrulama mantığını ekleme eylem oluşturun

Şu anda, size herhangi bir ileti oluşturmak için mantığı yazmıştır değil çünkü Oluştur görünümünün hiçbir zaman bir doğrulama hata iletisi görüntüler. Doğrulama hata iletilerini görüntülemek için hata iletileri için ModelState eklemeniz gerekir.

> [!NOTE] 
> 
> UpdateModel() yöntemi hata iletileri ModelState için bir özellik için bir form alanının değerini atanırken bir hata olduğunda otomatik olarak ekler. "Apple" dizesi DateTime değerleri kabul eden bir doğum tarihi özellik atama çalışırsanız, örneğin, ardından UpdateModel() yöntemi bir hata ModelState için ekler.


Değiştirilen Create() yöntemi listeleme 2'deki yeni kişi veritabanına eklenen önce ilgili kişi sınıf özelliklerini doğrulayan yeni bir bölüm içerir.

**2 - Controllers\ContactController.vb (Oluştur doğrulama ile) listeleme**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

Doğrula bölümüne dört farklı doğrulama kuralları uygular:

- FirstName özelliği bir uzunluk sıfırdan büyük olması gerekir (ve boşluklardan oluşamaz)
- Soyadı özelliği bir uzunluk sıfırdan büyük olması gerekir (ve boşluklardan oluşamaz)
- Telefon özellik değeri varsa (bir uzunluğu 0'dan büyük olan) telefon özellik normal ifadesiyle eşleşmelidir sonra.
- E-posta özelliği değeri varsa (bir uzunluğu 0'dan büyük olan) sonra e-posta özelliği normal bir ifade eşleşmesi gerekir.

Bir doğrulama kuralı ihlali olduğunda, bir hata iletisi ModelState için AddModelError() yöntemi yardımıyla eklenir. ModelState için bir ileti eklediğinizde, özelliğin adını ve bir doğrulama hata iletisinin metnini sağlar. Bu hata iletisini Html.ValidationSummary() ve Html.ValidationMessage() yardımcı yöntemler tarafından görünümünde görüntülenir.

Doğrulama kuralları yürütüldükten sonra ModelState IsValid özelliğinin denetlenir. ModelState için herhangi bir doğrulama hata iletisi eklendiğinde IsValid özelliği false değerini döndürür. Doğrulama başarısız olursa, form oluştur hata iletileri ile yeniden görüntülenir.

> [!NOTE] 
> 
> Sırasında normal ifade depodan telefon numarası ve e-posta adresini doğrulamak için normal ifadeler var [ *http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Doğrulama mantığını düzenleme eyleme ekleme

Edit() eylem kişi güncelleştirir. Tam olarak aynı doğrulama Create() eylem olarak gerçekleştirmek Edit() eylem gerekiyor. Böylece Create() ve Edit() eylemi aynı doğrulama yöntemini çağırma aynı doğrulama kodu çoğaltmak yerine biz kişi denetleyicisi yeniden düzenlemeniz gerekir.

Değiştirilen kişi denetleyici sınıfı listeleme 3'te yer alır. Bu sınıf içinde Create() ve Edit() Eylemler adlı yeni bir ValidateContact() yöntemi vardır.

**3 - Controllers\ContactController.vb listeleme**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Özet

Bu yinelemede Contact Manager uygulamamız temel form doğrulama eklediğimiz. Doğrulama mantığımızı kullanıcıların yeni bir kişi gönderme veya varolan bir kişinin adı ve Soyadı özelliklerine ilişkin değerleri girmeden düzenleme engeller. Ayrıca, kullanıcıların geçerli bir telefon numaraları ve e-posta adresi girmeniz gerekir.

Bu yinelemede doğrulama mantığını Contact Manager uygulamamız en kolay yolu eklediğimiz. Ancak, doğrulama mantığımızı denetleyicisi mantığımızı karıştırma sorunları bize için uzun vadede oluşturur. Uygulamamız Bakım ve zaman içinde değişiklik daha zor olacaktır.

Sonraki yinelemede biz bizim doğrulama mantığını ve veritabanı erişim mantığı dışı bizim denetleyicileri yeniden düzenlemeniz. Biz bize daha geniş eşleşmiş ve daha rahat bir uygulama oluşturmak etkinleştirmek için birkaç yazılım tasarım ilkeleri yararlanmak.

>[!div class="step-by-step"]
[Önceki](iteration-2-make-the-application-look-nice-vb.md)
[sonraki](iteration-4-make-the-application-loosely-coupled-vb.md)
