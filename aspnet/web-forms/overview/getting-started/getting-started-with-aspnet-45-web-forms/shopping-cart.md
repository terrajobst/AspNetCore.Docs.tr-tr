---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Alışveriş sepeti | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: a8e96da7737cdf649575711a464c4f7726cb6ded
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890726"
---
<a name="shopping-cart"></a>Alışveriş sepeti
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğretici alışveriş sepeti Wingtip Toys örnek ASP.NET Web Forms uygulamasına eklemek için gerekli iş mantığı açıklar. Bu öğretici önceki öğreticiyi "Görünen veri öğeleri ve Ayrıntıları" oluşturur ve Wingtip Oyuncak deposu öğretici serinin bir parçasıdır. Bu öğreticiyi tamamladıktan sonra kullanıcılar örnek uygulamanızın eklemek, kaldırmak ve bunların alışveriş sepeti ürünlerinde değiştirmek mümkün olacaktır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

1. Web uygulaması için alışveriş sepeti oluşturma
2. Alışveriş sepetine öğelerini eklemesine olanak sağlamak nasıl.
3. Nasıl ekleneceği bir [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) alışveriş sepeti ayrıntıları görüntülemek için denetimi.
4. Nasıl hesaplamak ve sipariş toplama görüntüler.
5. Nasıl kaldırılacağı ve alışveriş sepeti güncelleştirme öğeler.
6. Bir alışveriş sepeti sayaç dahil etme.

## <a name="code-features-in-this-tutorial"></a>Bu öğreticideki kod özellikleri:

1. Entity Framework Code First
2. Veri ek açıklamaları
3. Kesin türü belirtilmiş veri denetimleri
4. Model bağlama

## <a name="creating-a-shopping-cart"></a>Alışveriş sepeti oluşturma

Öğretici serisinde daha önce bu sayfaları ve veritabanından ürün verilerini görüntülemek için kodu eklemiştir. Bu öğreticide, ürünleri yönetmek için alışveriş sepeti oluşturacaksınız kullanıcılar satın alma ilginizi çekiyor mu olduğunu. Kullanıcıların göz atın ve bunlar olmayan kaydedilmiş veya oturum açmış olsa bile öğeleri alışveriş sepetine eklemek mümkün olacaktır. Alışveriş sepeti erişimi yönetmek için kullanıcıların benzersiz bir atayacaktır `ID` Sepeti ilk kez kullanıcı alışveriş eriştiğinde bir genel benzersiz tanımlayıcı (GUID) kullanarak. Bu depolayacağınız `ID` ASP.NET oturum durumu kullanma.

