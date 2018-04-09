---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: '8. Kısım: Alışveriş sepeti Ajax güncelleştirmeleriyle | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 8 Ajax güncelleştirmelerini ile alışveriş sepeti kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>8. Kısım: Alışveriş sepetine Ajax güncelleştirmelerle
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 8 Ajax güncelleştirmelerini ile alışveriş sepeti kapsar.


Kaydettirmeden kendi sepetinizde albümleri yerleştirmek kullanıcılara izin vermek, ancak tam checkout konuklar olarak kaydetmeniz gerekir. Alışveriş ve kullanıma alma işlemi iki denetleyicilerinde ayrılacaktır: anonim bir sepetine öğe eklemeyi sağlayan bir ShoppingCart denetleyici ve ödeme işlemi işleyen bir Checkout denetleyici. Biz bu bölümdeki alışveriş sepetine ile başlayın, ardından derleme şu bölümdeki kullanıma alma işlemi.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Sepeti, sipariş ve OrderDetail model sınıfları ekleme

Bizim alışveriş sepeti ve satın alma işlemleri yapar bazı yeni sınıflar kullanın. Modeller klasörü sağ tıklatın ve aşağıdaki kod ile Sepeti sınıfı (Cart.cs) ekleyin.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Bu sınıf, o ana kadarki öznitelik dışında [anahtarı] RecordID özelliği için kullandığımız başkalarına oldukça benzer. Bizim Sepeti öğeleri anonim alışveriş izin vermek için CartID adlı bir dize tanımlayıcısına sahip olacaktır, ancak tablo RecordID adlı bir tamsayı birincil anahtar içerir. Kurala göre Entity Framework kod-ilk Sepeti adlı bir tablo için birincil anahtar CartId veya kimliği olacaktır, ancak biz istiyorsanız, biz kolayca, ek açıklama ve kod kılabilirsiniz bekliyor. Bu bir örnek biz basit kuralları Entity Framework kod-ilk nasıl kullanabileceğinizi bize uygun olduğunda, ancak sunulmuyorsa, biz onlar tarafından kısıtlı değil.

Ardından, sipariş sınıfı (Order.cs) ile aşağıdaki kodu ekleyin.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Bu sınıf, Sipariş Özeti ve teslimat bilgileri izler. **Henüz derleme olmaz**, henüz oluşturduğumuz henüz bir sınıf üzerinde bağımlı bir sipariş ayrıntıları gezinti özelliği olduğundan. Şimdi şimdi ekleyerek bir sınıf OrderDetail.cs, aşağıdaki kod ekleme adlandırdığınız düzeltin.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Bir son güncelleştirme ayrıca bir DbSet dahil olmak üzere bu yeni Model sınıfları kullanıma DbSets dahil etmek için bizim MusicStoreEntities sınıfı sağlayacaksınız&lt;sanatçı&gt;. Güncelleştirilmiş MusicStoreEntities sınıfı olarak görünür aşağıda.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Alışveriş sepetine iş mantığı yönetme

Ardından, modelleri klasöründe ShoppingCart sınıfı oluşturacağız. ShoppingCart modeli Sepeti tabloya veri erişimi işler. Ayrıca, bu öğeler ekleme ve Alışveriş sepetinden kaldırma için iş mantığı işleyecek.

Yalnızca kendi alışveriş sepetine öğe eklemek bir hesap için kaydolun gerektirmek istemediğiniz olduğundan, biz kullanıcıların (bir GUID veya genel benzersiz tanımlayıcısını kullanarak) geçici benzersiz bir tanımlayıcı atar alışveriş sepeti eriştiklerinde. ASP.NET oturum sınıfını kullanarak bu kimliği depolarız.

*Not: ASP.NET oturum site çıktıktan sonra sona erecek kullanıcıya özgü bilgileri depolamak için uygun bir yerdir. Oturum durumu kötüye kullanılması büyük sitelerinde performans etkileri sahip olsa da, hafif kullanma da gösterim amaçları için çalışır.*

ShoppingCart sınıf aşağıdaki yöntemleri sunar:

