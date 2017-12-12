---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "9. Kısım: Kayıt ve Checkout | Microsoft Docs"
author: jongalloway
description: "Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 9 kayıt ve Checkout kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 799985f889b1063c53df7bce365ae3989809ba79
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-9-registration-and-checkout"></a>9. Kısım: Kayıt ve kullanıma alma
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 9 kayıt ve Checkout kapsar.


Bu bölümde, biz alışveriş yapanların adresini ve ödeme bilgilerini toplayan bir CheckoutController oluşturmaya olursunuz. Biz bu denetleyici yetkilendirme gerektirecek şekilde kullanıma, önce sitemizi ile kaydetmek kullanıcıların gerektirir.

Kullanıcılar için kullanıma alma işlemi, kendi Alışveriş sepetinden "Checkout" düğmesine tıklayarak gidin.

![](mvc-music-store-part-9/_static/image1.jpg)

Kullanıcı günlüğe kaydedilmezse için istenir.

![](mvc-music-store-part-9/_static/image1.png)

Oturum açma başarılı olduğunda, kullanıcı ardından adresinizi ve ödeme görünümü gösterilir.

![](mvc-music-store-part-9/_static/image2.png)

Formun doldurulmuş varsa ve sırasını gönderilen sonra sipariş onay ekranında gösterilir.

![](mvc-music-store-part-9/_static/image3.png)

Mevcut olmayan sipariş veya size ait olmayan bir sırada görüntüleme girişiminde hata görünümü gösterilir.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Alışveriş sepeti geçirme

Kullanıcı Checkout düğmesine tıkladığında bir alışveriş işlemi anonim, olsa da, bunlar kaydetmeniz gerekir ve oturum açma. Kullanıcıların kendi alışveriş sepeti bilgilerini ziyaretleri kayıt veya oturum açma tamamladığınızda alışveriş sepeti bilgilerin bir kullanıcıyla ilişkilendirmek ihtiyacımız şekilde korur beklediğiniz.

Bizim ShoppingCart sınıfı, geçerli sepetteki tüm öğeleri bir kullanıcı adı ile ilişkilendireceğiniz bir yöntem taşıdığından Bunu yapmak gerçekten çok kolaydır. Biz yalnızca bir kullanıcı kaydı veya oturum açma işlemi tamamlandığında bu yöntemi çağırmanız gerekir.

Açık **AccountController** biz üyeliği ve yetkilendirme ayarlama yükleyen eklediğimiz sınıfı. Ekleme bir MvcMusicStore.Models, başvuran deyimiyle sonra aşağıdaki MigrateShoppingCart yöntemi ekleyin:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Ardından, kullanıcı doğrulandıktan sonra aşağıda gösterildiği gibi MigrateShoppingCart çağırmak için oturum açma sonrası eylemi değiştirin:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Kullanıcı hesabı başarıyla oluşturulduktan hemen sonra eylem sonrası aynı kasaya değişiklik:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Anonim bir alışveriş sepeti başarılı kaydı veya oturum açma sırasında bir kullanıcı hesabına otomatik olarak aktarılacaktır şimdi onu - olur.

## <a name="creating-the-checkoutcontroller"></a>CheckoutController oluşturma

Denetleyicileri klasörü sağ tıklatın ve yeni bir denetleyici boş denetleyicisi şablonu kullanarak CheckoutController adlı projeye ekleyin.

![](mvc-music-store-part-9/_static/image5.png)

İlk olarak, Authorize öznitelik checkout önce kaydetmek kullanıcıların denetleyicisi sınıf bildiriminin üstüne ekleyin:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Not: Bu biz StoreManagerController daha önce yaptığınız değişiklikleri benzer, ancak bu durumda kullanıcının yönetici rolünde olması Authorize özniteliği gerekli. Checkout denetleyicisi, kullanıcı oturum açmış olmanız biz gerektiren ancak yöneticileri olmaları gerektiren değil.*

Basitleştirmek amacıyla, biz Bu öğreticide ödeme bilgilerle ilgilenen olmaz. Bunun yerine, biz kullanıcıların bir promosyon kodu kullanarak kullanıma izin vermiş olursunuz. Bu promosyon kodu PromoCode adlı bir sabit kullanarak depolarız.

StoreController olduğu gibi biz storeDB adlı MusicStoreEntities sınıfının bir örneği tutmak için bir alan belirtmesi. Yapmak için MusicStoreEntities sınıfını kullanmak, biz kullanarak bir eklemeniz gerekir MvcMusicStore.Models ad alanı bildirimi. Bizim Checkout denetleyicisi üstündeki aşağıda yer almaktadır.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController aşağıdaki denetleyici eylemleri olacaktır:

**AddressAndPayment (GET method)** bilgilerini girmesini izin vermek için bir form görüntüleyecek.

**AddressAndPayment (POST yöntemini)** giriş doğrulamak ve siparişi işleme.

