---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: '9. Bölüm: Kayıt ve kullanıma alma | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 9. Bölüm kayıt ve kasa işlemleri kapsar.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812879"
---
<a name="part-9-registration-and-checkout"></a>9. Bölüm: Kayıt ve kasa işlemleri
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 9. Bölüm kayıt ve kasa işlemleri kapsar.


Bu bölümde, size, platformumuz'ın adresini ve ödeme bilgilerini toplarız CheckoutController oluşturma. Biz sitemizi kullanıma, önce bu denetleyici yetkilendirme gerektirecek şekilde kaydedin açmasına gerektirir.

Kullanıcıların kullanıma alma işlemi için bunların alışveriş sepetini "Kullanıma Al" düğmesine tıklayarak gidin.

![](mvc-music-store-part-9/_static/image1.jpg)

Kullanıcı oturum açmadığı, için istenir.

![](mvc-music-store-part-9/_static/image1.png)

Başarıyla oturum açtıktan sonra kullanıcı ardından adresinizi ve ödeme görünümü gösterilir.

![](mvc-music-store-part-9/_static/image2.png)

Formu doldurduğunuzda varsa ve siparişi gönderen sonra siparişi onay ekranında gösterilir.

![](mvc-music-store-part-9/_static/image3.png)

Mevcut olmayan bir sipariş ya da size ait olmayan bir sırada görüntülemeye çalıştığınızda hata görünümü gösterilir.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Alışveriş sepetini geçirme

Kullanıcı kullanıma alma düğmeye tıkladığında bir alışveriş işlemi anonim, olsa da, bunlar kaydetme gerekecektir ve oturum açın. Kullanıcıların, alışveriş sepeti bilgilerine ziyaretleri kayıt veya oturum açma tamamlandığında, alışveriş sepeti bilgileri bir kullanıcıyla ilişkilendirmek ihtiyacımız şekilde korur beklediğiniz.

Bizim ShoppingCart sınıfı, geçerli Sepeti tüm öğeleri bir kullanıcı adı ile ilişkilendireceğiniz bir yöntem olduğundan Bunu yapmak gerçekten çok kolaydır. Biz yalnızca bir kullanıcı kaydı veya oturum açma tamamlandığında bu yöntemi çağırmanız gerekir.

Açık **AccountController** biz üyelik ve yetkilendirme ayarlama yükleyen ekledik sınıfı. Ekleme bir using deyimi başvuran MvcMusicStore.Models, ardından aşağıdaki MigrateShoppingCart yöntemi ekleyin:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Ardından, kullanıcının kimliği doğrulandıktan sonra aşağıda gösterildiği gibi MigrateShoppingCart çağırmak için oturum açma sonrası eylemi değiştirin:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Kullanıcı hesabı başarıyla oluşturulduktan hemen sonra eylem sonrası aynı kasaya değişiklik:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Bunu - olan artık anonim bir alışveriş sepeti başarılı kayıt veya oturum açma sırasında kullanıcı hesabına otomatik olarak aktarılır.

## <a name="creating-the-checkoutcontroller"></a>CheckoutController oluşturma

Denetleyicileri klasörü sağ tıklatın ve boş denetleyici şablonu kullanarak CheckoutController adlı projeye yeni bir denetleyici ekleyeceksiniz.

![](mvc-music-store-part-9/_static/image5.png)

İlk olarak, kullanıma alma önce kaydolmalarını iste için denetleyici sınıf bildiriminin üstüne Authorize özniteliği ekleyin:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Not: Bu StoreManagerController için daha önce yaptığımız değişiklik benzer, ancak bu durumda kullanıcı bir yönetici rolünde olmanız Authorize özniteliği gerekli. Kullanıma alma denetleyicide, kullanıcının oturum açmış olmanız biz gerektiren ancak yöneticileri olmaları zorunlu değildir.*

Basitleştirmek amacıyla, biz Bu öğreticide ödeme bilgilerle uğraşıyor olmaz. Bunun yerine, biz kullanıcıların bir promosyon kodu kullanarak kullanıma izin vermiş olursunuz. Bu promosyon kodu PromoCode adlı bir sabit kullanarak depolarız.

StoreController olduğu gibi biz storeDB adlı MusicStoreEntities sınıfının bir örneğini tutacak bir alan bildirmeniz. Yapmak için MusicStoreEntities sınıfı kullanın, biz kullanarak bir eklemeniz gerekecektir MvcMusicStore.Models ad alanı bildirimi. Kullanıma alma denetleyicimizin en altında görünür.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Şu denetleyici eylemleri CheckoutController olacaktır:

**AddressAndPayment (GET method)** bilgilerini girmesini izin vermek için bir form görüntüler.

**AddressAndPayment (POST yöntemi)** girişini doğrulamak ve siparişi işleme.

**Tam** kullanıcı kullanıma alma işlemi başarıyla tamamlandıktan sonra gösterilir. Bu görünüm, kullanıcının sipariş numarası da onay olarak dahil edilir.