> [!NOTE] 
> 
> ASP.NET oturum durumu, kullanıcı site ayrılmasından sonra sona erecek kullanıcıya özgü bilgileri depolamak için uygun bir yerdir. Oturum durumu kötüye kullanılması büyük sitelerinde performans etkileri sahip olsa da, oturum durumu çalışır tanıtım amacıyla iyi kullanımını açık. Wingtip Toys örnek proje oturum durumunu işlem içinde saklanıyorsa siteyi barındıran web sunucusunda olduğu oturum durumu bir dış sağlayıcı olmadan kullanmayı gösterir. Bir uygulama birden çok örneğini sağlamak büyük siteleri veya farklı sunucularda bir uygulama birden çok örneğini çalıştıran siteler için kullanmayı **Microsoft Azure önbellek hizmeti**. Bu önbellek hizmeti web sitesine dış ve işlem içi oturum durumu kullanmanın sorununu çözer dağıtılmış bir önbelleğe alma hizmeti sağlar. Daha fazla bilgi için [kullanım ASP.NET oturum durumu Windows Azure Web siteleri ile nasıl](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>CartItem bir Model sınıfı ekleme

Daha önce Bu öğretici serisinde, kategori ve ürün verilerini için şemaya oluşturarak tanımlanan `Category` ve `Product` sınıfları *modelleri* klasör. Şimdi, alışveriş sepeti şemasını tanımlamak için yeni bir sınıf ekleyin. Daha sonra Bu öğreticide, veri erişimi işlemek için bir sınıf ekler `CartItem` tablo. Bu sınıf eklemek, kaldırmak ve alışveriş sepeti öğelerde güncelleştirmek için iş mantığı sağlar.

1. Sağ *modelleri* klasörü ve select **Ekle**  - &gt; **yeni öğe**. 

    ![Alışveriş sepeti - yeni öğe](shopping-cart/_static/image1.png)
2. **Yeni Öğe Ekle** iletişim kutusu görüntülenir. Seçin **kod**ve ardından **sınıfı**. 

    ![Alışveriş sepeti - yeni öğe iletişim ekleyin](shopping-cart/_static/image2.png)
3. Bu yeni sınıf adını *CartItem.cs*.
4. **Ekle**'yi tıklatın.  
   Yeni sınıf dosyası Düzenleyicisi'nde görüntülenir.
5. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem` Sınıfı, bir kullanıcı ekler alışveriş sepetine her ürünün tanımlayacaksınız şeması içerir. Bu sınıf, Bu öğretici serisinde daha önce oluşturduğunuz diğer şema sınıfları benzerdir. Kurala göre Entity Framework Code First, bekliyor için birincil anahtar `CartItem` tablo ya da olacaktır `CartItemId` veya `ID`. Bununla birlikte, kod varsayılan davranışı veri ek açıklamasını kullanarak geçersiz kılar `[Key]` özniteliği. `Key` ItemId özelliğinin özniteliği belirtir `ItemID` birincil anahtarı bir özelliktir.

`CartId` Özellik belirtir `ID` satın almak için bir öğesiyle ilişkilendirilmiş olan kullanıcı. Bu kullanıcı oluşturmak için kod ekleyeceksiniz `ID` kullanıcının alışveriş sepeti eriştiği. Bu `ID` de bir ASP.NET oturum değişkeni depolanır.

### <a name="update-the-product-context"></a>Ürün içeriği güncelleştirme

Ekleme yanı sıra `CartItem` sınıfı, varlık sınıflarını yönetir ve veritabanına veri erişimi sağlayan veritabanı bağlamı sınıfının güncellemeniz gerekir. Bunu yapmak için yeni oluşturulan ekleyecek `CartItem` model sınıfı için `ProductContext` sınıfı.

1. İçinde **Çözüm Gezgini**, bulma ve açma *ProductContext.cs* dosyasını *modelleri* klasör.
2. Vurgulanmış kodu ekleyin *ProductContext.cs* gibi dosya:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Bu öğretici serisinde kodu daha önce belirtildiği gibi *ProductContext.cs* dosyayı ekler `System.Data.Entity` ad alanı Entity Framework'ün tüm çekirdek işlevlerin erişimi böylece. Bu işlevsellik, sorgu, Ekle, Güncelleştir ve güçlü şekilde yazılan nesnelerin çalışarak verilerini silmek için özelliği içerir. `ProductContext` Sınıfı erişim için yeni eklenen ekler `CartItem` model sınıfı.

### <a name="managing-the-shopping-cart-business-logic"></a>Alışveriş sepeti iş mantığı yönetme

Ardından, oluşturacağınız `ShoppingCart` sınıfının yeni bir *mantığı* klasör. `ShoppingCart` Sınıfı işler veri erişimi `CartItem` tablo. Sınıf eklemek, kaldırmak ve alışveriş sepeti öğelerde güncelleştirmek için iş mantığı da içerir.

Aşağıdaki eylemleri yönetmek için işlevselliği ekleyeceksiniz alışveriş sepeti mantığı içerir:

1. Alışveriş sepetine öğeler ekleme
2. Alışveriş sepetinden öğeleri kaldırma
3. Alışveriş sepeti kimliği alma
4. Alışveriş sepetinden öğeleri alınıyor
5. Tüm öğelerin alışveriş sepeti tutar toplamda
6. Alışveriş sepeti verileri güncelleştirme

Bir alışveriş sepeti sayfası (*ShoppingCart.aspx*) ve alışveriş sepeti sınıfı birlikte alışveriş sepeti verilere erişmek için kullanılır. Alışveriş sepeti sayfası kullanıcı alışveriş sepetine ekler tüm öğeleri görüntüler. Alışveriş sepeti yanında, sayfa ve sınıf, bir sayfa oluşturacaksınız (*AddToCart.aspx*) ürünleri alışveriş sepetine eklemek için. Ayrıca kod ekleyeceksiniz *ProductList.aspx* sayfa ve *ProductDetails.aspx* bağlantısını sağlayacak sayfa *AddToCart.aspx* sayfasında, böylece kullanıcı ekleyebilirsiniz Ürünler alışveriş sepetine.

Aşağıdaki diyagramda, kullanıcı bir ürün alışveriş sepetine eklediğinde oluşan temel işlemi gösterilmektedir.

![Alışveriş sepeti - alışveriş sepetine ekleme](shopping-cart/_static/image3.png)

Kullanıcı tıkladığında **Sepete Ekle** ya da bağlantıyı *ProductList.aspx* sayfa veya *ProductDetails.aspx* , uygulama sayfadan *AddToCart.aspx* sayfa ve ardından otomatik olarak *ShoppingCart.aspx* sayfası. *AddToCart.aspx* sayfa ekleyecek select ürün alışveriş sepetine ShoppingCart sınıfında bir yöntem çağırarak. *ShoppingCart.aspx* sayfası alışveriş sepetine eklenen ürünleri görüntüler.

#### <a name="creating-the-shopping-cart-class"></a>Alışveriş sepeti sınıfı oluşturma

`ShoppingCart` Sınıfı eklenir uygulamayı ayrı bir klasörde model (model klasör), sayfa (kök klasöründe) ve (mantığı klasör) mantığı arasında NET bir ayrıma böylece olacaktır.

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**proje ve seçin **Ekle**-&gt;**yeni klasör**. Yeni bir klasör adı *mantığı*.
2. Sağ *mantığı* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.
3. Adlı yeni bir sınıf dosyası ekleyin *ShoppingCartActions.cs*.
4. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart` Yöntemi etkinleştirir ürüne bağlı alışveriş sepeti dahil edilecek tek tek ürünlerin `ID`. Sepete ürün eklenir veya Sepeti bu ürün için bir öğe içeriyorsa, miktarı artırılır.

`GetCartId` Yöntemi döndürür Sepeti `ID` kullanıcı için. Sepeti `ID` bir kullanıcı, kendi alışveriş sepeti sahip öğelerini izlemek için kullanılır. Kullanıcı varolan Sepeti yoksa `ID`, yeni Sepeti `ID` kendileri için oluşturulur. Kullanıcı, Sepeti gibi kayıtlı bir kullanıcı olarak oturum varsa `ID` kendi kullanıcı adına ayarlayın. Ancak, kullanıcı sepetinizde, imzalı değilse `ID` benzersiz bir değere (GUID) ayarlayın. Bu yalnızca bir Sepeti oturum göre her kullanıcı için oluşturulmuş bir GUID sağlar.

`GetCartItems` Yöntemi alışveriş sepeti öğeleri kullanıcı için bir listesini döndürür. Daha sonra Bu öğreticide, model bağlama alışveriş sepeti kullanımında Sepeti öğeleri görüntülemek için kullanıldığını görürsünüz `GetCartItems` yöntemi.

### <a name="creating-the-add-to-cart-functionality"></a>Sepeti ile ekleme işlevselliği oluşturma

Daha önce belirtildiği gibi adlandırılmış bir işleme sayfa oluşturacak *AddToCart.aspx* yeni ürünler kullanıcı alışveriş sepetine eklemek için kullanılır. Bu sayfayı çağıracak `AddToCart` yönteminde `ShoppingCart` oluşturduğunuz sınıfı. *AddToCart.aspx* sayfası, beklediğiniz bir ürün `ID` kendisine geçirilen. Bu ürün `ID` çağrılırken kullanılacak `AddToCart` yönteminde `ShoppingCart` sınıfı.

> [!NOTE] 
> 
> Arka plan kodu değiştirme (*AddToCart.aspx.cs*) UI sayfada değil Bu sayfa için (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Ekle Sepeti oluşturmak için işlevselliği:

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**proje, tıklatın **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Standart yeni bir sayfa (Web formu) adlı uygulamaya eklemek *AddToCart.aspx*. 

    ![Alışveriş sepeti - Web formu ekleyin](shopping-cart/_static/image4.png)
3. İçinde **Çözüm Gezgini**, sağ *AddToCart.aspx* sayfasında ve ardından **görünümü kodu**. *AddToCart.aspx.cs* arka plan kodu dosya Düzenleyicisi'nde açılır.
4. Varolan kodla *AddToCart.aspx.cs* arka plan kodu aşağıdaki ile:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Zaman *AddToCart.aspx* sayfa yüklenir, ürün `ID` Sorgu dizesinden alınır. Ardından, alışveriş sepeti sınıfının bir örneği oluşturulur ve çağırmak için kullanılan `AddToCart` Bu öğreticide daha önce eklediğiniz yöntemi. `AddToCart` Yöntemi, bulunan *ShoppingCartActions.cs* dosya, seçilen ürün alışveriş sepetine eklemek veya seçili ürünün ürün miktarını artırmak için mantığı içerir. Ürün alışveriş sepetine eklenmemişse ürün eklenen `CartItem` veritabanı tablosu. Ürün alışveriş sepetine eklendi ve kullanıcı aynı ürünün ek bir öğe ekler, ürün miktarını içinde artırılır `CartItem` tablo. Son olarak, geri sayfasına yönlendiren *ShoppingCart.aspx* burada kullanıcının gördüğü sepetinizdeki öğeler güncelleştirilmiş bir listesini sonraki adımda ekleyeceksiniz sayfası.

Daha önce belirtildiği gibi bir kullanıcı `ID` belirli bir kullanıcı ile ilişkili ürün tanımlamak için kullanılır. Bu `ID` bir satır eklenir `CartItem` tablo her zaman kullanıcı alışveriş sepetine ürün ekler.

### <a name="creating-the-shopping-cart-ui"></a>Alışveriş sepeti kullanıcı Arabirimi oluşturma

*ShoppingCart.aspx* sayfası kullanıcı kendi alışveriş sepetine eklenen ürünleri görüntüler. De eklemek, kaldırmak ve alışveriş sepeti öğelerde güncelleştirme olanağı sağlanır.

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**, tıklatın **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Ana sayfayı seçerek içeren yeni bir sayfa (Web formu) ekleyin **Web ana sayfa kullanarak Form**. Yeni sayfa adı *ShoppingCart.aspx*.
3. Seçin **Site.Master** ana sayfa yeni oluşturulan eklemek için *.aspx* sayfası.
4. İçinde *ShoppingCart.aspx* sayfasında, varolan biçimlendirme aşağıdaki biçimlendirme ile değiştirin:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx* sayfa içeren bir **GridView** adlı Denetim `CartList`. Bu denetimi model bağlama veritabanından alışveriş sepeti veri bağlamak için kullanır. **GridView** denetim. Ayarladığınızda `ItemType` özelliği **GridView** denetlemek, veri bağlama ifadesi `Item` bulunur ve denetimi biçimlerini kesin türü belirtilmiş haline gelir. Bu öğretici serisinde daha önce belirtildiği gibi ayrıntılarını seçebilirsiniz `Item` IntelliSense kullanarak nesne. Model bağlama verileri seçmek için kullanılacak bir veri denetimini yapılandırmak için ayarladığınız `SelectMethod` denetiminin özelliği. Yukarıdaki biçimlendirme ayarladığınız `SelectMethod` listesi döndüren GetShoppingCartItems yöntemi kullanmak üzere `CartItem` nesneleri. **GridView** veri denetimi sayfa yaşam döngüsündeki uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. `GetShoppingCartItems` Yöntemi hala eklenmelidir.

#### <a name="retrieving-the-shopping-cart-items"></a>Alışveriş sepeti öğeleri alınıyor

Ardından, kod ekleyin *ShoppingCart.aspx.cs* almak ve alışveriş sepeti UI doldurmak için arka plan kod.

1. İçinde **Çözüm Gezgini**, sağ *ShoppingCart.aspx* sayfasında ve ardından **görünümü kodu**. *ShoppingCart.aspx.cs* arka plan kodu dosya Düzenleyicisi'nde açılır.
2. Var olan kodu aşağıdakilerle değiştirin:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Yukarıda belirtildiği gibi `GridView` veri denetimi çağrıları `GetShoppingCartItems` yöntemi sayfası yaşamında uygun zamanda döngüsü ve döndürülen verileri otomatik olarak bağlar. `GetShoppingCartItems` Yöntemi bir örneğini oluşturur `ShoppingCartActions` nesnesi. Daha sonra çağrılarak sepetinizde öğeleri döndürmek için bu örnek kodu kullanan `GetCartItems` yöntemi.

### <a name="adding-products-to-the-shopping-cart"></a>Alışveriş sepetine ürün ekleme

Zaman ya da *ProductList.aspx* veya *ProductDetails.aspx* sayfası görüntülenirse, kullanıcı bir bağlantıyı kullanarak alışveriş sepetine ürün eklemek mümkün olacaktır. Bunlar bağlantısını tıklattığınızda uygulama adlı işleme sayfasına gider *AddToCart.aspx*. *AddToCart.aspx* sayfa çağıracaktır `AddToCart` yönteminde `ShoppingCart` Bu öğreticide daha önce eklediğiniz sınıfı.

Şimdi, ekleyeceksiniz bir **Sepete Ekle** hem de bağlantı *ProductList.aspx* sayfa ve *ProductDetails.aspx* sayfası. Bu bağlantıyı ürün içerecektir `ID` veritabanından alınır.

1. İçinde **Çözüm Gezgini**, bulma ve adlı sayfası açmak *ProductList.aspx*.
2. Sarı vurgulanan biçimlendirmeleri eklemek *ProductList.aspx* tüm sayfayı şu şekilde görünmesi sayfasında:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Alışveriş sepeti test etme

Alışveriş sepetine ürünleri eklemek ne görmek için uygulamayı çalıştırın.

1. Tuşuna **F5** uygulamayı çalıştırın.  
 Proje veritabanını yeniden oluşturur sonra tarayıcı Göster açıp *Default.aspx* sayfası.
2. Seçin **araba** kategori Gezinti menüsünde.  
 *ProductList.aspx* sayfası, yalnızca "Araba" kategorisindeki ürünleri gösteren görüntülenir. 

    ![Alışveriş sepeti - araba](shopping-cart/_static/image5.png)
3. Tıklatın **Sepete Ekle** ilk ürün yanındaki bağlantı listelenen (dönüştürülebilir car).   
 *ShoppingCart.aspx* sayfası görüntülenirse, alışveriş Sepetiniz seçimi gösterme. 

    ![Alışveriş sepeti - Sepeti](shopping-cart/_static/image6.png)
4. Ek ürünler seçerek görüntülemek **düzlemleri** kategori Gezinti menüsünde.
5. Tıklatın **Sepete Ekle** listelenen ilk ürün yanındaki bağlantı.  
 *ShoppingCart.aspx* ek öğesiyle sayfası görüntülenir.
6. Tarayıcıyı kapatın.

### <a name="calculating-and-displaying-the-order-total"></a>Hesaplama ve sipariş toplam görüntüleme

Alışveriş sepetine ürün ekleme yanı sıra ekleyeceksiniz bir `GetTotal` yönteme `ShoppingCart` sınıfı ve alışveriş sepeti sayfasında toplam sipariş miktarını görüntüler.

1. İçinde **Çözüm Gezgini**, açık *ShoppingCartActions.cs* dosyasını *mantığı* klasör.
2. Aşağıdakileri ekleyin `GetTotal` sarıya vurgulanan yöntemi `ShoppingCart` sınıf böylece sınıfı şu şekilde görünür:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

İlk olarak, `GetTotal` yöntemi, kullanıcı için alışveriş sepeti Kimliğini alır. Ardından yöntemi alışveriş sepeti listelenen her ürün için ürün miktarı Ürün Fiyat çarparak toplam alır.

> [!NOTE] 
> 
> Yukarıdaki kod boş değer atanabilir tür kullanır "`int?`". Boş değer atanabilir türler tüm değerlerin temel alınan türü ve boş bir değer olarak gösterebilir. Daha fazla bilgi için [kullanarak boş değer atanabilir türler](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Alışveriş sepeti görüntüsünü değiştirme

Sonraki kodunu değiştireceksiniz *ShoppingCart.aspx* çağırmak için sayfa `GetTotal` yöntemi ve toplam görüntü *ShoppingCart.aspx* sayfasında sayfa yüklendiğinde.

1. İçinde **Çözüm Gezgini**, sağ *ShoppingCart.aspx* sayfasından seçim yapıp **görünümü kodu**.
2. İçinde *ShoppingCart.aspx.cs* dosya, güncelleştirme `Page_Load` sarı ile vurgulanmış aşağıdaki kodu ekleyerek işleyici:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Zaman *ShoppingCart.aspx* sayfayı yükler, alışveriş sepeti nesnesi yükler ve ardından çağırarak alışveriş sepeti toplam alır `GetTotal` yöntemi `ShoppingCart` sınıfı. Bir ileti alışveriş sepeti boşsa, bunu belirten görüntülenir.

### <a name="testing-the-shopping-cart-total"></a>Alışveriş sepeti toplam test etme

Nasıl yalnızca bir ürün alışveriş sepetine ekleyebileceğiniz değil görmek için uygulamayı şimdi çalıştırın, ancak alışveriş sepeti toplam görebilirsiniz.

1. Tuşuna **F5** uygulamayı çalıştırın.  
 Tarayıcıyı açın ve göstermek *Default.aspx* sayfası.
2. Seçin **araba** kategori Gezinti menüsünde.
3. Tıklatın **Sepete Ekle** ilk ürün yanındaki bağlantı.   
 *ShoppingCart.aspx* sayfası, sipariş toplamı ile gösterilir. 

    ![Sepeti - alışveriş sepeti toplamı](shopping-cart/_static/image7.png)
4. Bazı diğer ürünleri (örneğin, uçak) Sepete ekleyin.
5. *ShoppingCart.aspx* eklediğiniz tüm ürünleri güncelleştirilmiş bir toplamı ile sayfası görüntülenir. 

    ![Alışveriş sepeti - birden çok ürün](shopping-cart/_static/image8.png)
6. Tarayıcı penceresini kapatarak çalışan uygulamayı durdurun.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Alışveriş sepetine ekleme, güncelleştirme ve Checkout düğmeleri

Kullanıcıların alışveriş sepeti değiştirmesine izin vermek için ekleyeceksiniz bir **güncelleştirme** düğmesi ve **Checkout** alışveriş sepeti sayfasının düğmesini. **Checkout** daha sonra bu serideki öğretici kadar düğmesi kullanılamıyor.

1. İçinde **Çözüm Gezgini**, açık *ShoppingCart.aspx* web uygulaması projesi kökündeki sayfası.
2. Eklemek için **güncelleştirme** düğmesi ve **Checkout** düğmesine *ShoppingCart.aspx* sayfasında, varolan biçimlendirme sarıya vurgulanan biçimlendirmeyi gösterildiği gibi ekleyin Aşağıdaki kod:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Kullanıcı tıkladığında **güncelleştirme** düğmesini `UpdateBtn_Click` olay işleyicisi çağrılır. Bu olay işleyicisi sonraki adımda ekleyeceksiniz kodu çağırır.

Ardından, içinde yer alan kodu güncelleştirebilirsiniz *ShoppingCart.aspx.cs* Sepeti öğeleri ve çağrı döngü dosyaya `RemoveItem` ve `UpdateItem` yöntemleri.

1. İçinde **Çözüm Gezgini**, açık *ShoppingCart.aspx.cs* web uygulaması projesi kökündeki dosyasında.
2. Sarı vurgulanan aşağıdaki kod bölümleri eklemek *ShoppingCart.aspx.cs* dosyası:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Kullanıcı tıkladığında **güncelleştirme** düğmesini *ShoppingCart.aspx* sayfasında, UpdateCartItems yöntemi çağrılır. UpdateCartItems yöntemi alışveriş sepeti her öğe için güncelleştirilmiş değerlerini alır. Ardından, UpdateCartItems yöntemini çağırır `UpdateShoppingCartDatabase` (eklenir ve sonraki adımda açıklanan) bir yöntem eklemek veya Alışveriş sepetinden öğeleri kaldırmak için. Veritabanı alışveriş sepeti güncelleştirmeler yansıtacak şekilde güncelleştirildi sonra **GridView** denetim çağırarak alışveriş sepeti sayfasında güncelleştirilir `DataBind` yöntemi **GridView**. Ayrıca, alışveriş sepeti sayfasında toplam sipariş tutarı öğeleri güncelleştirilmiş listesini yansıtacak şekilde güncelleştirilir.

### <a name="updating-and-removing-shopping-cart-items"></a>Güncelleştirme ve alışveriş sepeti öğeleri kaldırma

Üzerinde *ShoppingCart.aspx* sayfasında, bir öğe miktarını güncelleştirme ve bir öğeyi kaldırmak için denetimleri eklenmiş görebilirsiniz. Şimdi, iş bu denetimleri yapacak kodu ekleyin.

1. İçinde **Çözüm Gezgini**, açık *ShoppingCartActions.cs* dosyasını *mantığı* klasör.
2. Sarı vurgulanan aşağıdaki kodu ekleyin *ShoppingCartActions.cs* sınıf dosyası:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase` Yöntemi çağrılır, `UpdateCartItems` yöntemi *ShoppingCart.aspx.cs* sayfasında, güncelleştirmek veya Alışveriş sepetinden öğeleri kaldırmak için mantığını içerir. `UpdateShoppingCartDatabase` Yöntemi tekrarlanan alışveriş sepeti listesindeki tüm satırların üzerinden. Bir alışveriş sepeti öğesi kaldırılması için işaretlenmiş veya miktarı değerinden bir `RemoveItem` yöntemi çağrılır. Aksi takdirde, alışveriş sepeti öğesi için denetlenir ne zaman güncelleştirmeleri `UpdateItem` yöntemi çağrılır. Alışveriş sepeti öğesi kaldırılmış ya da güncelleştirilmiş sonra veritabanı değişiklikler kaydedilir.

`ShoppingCartUpdates` Yapısı tüm alışveriş sepeti öğeleri yerleştirmek için kullanılır. `UpdateShoppingCartDatabase` Yöntemi kullanan `ShoppingCartUpdates` öğelerden herhangi birini güncelleştirilmiş veya kaldırılması gerekip gerekmediğini belirlemek için yapısı.

Sonraki öğreticide kullanacağınız `EmptyCart` alışveriş temizlemek için yöntemi Sepeti ürünleri satın aldıktan sonra. Ancak şu an için kullanacağınız `GetCount` için eklediğiniz yöntemi *ShoppingCartActions.cs* alışveriş sepeti öğe sayısını belirlemek için dosya.

### <a name="adding-a-shopping-cart-counter"></a>Bir alışveriş sepeti sayaç ekleme

Alışveriş sepeti içinde toplam öğe sayısını görüntülemek izin vermek için bir sayaç ekleyecek *Site.Master* sayfası. Bu sayaç, alışveriş sepeti bağlantı olarak da hareket eder.

1. İçinde **Çözüm Gezgini**, açık *Site.Master* sayfası.
2. İşaretleme sarı gezinti bölümüne aşağıdaki gibi göründüğü şekilde gösterildiği gibi alışveriş sepeti sayaç bağlantı ekleyerek değiştirin:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Ardından, arka plan kodu, güncelleştirme *Site.Master.cs* dosyasını sarı ile aşağıdaki gibi vurgulanmış kodu ekleyerek düzenleyin:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Sayfanın HTML olarak işlenmeden önce `Page_PreRender` olayı oluşturulur. İçinde `Page_PreRender` işleyici, alışveriş sepeti toplam sayısı, çağıran belirlenir `GetCount` yöntemi. Döndürülen değer eklenen `cartCount` biçimlendirmede dahil yayılma *Site.Master* sayfası. `<span>` Etiketleri düzgün işlenecek iç öğeleri sağlar. Sitenin herhangi bir sayfayı görüntülendiğinde, alışveriş sepeti toplam görüntülenir. Kullanıcı, alışveriş sepeti görüntülenecek alışveriş sepeti toplam tıklatabilirsiniz.

## <a name="testing-the-completed-shopping-cart"></a>Tamamlanan alışveriş sepeti test etme

Alışveriş sepeti içinde nasıl ekleyebileceğiniz görmek için uygulamayı şimdi, silme ve güncelleştirme öğeleri çalıştırabilirsiniz. Alışveriş sepeti toplam alışveriş sepeti tüm öğelerde toplam maliyeti yansıtır.

1. Tuşuna **F5** uygulamayı çalıştırın.  
 Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Seçin **araba** kategori Gezinti menüsünde.
3. Tıklatın **Sepete Ekle** ilk ürün yanındaki bağlantı.   
 *ShoppingCart.aspx* sayfası, sipariş toplamı ile gösterilir.
4. Seçin **düzlemleri** kategori Gezinti menüsünde.
5. Tıklatın **Sepete Ekle** ilk ürün yanındaki bağlantı.
6. 3 alışveriş sepetine ilk öğe miktarını kümesindeki ve seçin **öğeyi Kaldır** ikinci öğenin onay kutusunu.<a id="a"></a>
7. ' I tıklatın **güncelleştirme** düğmesine alışveriş sepeti sayfasını güncelleştirmek ve yeni sipariş toplama görüntüler. 

    ![Alışveriş sepeti - Sepeti güncelleştirme](shopping-cart/_static/image9.png)

## <a name="summary"></a>Özet

Bu öğreticide, alışveriş sepeti Wingtip Toys Web Forms örnek uygulama için oluşturdunuz. Entity Framework Code First, veri ek açıklamaları, kesin türü belirtilmiş veri denetimleri ve model bağlama sırasında Bu öğreticide kullandığınız.

Alışveriş sepeti ekleme, silme ve kullanıcı için satın seçilmiş öğeler güncelleştiriliyor destekler. Alışveriş sepeti işlevselliği uygulamadan yanı sıra, alışveriş sepeti öğeleri görüntülemek nasıl öğrendiniz bir **GridView** denetlemek ve sipariş toplamı hesaplayın.

## <a name="addition-information"></a>Ek bilgi

[ASP.NET oturum durumuna genel bakış](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Önceki](display_data_items_and_details.md)
> [sonraki](checkout-and-payment-with-paypal.md)