**AddToCart** albüm bir parametre olarak alır ve kullanıcının sepetine ekler. Sepeti tablonun her albüm miktarını izler olduğundan, gerekiyorsa yeni bir satır oluşturun veya kullanıcı albümü bir kopyası zaten sipariş miktar yalnızca artırmak için mantığı içerir.

**RemoveFromCart** albüm kimliği alır ve kullanıcının sepetinden kaldırır. Kullanıcı kendi sepetinizde yalnızca albümü bir kopyasını olsaydı, satır kaldırılır.

**EmptyCart** kullanıcının Alışveriş sepetinden tüm öğeleri kaldırır.

**GetCartItems** görüntü veya işlem için CartItems listesini alır.

**GetCount** alır bir bir kullanıcı, kendi alışveriş sepeti sahip albümleri toplam sayısı.

**GetTotal** sepetteki tüm öğelerin toplam maliyeti hesaplar.

**CreateOrder** alışveriş sepeti siparişe checkout aşamasında dönüştürür.

**GetCart** Sepeti nesnesi edinmek bizim denetleyicileri sağlayan statik bir yöntemdir. Kullandığı **GetCartId** CartId kullanıcının oturumunu okuma işlemek için yöntem. Kullanıcının oturumunu kullanıcının CartId okuyabilmeniz GetCartId yöntemi ortamdan HttpContextBase gerektirir.

İşte tam **ShoppingCart sınıfı**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Bizim alışveriş sepeti denetleyicisi bizim Model nesneleri düzgün bir şekilde eşlenmez bazı karmaşık bilgileri kendi görünümlere iletişim kurmak gerekir. Bizim görünümleri uyacak şekilde bizim modelleri değiştirmek istemiyorsanız; Model sınıfları bizim etki alanı, kullanıcı arabirimi temsil etmelidir. Mağaza Yöneticisi açılır bilgilerle yaptığımız, ancak çok sayıda bilgi ViewBag geçirme yönetmek sabit alır ViewBag sınıfını kullanarak bizim görünümlerine bilgi aktarmak için bir çözüm olabilir.

Bu çözüme kullanmaktır *ViewModel* düzeni. Bu deseni kullanılırken bizim belirli görünüm senaryolar için iyileştirilen ve hangi görünüm şablonlarımız tarafından gerekli dinamik değerler/içerik özelliklerini ortaya kesin türü belirtilmiş sınıfları oluşturuyoruz. Bizim denetleyicisi sınıfları sonra doldurmak ve bu görünüm için iyileştirilmiş sınıfları kullanmak üzere bizim görünüm şablonu geçirin. Bu tür güvenliği, derleme zamanı denetlemek ve düzenleyici IntelliSense görünümü şablonlar içindeki sağlar.

Bizim alışveriş sepeti denetleyicisi kullanmak için iki görünüm modeli oluşturacağız: ShoppingCartViewModel kullanıcının alışveriş sepeti içeriğini tutacak ve ShoppingCartRemoveViewModel bir kullanıcı bir şey çıkardığında onay bilgilerini görüntülemek için kullanılır kendi sepetinden.

Yeni bir ViewModels klasör düzenli tutmak için bizim proje kök dizininde oluşturalım. Projeyi sağ tıklatın, Ekle'yi seçin / yeni bir klasör.

![](mvc-music-store-part-8/_static/image1.jpg)

ViewModels klasörün adı.

![](mvc-music-store-part-8/_static/image1.png)

Ardından, ShoppingCartViewModel sınıfı ViewModels klasöründe ekleyin. İki özelliklere sahiptir: Sepeti öğeleri ve tüm öğeler için toplam fiyatı sepetinizde tutmak için ondalık bir değer listesi.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Şimdi ShoppingCartRemoveViewModel ViewModels klasörüyle aşağıdaki dört özellikleri ekleyin.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Alışveriş sepeti denetleyicisi

Alışveriş sepetine denetleyicisi üç ana amacı vardır: bir sepetine öğe ekleme, Alışveriş sepetinden öğeleri kaldırma ve sepetinizde öğeleri görüntüleme. Kullanmak üç sınıflarını biz yapacak yeni oluşturduğunuz: ShoppingCartViewModel, ShoppingCartRemoveViewModel ve ShoppingCart. StoreController ve StoreManagerController olduğu gibi MusicStoreEntities örneği tutmak için bir alan ekleyeceğiz.

