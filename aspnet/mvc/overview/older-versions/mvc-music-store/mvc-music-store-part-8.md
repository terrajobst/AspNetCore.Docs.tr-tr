---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: '8. Bölüm: Ajax güncelleştirmeleriyle alışveriş | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 8. Bölüm Ajax güncelleştirmeleriyle alışveriş sepeti kapsar.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 881d47b5b4df5a4d310a1b3a7eec6ee97b0d42ea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823845"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>8. Bölüm: Ajax güncelleştirmeleriyle alışveriş sepeti
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 8. Bölüm Ajax güncelleştirmeleriyle alışveriş sepeti kapsar.


Kayıt olmadan kendi arabasında albümleri yerleştirmek kullanıcılar izin veriyoruz, ancak bunlar tam kullanıma Konukları olarak kaydetmeniz gerekir. Alışveriş ve kullanıma alma işlemi iki denetleyicilerine ayrılacaktır: anonim olarak öğe için bir sepet ekleme izin veren bir ShoppingCart denetleyicisi ve kullanıma alma işlemi yürüten bir kullanıma alma denetleyicisi. Biz bu bölümdeki alışveriş sepetine ile başlayın, ardından derleme aşağıdaki bölümünde kullanıma alma işlemi.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Sepet, sırası ve OrderDetail model sınıfları ekleme

Bizim alışveriş sepeti ve kullanıma alma işlemleri yapar bazı yeni sınıflarını kullanın. Modeller klasörü sağ tıklatın ve aşağıdaki kod ile sepet sınıfı (Cart.cs) ekleyin.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Bu sınıf ana kadar [anahtarı] özniteliği için kayıt kimliği özelliği dışında kullandığımız başkalarına oldukça benzerdir. Bizim sepet öğeleri anonim alışveriş izin vermek için CartID adlı bir dize tanımlayıcısına sahip olacaktır, ancak tablo RecordID adlı bir tamsayı birincil anahtarı içerir. Kural gereği, Entity Framework Code-First, sepet adlı bir tablo için birincil anahtarı CartId veya kimliği olacaktır, ancak biz istiyorsanız biz kolayca, ek açıklamalar veya kod yoluyla kılabilirsiniz bekliyor. Bu bir örnek bize uygun olduğunda basit kuralları Entity Framework Code-First nasıl kullanabileceğimizi ancak sunulmuyorsa, biz bunları tarafından kısıtlanmış değil.

Ardından, bir sipariş sınıfı (Order.cs) ile aşağıdaki kodu ekleyin.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Bu sınıf, bir sipariş için Özet ve teslimat bilgilerini izler. **Henüz derlenemeyecektir**, henüz oluşturduğumuz henüz bir sınıfa bağlıdır bir OrderDetails gezinti özelliği mevcuttur. Şimdi artık ekleyerek bir sınıf OrderDetail.cs, aşağıdaki kodu ekleyerek adlandırdığınız düzeltelim.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Ayrıca bir olan DB dahil olmak üzere bu yeni Model sınıfları gösterdiğiniz DbSets içerecek şekilde bizim MusicStoreEntities sınıfı bir son güncelleştirme oluşturacağız&lt;sanatçının&gt;. Güncelleştirilmiş MusicStoreEntities sınıfı olarak görünür aşağıda.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Alışveriş sepeti iş mantığı yönetme

Ardından, modelleri klasöründe ShoppingCart sınıfı oluşturacağız. Sepet tablosuna veri erişimi ShoppingCart modeli işler. Buna ek olarak, ekleme ve alışveriş sepetine öğeleri kaldırma iş mantığı işleyeceği.

Yalnızca öğe, alışveriş sepetine eklemek bir hesap için kaydolun gerektirmek istemiyorsanız bu yana, biz kullanıcıların geçici bir benzersiz tanımlayıcı (GUID veya genel olarak benzersiz tanımlayıcısı kullanılarak) atar alışveriş sepetini eriştiklerinde. ASP.NET oturum sınıfını kullanarak bu kimliği depolarız.

*Not: ASP.NET oturum bunlar site ayrıldıktan sonra süresi dolacak kullanıcıya özgü bilgileri depolamak için kullanışlı bir yerdir. Oturum durumu b.internet performans etkilerinin daha büyük sitelerinde olabilse de tanıtım amacıyla ışık kullanma çalışır.*