**Tam** bir kullanıcı teslim alma işlemi başarıyla tamamlandıktan sonra gösterilir. Bu görünüm kullanıcının sipariş numarası, onay içerir.

İlk olarak, şimdi (biz denetleyicisini oluşturduğunuzda oluşturulan) dizin denetleyici eylemi için AddressAndPayment yeniden adlandırın. Bu denetleyici eylemi yalnızca checkout formun görüntüler, böylece herhangi bir model bilgi gerektirmez.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Bizim AddressAndPayment POST yöntemini StoreManagerController kullandık aynı düzeni izler: form gönderme kabul etmek ve sırasını tamamlamak deneyecek ve başarısız olursa formu yeniden görüntüleyin.

Form girişi doğrulama sipariş bizim doğrulama gereksinimlerini karşılayan sonra biz PromoCode form değer doğrudan denetler. Her şeyi güncelleştirilmiş bilgileri siparişle kaydedecek doğru olduğunu varsayarak, sipariş sürecini tamamlayıp tam eyleme yönlendirmek için ShoppingCart nesne söyleyin.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Kullanıma alma işlemi başarıyla tamamlandıktan sonra kullanıcılar için tam denetleyici eylemi yönlendirilir. Bu eylem, sipariş gerçekten oturum açmış kullanıcıya bir onay olarak sipariş numarası gösterilmeden önce ait olduğunu doğrulamak için basit bir denetim gerçekleştirir.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Not: biz proje başladığındaki hata Görünüm otomatik olarak bize /Views/Shared klasöründe oluşturuldu.*

Tam CheckoutController kod aşağıdaki gibidir:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>AddressAndPayment görünümü ekleme

Şimdi, AddressAndPayment görünüm oluşturalım. Aşağıdakilerden birini sağ AddressAndPayment denetleyici eylemleri ve aşağıda gösterildiği gibi bir sırada kesin türü belirtilmiş ve Düzen şablonunu kullanan AddressAndPayment adlı bir görünüm ekleyin.

![](mvc-music-store-part-9/_static/image6.png)

Bu görünüm yapacak iki biz Aranan adresindeki StoreManagerEdit görünüm oluşturulurken yöntemlerden birini kullanın:

- Html.EditorForModel() sipariş modeli için form alanlarını görüntülemek için kullanacağız
- Doğrulama kuralları ile doğrulama öznitelikleri bir sipariş sınıfı kullanarak özelliğinden yararlanır

Promosyon kodu için ek bir metin kutusu tarafından izlenen Html.EditorForModel() kullanmak için form kodu güncelleştirerek başlayacağız. AddressAndPayment görünüm için tam kod aşağıda verilmiştir.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Sipariş için doğrulama kurallarını tanımlama

Bizim görünüm ayarlamak, albüm modeli için daha önce yaptığımız gibi biz doğrulama kurallarını sipariş modelimiz için ayarlar. Modeller klasörü sağ tıklatın ve sipariş adlı bir sınıf ekleyin. Daha önce albüm için kullandık doğrulama özniteliklerinin yanı sıra, biz de normal bir ifade kullanıcının e-posta adresini doğrulamak için kullanır.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Çalışılıyor eksik formu göndermeden veya geçersiz bilgiler şimdi istemci tarafı doğrulama kullanırken hata iletisi gösterir.

![](mvc-music-store-part-9/_static/image7.png)

Tamam, biz kullanıma alma işlemi için sabit işlerin çoğunu yaptıktan; Biz yalnızca birkaç büyük olasılıkla ve tamamlanması uçları sahip. İki basit görünüm eklemek ihtiyacımız ve oturum açma işlemi sırasında Sepeti bilgi iletimi ilişkin halletmeniz gerekir.

## <a name="adding-the-checkout-complete-view"></a>Checkout tam görünümü ekleme

Yalnızca sipariş kimliği görüntülenecek gereksinimleriniz değiştikçe Checkout tam görünümü oldukça basit bir işlemdir Tam denetleyici eylemini sağ tıklayın ve bir tamsayı kesin türü belirtilmiş tam adlı bir görünüm ekleyin

![](mvc-music-store-part-9/_static/image8.png)

Aşağıda gösterildiği gibi sipariş kimliği, görüntülemek için görünümü kodu şimdi güncelleştireceğiz.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Hatanın görünüm güncelleştiriliyor

Varsayılan şablonu, bir hata görünümü paylaşılan görünümler klasöründe içerir, başka bir sitede yeniden kullanılabilir. Bu hata görünümü çok basit bir hata içerir ve biz güncelleştireceğim şekilde sitemizi düzeni kullanmaz.

Bu genel bir hata sayfası olduğundan, içeriği çok kolaydır. Bir ileti ve bağlantı kullanıcı kendi eylem yeniden denemek isterse geçmişi önceki sayfaya gitmek için eklediğimiz.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
[Önceki](mvc-music-store-part-8.md)
[sonraki](mvc-music-store-part-10.md)