Yeni bir alışveriş sepeti denetleyicisi boş denetleyicisi şablonu kullanarak projenize ekleyin.

![](mvc-music-store-part-8/_static/image2.png)

Tam ShoppingCart denetleyicisi aşağıdadır. Dizin ve denetleyici Ekle Eylemler tanıdık gelecektir. Kaldır ve CartSummary denetleyici eylemleri aşağıdaki bölümde ele alacağız iki özel durumları işler.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>JQuery AJAX güncelleştirmelerle

Sonraki ShoppingCartViewModel kesin türü belirtilmiş ve önce yöntemi olarak kullanarak liste görünümü şablonu kullanan bir alışveriş sepeti dizin sayfası oluşturacağız.

![](mvc-music-store-part-8/_static/image3.png)

Ancak, Alışveriş sepetinden öğeleri kaldırmak için bir Html.ActionLink kullanmak yerine, jQuery "yukarı RemoveLink HTML sınıfı bu görünümde tüm bağlantılar için click olayını wire için" kullanacağız. Form gönderme yerine bu click olay işleyicisi yalnızca bir AJAX geri çağırma bizim RemoveFromCart denetleyici eylemi hale getirir. Bir JSON serisi haline getirilen sonuç RemoveFromCart verir bizim jQuery geri çağırma daha sonra ayrıştırır ve jQuery kullanma sayfaya dört Hızlı güncelleştirmeleri gerçekleştirir:

- 1. Silinen albüm listeden kaldırır
- 2. Sepeti sayısı güncelleştirir
- 3. Kullanıcı için bir güncelleştirme iletisi görüntüler
- 4. Sepeti toplam fiyat güncelleştirir

Kaldır senaryo bir Ajax geri çağırma dizin görünümünün içinde tarafından işlenen beri RemoveFromCart eylemi için ek bir görünüm gerekmez. /ShoppingCart/Index görünüm için tam kod aşağıdaki gibidir:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Bunu test etmek için şu öğeler için alışveriş sepetimizi eklemek gerekir. Biz güncelleştireceğim bizim **deposu ayrıntıları** "Sepete Ekle" düğmesini içerecek biçimde görünümü. Yerinde ki olsa da, bazı ekledik albüm ek bilgiler dahil edebilirsiniz Biz bu görünümünün son güncelleştirmesinden: Tarz, sanatçı, fiyat ve albüm resmi. Güncelleştirilmiş deposu Ayrıntıları görünümü kodu aşağıda gösterildiği gibi görünür.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Şimdi biz mağazayla tıklayın ve test ekleme ve albümleri alışveriş sepetimizi gelen ve giden kaldırılıyor. Uygulamayı çalıştırın ve depolama dizinine göz atın.

![](mvc-music-store-part-8/_static/image4.png)

Ardından, Albümler listesini görüntülemek için bir tarzını'ı tıklatın.

![](mvc-music-store-part-8/_static/image5.png)

Şimdi bir albüm başlığına tıklamak "Sepete Ekle" düğmesi de dahil olmak üzere bizim güncelleştirilmiş Albüm Ayrıntıları görünümü gösterir.

![](mvc-music-store-part-8/_static/image6.png)

"Sepete Ekle" düğmesini tıklatarak bizim alışveriş sepeti dizin görünüm alışveriş sepeti Özet listesini gösterir.

![](mvc-music-store-part-8/_static/image7.png)

Alışveriş Sepetiniz yüklendikten sonra alışveriş Sepetiniz Ajax güncelleştirme görmek için Sepeti bağlantısından Kaldır tıklatabilirsiniz.

![](mvc-music-store-part-8/_static/image8.png)

Kaydı kullanıcıların kendi sepetine öğeleri eklemesine olanak veren alışveriş çalışma çıkışı temel aldık. Aşağıdaki bölümde, bunları kaydetmek ve kullanıma alma işlemini tamamlamak izin veriyoruz.


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-7.md)
> [sonraki](mvc-music-store-part-9.md)