ShoppingCart sınıfı aşağıdaki yöntemi kullanıma sunar:

**AddToCart** albüm bir parametre olarak alır ve kullanıcının sepetine ekler. Sepet tablonun her albüm miktarını izler olduğundan, gerekirse yeni bir satır oluşturun veya kullanıcı zaten bir kopyasını albümü sipariş yalnızca miktarını artırmak için mantığı içerir.

**RemoveFromCart** albüm kimliği alır ve kullanıcının sepetinden kaldırır. Kullanıcı kendi arabasında yalnızca bir kopyasını albümü olsaydı, satır kaldırılır.

**EmptyCart** kullanıcının alışveriş sepetini tüm öğeleri kaldırır.

**GetCartItems** görüntüleme veya işleme için CartItems listesini alır.

**GetCount** alır bir albümleri bir kullanıcının sahip, alışveriş sepetine içinde toplam sayısı.

**GetTotal** sepetteki tüm öğelerin toplam maliyeti hesaplar.

**CreateOrder** alışveriş sepetini sipariş kullanıma alma aşamasında dönüştürür.

**GetCart** Sepeti nesne elde etmek üzere bizim denetleyicileri sağlayan statik bir yöntemdir. Kullandığı **GetCartId** kullanıcının oturumunu CartId okuma işlemek için yöntemi. Kullanıcının oturumunu kullanıcı CartId okuyabilirsiniz GetCartId yöntemi httpcontextbase öğesini gerektirir.

İşte tam **ShoppingCart sınıfı**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>Viewmodel'lar

Bizim alışveriş sepeti denetleyicisi bizim Model nesneleri için düzgün bir şekilde eşlemiyor karmaşık bazı bilgileri görünümlerini için iletişim kurması gerekir. Bizim modelleri, görünümleri uyacak şekilde değiştirmek istemediğiniz; Etki alanımızda, kullanıcı arabiriminde model sınıfları temsil etmelidir. Store Yöneticisi açılır bilgilerle yaptığımız, ancak birçok bilgi geçirme ViewBag yönetmek zor alır ViewBag sınıfı kullanarak bizim görünümleri için bilgi geçirmek için bir çözüm olacaktır.

Bu çözüme kullanmaktır *ViewModel* deseni. Bu model kullanılırken, bizim belirli görünüm senaryolar için iyileştirilen ve hangi özellikleri görünümü şablonlarımızı gerekli dinamik değerler/içerik üzerinden kullanıma sunacaksınız kesin türü belirtilmiş sınıfları oluştururuz. Bizim denetleyici sınıflarına doldurun ve kullanmak üzere bizim görünüm şablonu bu görünüm için iyileştirilmiş sınıfların geçirin. Bu tür güvenliği, derleme zamanı denetimi ve Düzenleyicisi IntelliSense içinde görüntüleme şablonları sağlar.

Bizim alışveriş sepeti denetleyicisi kullanmak için iki görünüm modeli oluşturacağız: ShoppingCartViewModel kullanıcının alışveriş sepeti içeriğini tutacak ve ShoppingCartRemoveViewModel bir kullanıcının bir şey kaldırdığında onay bilgileri görüntülemek için kullanılacak kendi sepetinden.

Yeni bir Viewmodel'lar klasör düzenli tutmak için proje kök dizininde oluşturalım. Projeyi sağ tıklatın, Ekle'yi seçin / yeni bir klasör.

![](mvc-music-store-part-8/_static/image1.jpg)

Viewmodel'lar klasörün adı.

![](mvc-music-store-part-8/_static/image1.png)

Ardından, ShoppingCartViewModel sınıfı Viewmodel'lar klasöre ekleyin. İki özelliğe sahiptir: alışveriş sepetini öğeleri ve tüm öğeleri toplam fiyatı arabasına tutacak bir ondalık değer listesi.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Şimdi aşağıdaki dört özelliklerle Viewmodel'lar klasörüne ShoppingCartRemoveViewModel ekleyin.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Alışveriş sepeti denetleyicisi