İlk olarak, şimdi AddressAndPayment için (denetleyici oluşturduğumuz bağlandığınızda oluşturuldu) dizin denetleyici eylemini yeniden adlandırın. Herhangi bir model bilgi gerektirmez, dolayısıyla bu denetleyici eylemi yalnızca kullanıma alma form görüntüler.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Bizim AddressAndPayment POST yöntemi içinde StoreManagerController kullandık aynı düzeni izler: form gönderme kabul edin ve sırasını tamamlamak deneyecek ve başarısız olursa formu yeniden görüntüleyin.

Form girişini doğrulama sipariş bizim doğrulama gereksinimlerini karşılayan sonra biz doğrudan PromoCode form değeri kontrol eder. Her şeyin doğru olduğunu varsayarsak, biz güncelleştirilmiş bilgileri siparişle kaydedebilir, sipariş işlemini tamamlayıp tamamlama eyleminde olduğu için yeniden yönlendirme ShoppingCart söylemek.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Kullanıma alma işlemi işlemin başarıyla tamamlanmasından sonra kullanıcılar için eksiksiz bir denetleyici eylemini yönlendirilirsiniz. Bu eylem sırasını gerçekten oturum açmış kullanıcıya bir onay olarak sipariş numarası göstermeden önce ait olduğunu doğrulamak için basit bir denetim gerçekleştirir.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Not: şu projenin başladığında hata görünümünün otomatik olarak bizim için /Views/Shared klasöründe oluşturulur.*

Tam CheckoutController kod aşağıdaki gibidir:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>AddressAndPayment görünüm ekleme

Şimdi, AddressAndPayment görünüm oluşturalım. AddressAndPayment denetleyici eylemleri birine sağ tıklayın ve aşağıda gösterildiği gibi bir sırada kesin ve düzenleme şablonu kullanıldığı AddressAndPayment adlı bir görünüm ekleyin.

![](mvc-music-store-part-9/_static/image6.png)

Bu görünüm hale getirecek iki incelemiştik adresindeki StoreManagerEdit görünümü oluştururken tekniklerden birini kullanın:

- Sipariş modeli için form alanlarını görüntülemek için Html.EditorForModel() kullanacağız
- Bir sipariş sınıfı ile doğrulama öznitelikleri kullanarak doğrulama kuralları yararlanılacaktır.

Html.EditorForModel(), ek bir textbox tarafından izlenen promosyon kodunu kullanmak için form kodu güncelleştirerek başlayacağız. AddressAndPayment görünümü için tam kod aşağıda gösterilmiştir.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Sipariş için doğrulama kurallarını tanımlama

Bizim görünümü ayarlamak, albüm modeli için daha önce yaptığımız gibi doğrulama kuralları sipariş modelimizi ayarlayacağız. Modeller klasörü sağ tıklatın ve sipariş adlı bir sınıf ekleyin. Albüm için daha önce kullandığımız doğrulama özniteliklerinin yanı sıra, biz de normal bir ifade kullanıcının e-posta adresinizi doğrulamak için kullanır.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Çalışılıyor formunun eksik olan veya geçersiz bilgileri artık kullanarak istemci tarafı doğrulama hata iletisi gösterir.

![](mvc-music-store-part-9/_static/image7.png)

Tamam, çoğunu sabit kullanıma alma işlemi için uyguladığımız güncelleştirmede; yalnızca birkaç ekledikçe ve tamamlanması uçları sahibiz. İki basit görünüm eklemek ihtiyacımız ve oturum açma işlemi sırasında sepet bilgi iletimi ölçeklendirilmesini gerekiyor.

## <a name="adding-the-checkout-complete-view"></a>Kullanıma alma tam görünüm ekleme

Yalnızca sipariş kimliği görüntülenecek gereksinimleriniz değiştikçe kullanıma alma tam görünümü oldukça basit Tam denetleyici eylemini sağ tıklayıp, bir tamsayı kesin tam olarak adlandırılan bir görünümü Ekle

![](mvc-music-store-part-9/_static/image8.png)

Aşağıda gösterildiği gibi sipariş kimliği görüntülenecek görünüm kodu artık güncelleştireceğiz.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Hatanın görünüm güncelleştiriliyor

Böylece, başka bir site için yeniden kullanılabilir varsayılan şablonu paylaşılan görünümler klasöründe bir hata görünümü içerir. Bu hata görünüm çok basit bir hata içeriyor ve bu güncelleştireceğiz şekilde sitemizi düzeni kullanmaz.

Bu genel bir hata sayfası olduğundan, içeriği çok kolaydır. Bir ileti ve bir bağlantı kullanıcı kendi eylemini yeniden denemek isterse geçmişinde önceki sayfaya gitmek için yer alacak.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-8.md)
> [İleri](mvc-music-store-part-10.md)