Alışveriş sepeti denetleyicisi üç temel amacı vardır: öğe için bir sepet ekleme, sepetinden öğeleri kaldırma ve arabasında öğeler görüntüleniyor. Kullanan üç sınıfları ki hale getirecek yeni oluşturduğunuz: ShoppingCartViewModel ShoppingCartRemoveViewModel ve ShoppingCart. StoreController ve StoreManagerController olduğu gibi MusicStoreEntities örneğini tutacak bir alan ekleyeceğiz.

Yeni bir alışveriş sepeti denetleyicisi boş denetleyici şablon kullanılarak projeye ekleyin.

![](mvc-music-store-part-8/_static/image2.png)

Tam ShoppingCart denetleyicisi aşağıda verilmiştir. Dizin ve denetleyici Ekle eylemleri tanıdık gelecektir. Remove ve CartSummary denetleyici eylemleri aşağıdaki bölümde açıklayacağız iki özel durumları işler.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>JQuery AJAX güncelleştirmeleriyle

Sonraki ShoppingCartViewModel için kesin ve önce olarak aynı yöntemi kullanarak liste görünümü şablonunu kullanan bir alışveriş sepeti dizin sayfa oluşturacağız.

![](mvc-music-store-part-8/_static/image3.png)

Ancak, bir Html.ActionLink sepetinden öğeleri kaldırmak için kullanmak yerine, jQuery "yukarı RemoveLink HTML sınıfı bu görünümde tüm bağlantılar için tıklama olayı wire için" kullanacağız. Form gönderme yerine bu click olay işleyicisi yalnızca bir AJAX geri çağırma RemoveFromCart denetleyicisi eylemimiz hale getirir. Bir seri hale getirilmiş JSON sonuç RemoveFromCart döndürür, bizim jQuery geri çağırma daha sonra ayrıştırır ve jQuery kullanarak sayfasına dört Hızlı güncelleştirmeler yapar:

- 1. Silinen albümü listeden kaldırır.
- 2. Sepet sayısı üst bilgisindeki güncelleştirir
- 3. Kullanıcı için bir güncelleştirme iletisi görüntüler
- 4. Sepet toplam fiyat güncelleştirir

Kaldır senaryo dizin görünümündeki bir Ajax geri çağırma tarafından işlenen olduğundan, ek bir görünüm için RemoveFromCart eylem gerekmez. /ShoppingCart/Index görünümü için tam kod aşağıdaki gibidir:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Bu test için öğe, alışveriş sepetimizi eklemek gerekiyor. Güncelleştireceğiz bizim **Store ayrıntıları** bir "Sepete Ekle" düğmesi içerecek şekilde görünümü. Biz adresinden iken, bazı ekledik albüm ek bilgiler dahil edebilirsiniz Biz bu görünüm son güncelleştirmesinden: Tarz, sanatçının, fiyat ve albüm resmi. Güncelleştirilmiş kodu Store Ayrıntıları Görüntüle, aşağıda gösterildiği gibi görünür.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Şimdi biz mağazası aracılığıyla tıklayın ve test ekleme ve albümleri, için ve alışveriş sepetimizi kaldırma. Uygulamayı çalıştırmak ve Store dizinine göz atın.

![](mvc-music-store-part-8/_static/image4.png)

Ardından, Albümler listesini görüntülemek için bir türe tıklayın.

![](mvc-music-store-part-8/_static/image5.png)

Artık bir albüm başlığına tıklayarak "Sepete Ekle" düğmesi de dahil olmak üzere güncelleştirilmiş bizim albüm Ayrıntılar görünümünü gösterir.

![](mvc-music-store-part-8/_static/image6.png)

"Sepete Ekle" düğmesine tıklayarak bizim alışveriş sepeti dizini görünümde alışveriş sepeti Özet listesi ile gösterilir.

![](mvc-music-store-part-8/_static/image7.png)

Alışveriş sepetinize yüklendikten sonra Ajax güncelleştirme, alışveriş sepetine görmek için sepet bağlantısından Kaldır tıklayabilirsiniz.

![](mvc-music-store-part-8/_static/image8.png)

Alışveriş sepeti kaydı kendi sepetine öğe ekleme olanağı tanıyan bir çalışan bir çıkış oluşturduk. Aşağıdaki bölümde, bunları kaydetmek ve kullanıma alma işlemini tamamlamak izin veriyoruz.


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-7.md)
> [İleri](mvc-music-store-part-9.md)
