---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Alışveriş sepeti | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 2823970144e6dce4a3a9f4d28dabfabe9fe053d6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753616"
---
<a name="shopping-cart"></a>Alışveriş sepeti
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide, Wingtip Toys örnek ASP.NET Web Forms uygulaması için bir alışveriş sepetine eklemek için gerekli iş mantığı açıklanmaktadır. Bu öğreticide, önceki öğreticide "Görünen veri öğelerini ve Ayrıntıları" oluşturur ve Wingtip çocuğunun Store öğretici serisinin bir parçasıdır. Bu öğreticiyi tamamladığınızda, örnek uygulamanızı kullanıcıları eklemek, kaldırmak ve bunların alışveriş sepeti ürünleri değiştirmek mümkün olacaktır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

1. Alışveriş sepeti web uygulaması oluşturma
2. Öğe, alışveriş sepetine eklemek kullanıcılara tanıma.
3. Nasıl ekleneceği bir [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) alışveriş sepeti ayrıntılarını görüntülemek için denetim.
4. Hesaplamak ve toplam sırayı görüntülemek nasıl.
5. Nasıl kaldırılacağı ve alışveriş sepetini güncelleştirme öğeleri.
6. Alışveriş sepeti sayaç ekleme.

## <a name="code-features-in-this-tutorial"></a>Bu öğreticideki kod özellikleri:

1. Entity Framework Code First
2. Veri ek açıklamaları
3. Kesin türü belirtilmiş veri denetimleri
4. Model bağlama

## <a name="creating-a-shopping-cart"></a>Bir alışveriş sepeti oluşturmak

Daha önce Bu öğretici serisinde, sayfalar ve veritabanından ürün verilerini görüntülemek için kod eklendi. Bu öğreticide, ürünleri yönetmek için bir alışveriş sepeti oluşturacaksınız kullanıcılar satın alma ilgileniyor olduğunu. Kullanıcıların göz atın ve bunlar katılmamaları kayıtlı veya oturum açmış olsanız bile, alışveriş sepetine öğe ekleme mümkün olacaktır. Alışveriş sepeti erişimi yönetmek için kullanıcıların benzersiz bir atar `ID` sepete ilk kez kullanıcı alışveriş eriştiğinde bir genel benzersiz tanıtıcısı (GUID) kullanarak. Bu depolayacağınızı `ID` ASP.NET oturum durumu kullanma.

> [!NOTE] 
> 
> ASP.NET oturum durumu, kullanıcının site ayrılmasından sonra süresi dolacak kullanıcıya özgü bilgileri depolamak için kullanışlı bir yerdir. Oturum durumu b.internet performans etkilerinin daha büyük sitelerinde olabilse de tanıtım amacıyla oturum durumu çalışır kullanımını açık. Wingtip Toys örnek proje, bir dış sağlayıcı olmadan oturum durumu depolanan işlem içi siteyi barındıran web sunucusu üzerinde olduğu oturum durumu kullanma işlemini gösterir. Bir uygulamanın birden çok örneği sağlamak büyük siteleri veya farklı sunucularda bir uygulama birden çok örneğini çalıştıran siteler için kullanmayı **Windows Azure önbellek hizmeti**. Bu önbellek hizmeti, dış web sitesine gidin ve işlem içi oturum durumu kullanmanın sorunu çözdü dağıtılmış bir önbelleğe alma hizmeti sağlar. Daha fazla bilgi edinmek, [ASP.NET oturum durumunu Windows Azure Web siteleriyle nasıl](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Bir Model sınıfı CartItem ekleme

Daha önce Bu öğretici serisinde, kategori ve ürün veriler için şema oluşturarak tanımladığınız `Category` ve `Product` sınıfları *modelleri* klasör. Şimdi, alışveriş sepetine şemasını tanımlamak için yeni bir sınıf ekleyin. Daha sonra Bu öğreticide, veri erişimi işlemek için bir sınıf ekleyeceksiniz `CartItem` tablo. Bu sınıf, iş mantığı eklemek, kaldırmak ve öğeleri alışveriş sepetini güncelleştirme sağlayacaktır.

1. Sağ *modelleri* klasörü ve select **Ekle**  - &gt; **yeni öğe**. 

    ![Alışveriş sepeti - yeni öğe](shopping-cart/_static/image1.png)
2. **Yeni Öğe Ekle** iletişim kutusu görüntülenir. Seçin **kod**ve ardından **sınıfı**. 

    ![Alışveriş sepeti - yeni öğesi ekleme](shopping-cart/_static/image2.png)
3. Bu yeni bir sınıf adı *CartItem.cs*.
4. **Ekle**'yi tıklatın.  
   Yeni bir sınıf dosyası Düzenleyicisi'nde görüntülenir.
5. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem` Sınıfı, bir kullanıcı ekler alışveriş sepetine her ürün tanımlayan şemayı içerir. Bu sınıf, Bu öğretici serisinde daha önce oluşturduğunuz diğer şema sınıfları benzerdir. Kural gereği, Entity Framework Code First, bekliyor için birincil anahtarı `CartItem` tablo ya da olacak `CartItemId` veya `ID`. Ancak, kod varsayılan davranışı veri ek açıklama kullanarak geçersiz kılar `[Key]` özniteliği. `Key` ItemId özelliğinin öznitelik belirtir `ItemID` birincil anahtar özelliğidir.

`CartId` Özellik belirtir `ID` satın almak için öğesiyle ilişkili kullanıcı. Bu kullanıcı oluşturmak için kod ekleyeceksiniz `ID` zaman alışveriş sepetini erişen kullanıcı. Bu `ID` bir ASP.NET oturum değişkeni depolanır.

### <a name="update-the-product-context"></a>Ürün içeriği güncelleştirme

Ekleme yanı sıra `CartItem` sınıfı, varlık sınıflarının yönetir ve veritabanı veri erişim sağlayan veritabanı bağlamı sınıfının güncelleştirmesi gerekir. Bunu yapmak için yeni oluşturulan ekleyeceksiniz `CartItem` model sınıfı için `ProductContext` sınıfı.

1. İçinde **Çözüm Gezgini**, bulma ve açma *ProductContext.cs* dosyası *modelleri* klasör.
2. Vurgulanmış kodu ekleyin *ProductContext.cs* aşağıdaki gibi:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Bu öğretici serisindeki kodda daha önce belirtildiği gibi *ProductContext.cs* dosyası ekler `System.Data.Entity` ad alanı Entity Framework'ün tüm çekirdek işlevlerin erişiminiz olduğunu. Bu işlev, sorgulama, ekleme, güncelleştirme ve kesin olarak belirlenmiş nesneler ile çalışma ile verileri silme özelliği içerir. `ProductContext` Sınıfı erişim için yeni eklenen ekler `CartItem` model sınıfı.

### <a name="managing-the-shopping-cart-business-logic"></a>Alışveriş sepeti iş mantığı yönetme

Ardından, oluşturacağınız `ShoppingCart` yeni bir sınıfta *mantıksal* klasör. `ShoppingCart` Sınıfı işler veri erişimi `CartItem` tablo. Sınıfı ayrıca iş mantığı eklemek, kaldırmak ve öğeleri alışveriş sepetini güncelleştirme içerir.

Aşağıdaki eylemleri yönetmek için gereken işlevleri ekleyeceğiniz alışveriş sepeti mantığı içerir:

1. Alışveriş sepetine öğe ekleme
2. Alışveriş sepeti öğeleri kaldırma
3. Alışveriş sepeti Kimliğini alma
4. Alışveriş sepeti öğeleri alınıyor
5. Toplam miktarı tüm öğelerin alışveriş sepeti
6. Alışveriş sepeti verileri güncelleştirme

Bir alışveriş sepeti sayfası (*ShoppingCart.aspx*) ve alışveriş sepeti sınıfı birlikte alışveriş sepeti verilere erişmek için kullanılır. Alışveriş sepeti sayfası kullanıcı alışveriş sepetine ekler tüm öğeleri görüntüler. Alışveriş sepeti yanında, sayfa ve sınıf, bir sayfa oluşturacaksınız (*AddToCart.aspx*) ürün alışveriş sepetine eklemek için. Ayrıca kod ekleyeceksiniz *ProductList.aspx* sayfası ve *ProductDetails.aspx* bağlantı sağlayacak bir sayfa *AddToCart.aspx* sayfasında, böylece kullanıcı ekleyebilirsiniz alışveriş sepetine ürünleri.

Aşağıdaki diyagramda, kullanıcı bir ürün alışveriş sepetine eklediğinde gerçekleşen temel işlemi gösterilmektedir.

![Alışveriş sepeti - alışveriş sepetine ekleme](shopping-cart/_static/image3.png)

Kullanıcı tıkladığında **Sepete Ekle** ya da bağlantıyı *ProductList.aspx* sayfası veya *ProductDetails.aspx* sayfasında, uygulama gezinmek için *AddToCart.aspx* sayfası ve ardından otomatik olarak *ShoppingCart.aspx* sayfası. *AddToCart.aspx* sayfası eklenir select ürün alışveriş sepetine ShoppingCart sınıfta bir yöntemi çağırarak. *ShoppingCart.aspx* sayfası, alışveriş sepetine eklendi ürünleri görüntüler.

#### <a name="creating-the-shopping-cart-class"></a>Alışveriş sepeti sınıfı oluşturma

`ShoppingCart` Sınıfı eklenecek uygulama ayrı bir klasörde model (model klasör), sayfa (kök klasöründe) ve (mantıksal klasör) mantıksal arasında NET bir ayrıma böylece olacaktır.

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**seçin ve proje **Ekle**-&gt;**yeni klasör**. Yeni klasör adı *mantıksal*.
2. Sağ *mantıksal* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.
3. Adlı yeni bir sınıf dosyası ekleyin *ShoppingCartActions.cs*.
4. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart` Yöntemi etkinleştirir ürüne bağlı olarak alışveriş sepetini dahil edilecek tek tek ürünlerin `ID`. Ürün sepete eklenir veya sepete ürün için bir öğe içeriyorsa, miktar artırılır.

`GetCartId` Yöntemi döndürür sepet `ID` kullanıcı. Sepet `ID` , alışveriş sepeti kullanıcının öğelerini izlemek için kullanılır. Kullanıcı mevcut bir sepet yoksa `ID`, yeni bir sepet `ID` kendileri için oluşturulur. Kullanıcının kayıtlı kullanıcı olarak, sepet açan, `ID` kendi kullanıcı adına ayarlayın. Ancak, kullanıcı arabasında imzalı değilse `ID` benzersiz bir değere (GUID) ayarlayın. Bu yalnızca bir sepet oturum tabanlı her bir kullanıcı için oluşturulmuş bir GUID sağlar.

`GetCartItems` Yöntemi alışveriş sepeti öğeleri kullanıcının bir listesini döndürür. Bu öğreticinin ilerleyen bölümlerinde model bağlama alışveriş sepeti kullanımında sepet öğeleri görüntülemek için kullanıldığını görürsünüz `GetCartItems` yöntemi.

### <a name="creating-the-add-to-cart-functionality"></a>Sepet ile ekleme işlevi oluşturma

Daha önce bahsedildiği gibi bir işleme sayfası oluşturacak *AddToCart.aspx* kullanıcının alışveriş sepetine yeni ürün eklemek için kullanılır. Bu sayfa çağıracak `AddToCart` yönteminde `ShoppingCart` oluşturduğunuz sınıfı. *AddToCart.aspx* sayfası, beklediğiniz bir ürün `ID` kendisine geçirilir. Bu ürün `ID` çağrılırken kullanılacak `AddToCart` yönteminde `ShoppingCart` sınıfı.

> [!NOTE] 
> 
> Arka plan kod değiştirme (*AddToCart.aspx.cs*) UI sayfada değil Bu sayfanın (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Add-Sepeti oluşturmak için işlevselliği:

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**proje, tıklayın **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Standart yeni sayfa (Web formu) adlı bir uygulamaya ekleme *AddToCart.aspx*. 

    ![Alışveriş sepeti - Web formu ekleyin](shopping-cart/_static/image4.png)
3. İçinde **Çözüm Gezgini**, sağ *AddToCart.aspx* sayfasında ve ardından **kodu görüntüle**. *AddToCart.aspx.cs* arka plan kod dosyası düzenleyicide açılır.
4. Varolan kodda değiştirin *AddToCart.aspx.cs* arka plan kod aşağıdaki:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Zaman *AddToCart.aspx* sayfası yüklendikten, ürün `ID` Sorgu dizesinden alınır. Ardından, alışveriş sepeti sınıfının bir örneği oluşturulur ve çağırmak için kullanılan `AddToCart` Bu öğreticide daha önce eklediğiniz yöntemi. `AddToCart` Yöntemi, bulunan *ShoppingCartActions.cs* dosya, seçilen ürün alışveriş sepetine eklemek veya seçili ürünün ürün miktarını artırmak için mantığı içerir. Ürün alışveriş sepetine eklenmemişse, ürün eklenen `CartItem` veritabanı tablosu. Ürün alışveriş sepetine eklendi ve aynı ürün ek bir öğenin kullanıcı ekler, ürün miktarını içinde artırılır `CartItem` tablo. Son olarak, geri sayfasına yönlendiren *ShoppingCart.aspx* sayfasını, sonraki adımda, burada kullanıcı sepet öğelerde güncelleştirilmiş bir listesini görür ekleyeceksiniz.

Daha önce belirtildiği gibi bir kullanıcı `ID` , belirli bir kullanıcıyla ilişkili ürünleri tanımlamak için kullanılır. Bu `ID` bir satır eklenir `CartItem` tablo kullanıcı, bir ürün alışveriş sepetine eklediği her durumda.

### <a name="creating-the-shopping-cart-ui"></a>Alışveriş sepeti kullanıcı Arabirimi oluşturma

*ShoppingCart.aspx* sayfası, kullanıcının kendi alışveriş sepetine eklendi ürünleri görüntüler. Bu da eklemek, kaldırmak ve öğeleri alışveriş sepetini güncelleştirme olanağı sağlayacaktır.

1. İçinde **Çözüm Gezgini**, sağ **WingtipToys**, tıklayın **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Ana sayfayı seçerek içeren yeni bir sayfa (Web formu) ekleme **ana sayfa kullanan Web formu**. Yeni sayfa adı *ShoppingCart.aspx*.
3. Seçin **Site.Master** ana sayfa için yeni oluşturulan eklemek için *.aspx* sayfası.
4. İçinde *ShoppingCart.aspx* sayfasında, aşağıdaki biçimlendirme mevcut biçimlendirme değiştirin:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx* sayfasını içeren bir **GridView** adlı Denetim `CartList`. Bu denetimi model bağlama için veritabanından alışveriş sepeti veri bağlamak için kullanır. **GridView** denetimi. Ayarladığınızda `ItemType` özelliği **GridView** denetimine veri bağlama ifadesi `Item` kullanılabilir biçimlendirme ve denetimi, türü kesin belirlenmiş olur. Bu öğretici serisinde daha önce bahsedildiği gibi ayrıntılarını seçebileceğiniz `Item` IntelliSense kullanarak nesne. Model bağlama verileri seçmek için kullanılacak veri denetimi yapılandırmak için ayarladığınız `SelectMethod` denetiminin özelliği. Yukarıdaki biçimlendirme ayarladığınız `SelectMethod` listesini döndüren GetShoppingCartItems yöntemi kullanmak üzere `CartItem` nesneleri. **GridView** veri denetimi sayfa yaşam döngüsü içinde uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. `GetShoppingCartItems` Yöntemi yine de eklenmelidir.

#### <a name="retrieving-the-shopping-cart-items"></a>Alışveriş sepeti öğeleri alınıyor

Ardından, kod eklediğiniz *ShoppingCart.aspx.cs* almak ve alışveriş sepeti UI doldurmak için gerideki kod.

1. İçinde **Çözüm Gezgini**, sağ *ShoppingCart.aspx* sayfasında ve ardından **kodu görüntüle**. *ShoppingCart.aspx.cs* arka plan kod dosyası düzenleyicide açılır.
2. Varolan kodu aşağıdakiyle değiştirin:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Yukarıda belirtildiği gibi `GridView` verilerinin denetimini çağrıları `GetShoppingCartItems` yöntemi Sayfası'nda uygun zamanda döngüsü ve döndürülen verileri otomatik olarak bağlar. `GetShoppingCartItems` Yöntemi bir örneğini oluşturur `ShoppingCartActions` nesne. Ardından, kod çağırarak arabasında öğeleri döndürmek için bu örneği kullanır `GetCartItems` yöntemi.

### <a name="adding-products-to-the-shopping-cart"></a>Alışveriş sepetine ürünleri ekleme

Zaman ya da *ProductList.aspx* veya *ProductDetails.aspx* sayfası görüntülenirse, kullanıcı ürün bağlantı kullanarak bir alışveriş sepetine eklemek mümkün olacaktır. Bağlantıya tıkladıklarında, uygulama adlı işleme sayfasına gider *AddToCart.aspx*. *AddToCart.aspx* sayfa çağıracaktır `AddToCart` yönteminde `ShoppingCart` Bu öğreticide daha önce eklediğiniz sınıfı.

Şimdi, ekleyeceksiniz bir **Sepete Ekle** her iki bağlantı *ProductList.aspx* sayfası ve *ProductDetails.aspx* sayfası. Bu bağlantı, ürün bulunur `ID` veritabanından alınır.

1. İçinde **Çözüm Gezgini**bulun ve adlı sayfasını açın *ProductList.aspx*.
2. Sarı ile vurgulanmış biçimlendirme eklemek *ProductList.aspx* sayfanın tamamını aşağıdaki gibi görünür, böylece sayfa:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Alışveriş sepeti test etme

Ürün alışveriş sepetine eklemek nasıl görmek için uygulamayı çalıştırın.

1. Tuşuna **F5** uygulamayı çalıştırın.  
 Proje veritabanını yeniden oluşturur sonra tarayıcıyı açın Göster ve *Default.aspx* sayfası.
2. Seçin **otomobiller** kategori Gezinti menüsünde.  
 *ProductList.aspx* sayfası, yalnızca "Arabalar" kategorisindeki ürünlerin gösteren görüntülenir. 

    ![Alışveriş sepeti - otomobiller](shopping-cart/_static/image5.png)
3. Tıklayın **Sepete Ekle** ilk ürün yanındaki bağlantısını (dönüştürülebilir car) listelenir.   
 *ShoppingCart.aspx* sayfası görüntülenirse, seçimi, alışveriş sepetine gösteriliyor. 

    ![Alışveriş sepeti - Sepeti](shopping-cart/_static/image6.png)
4. Ek ürünler seçerek görüntülemek **düzlemleri** kategori Gezinti menüsünde.
5. Tıklayın **Sepete Ekle** listelenen ilk ürün yanındaki bağlantı.  
 *ShoppingCart.aspx* ek öğeyle sayfası görüntülenir.
6. Tarayıcıyı kapatın.

### <a name="calculating-and-displaying-the-order-total"></a>Hesaplama ve toplam sırayı görüntüleme

Alışveriş sepetine ürünleri eklemenin yanı sıra ekleyeceksiniz bir `GetTotal` yönteme `ShoppingCart` sınıfı ve toplam sipariş miktarı alışveriş sepeti sayfasında görüntüleyebilirsiniz.

1. İçinde **Çözüm Gezgini**açın *ShoppingCartActions.cs* dosyası *mantıksal* klasör.
2. Aşağıdakileri ekleyin `GetTotal` yöntemi için sarı renkle vurgulanmış `ShoppingCart` sınıfı böylece sınıfı şu şekilde görünür:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

İlk olarak, `GetTotal` yöntemi, kullanıcı için alışveriş sepetini Kimliğini alır. Ardından yöntemi sepete ürün fiyatı arabasına listelenen her ürün için ürün miktarı çarparak toplam alır.

> [!NOTE] 
> 
> Yukarıdaki kod, boş değer atanabilir tür kullanır "`int?`". Tüm değerleri bir temel alınan türü ve null değer olarak boş değer atanabilir türleri temsil edebilir. Daha fazla bilgi edinmek, [kullanarak boş değer atanabilir türler](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Alışveriş sepeti görüntüle değiştirme

Sonraki kodu değiştireceksiniz *ShoppingCart.aspx* çağırmak için sayfayı `GetTotal` yöntemi ve toplam görüntüleme *ShoppingCart.aspx* sayfa sayfa yüklendiğinde.

1. İçinde **Çözüm Gezgini**, sağ *ShoppingCart.aspx* sayfasından seçim yapıp **kodu görüntüle**.
2. İçinde *ShoppingCart.aspx.cs* dosyası, güncelleştirme `Page_Load` sarı ile vurgulanmış aşağıdaki kodu ekleyerek işleyicisi:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Zaman *ShoppingCart.aspx* sayfayı yüklemez, alışveriş sepeti nesne yükler ve ardından çağırarak alışveriş sepeti toplam alır `GetTotal` yöntemi `ShoppingCart` sınıfı. Alışveriş sepeti boşsa, bir ileti belirlememişse bu bağlamda görüntülenir.

### <a name="testing-the-shopping-cart-total"></a>Alışveriş sepeti toplam test etme

Nasıl yalnızca bir ürün alışveriş sepetine ekleyebilirsiniz değil görmek için uygulamayı şimdi çalıştırmak, ancak alışveriş sepeti toplam görebilirsiniz.

1. Tuşuna **F5** uygulamayı çalıştırın.  
 Bir tarayıcı açın ve göstermek *Default.aspx* sayfası.
2. Seçin **otomobiller** kategori Gezinti menüsünde.
3. Tıklayın **Sepete Ekle** ilk ürün yanındaki bağlantı.   
 *ShoppingCart.aspx* toplam sırayı sayfası görüntülenir. 

    ![Alışveriş sepeti - alışveriş sepetini toplam](shopping-cart/_static/image7.png)
4. Bazı diğer ürünleri (örneğin, uçak) Sepete ekleyin.
5. *ShoppingCart.aspx* eklediğiniz tüm ürünler için güncelleştirilmiş bir toplam ile sayfası görüntülenir. 

    ![Alışveriş sepeti - birden çok ürünlerin](shopping-cart/_static/image8.png)
6. Tarayıcı penceresini kapatarak çalışan uygulamayı durdurun.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Alışveriş sepetine ekleme, güncelleştirme ve kullanıma alma düğmeleri

Alışveriş sepeti değiştirmek kullanıcılara izin vermek için ekleyeceksiniz bir **güncelleştirme** düğmesi ve **kullanıma alma** alışveriş sepeti sayfası düğmesi. **Kullanıma alma** düğmesi, daha sonra Bu öğretici serisindeki kadar kullanılmaz.

1. İçinde **Çözüm Gezgini**açın *ShoppingCart.aspx* web uygulaması projesi kökündeki sayfası.
2. Eklenecek **güncelleştirme** düğmesi ve **kullanıma alma** düğmesi *ShoppingCart.aspx* sayfasında, mevcut biçimlendirmeye sarı renkle vurgulanmış biçimlendirme gösterildiği gibi ekleyin Aşağıdaki kodu:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Kullanıcı tıkladığında **güncelleştirme** düğme `UpdateBtn_Click` olay işleyicisi çağrılır. Bu olay işleyicisi, sonraki adımda ekleyeceksiniz kodu çağırır.

Ardından, içerdiği kodu güncelleştirebilirsiniz *ShoppingCart.aspx.cs* Sepeti öğeleri ve çağrısı döngü dosyaya `RemoveItem` ve `UpdateItem` yöntemleri.

1. İçinde **Çözüm Gezgini**açın *ShoppingCart.aspx.cs* web uygulama projesinin kök dosyasında.
2. Sarı ile vurgulanmış kodu aşağıdaki bölümlerde ekleme *ShoppingCart.aspx.cs* dosyası:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Kullanıcı tıkladığında **güncelleştirme** düğmesini *ShoppingCart.aspx* sayfasında UpdateCartItems yöntemi çağrılır. UpdateCartItems yöntemi alışveriş sepetini her öğe için güncelleştirilmiş değerleri alır. Ardından, UpdateCartItems yöntemi çağıran `UpdateShoppingCartDatabase` (eklenir ve bir sonraki adımda açıklanan) yöntemi ekleyebilir veya alışveriş sepetini öğeleri kaldırmak için. Veritabanı güncelleştirmelerini alışveriş sepetine yansıtacak şekilde güncelleştirildi sonra **GridView** denetim çağırarak alışveriş sepeti sayfada güncelleştirilir `DataBind` yöntemi **GridView**. Ayrıca, alışveriş sepeti sayfası toplam sipariş miktarı, güncelleştirilmiş öğelerin listesini yansıtacak şekilde güncelleştirilir.

### <a name="updating-and-removing-shopping-cart-items"></a>Güncelleştirme ve alışveriş sepetine öğe kaldırma

Üzerinde *ShoppingCart.aspx* sayfasında, bir öğe miktarını güncelleştiriliyor ve bir öğe kaldırmak için Denetim eklenmiştir görebilirsiniz. Şimdi çalışması için bu denetimleri yapar kodu ekleyin.

1. İçinde **Çözüm Gezgini**açın *ShoppingCartActions.cs* dosyası *mantıksal* klasör.
2. Sarı ile vurgulanmış aşağıdaki kodu ekleyin *ShoppingCartActions.cs* sınıf dosyası:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase` Yöntemi çağrılır, `UpdateCartItems` metodunda *ShoppingCart.aspx.cs* sayfasında, güncelleştirmek veya alışveriş sepetini öğeleri kaldırmak için mantığı içerir. `UpdateShoppingCartDatabase` Yöntemi yinelenir alışveriş sepeti listesindeki tüm satırların üzerinden. Kaldırılacak bir alışveriş sepeti öğesi işaretlenmiş veya değerden daha az miktardır `RemoveItem` yöntemi çağrılır. Aksi takdirde, alışveriş sepetine öğe için teslim zaman güncelleştirmeleri `UpdateItem` yöntemi çağrılır. Alışveriş sepeti öğesi kaldırıldı veya güncelleştirildikten sonra veritabanı değişikliklerini kaydedilir.

`ShoppingCartUpdates` Yapısı, alışveriş sepeti öğeleri tutmak için kullanılır. `UpdateShoppingCartDatabase` Yöntemi kullanan `ShoppingCartUpdates` yapısı herhangi bir öğeyi güncelleştirilmesi veya kaldırılması gerekip gerekmediğini belirlemek için.

Sonraki öğreticide kullanacağınız `EmptyCart` alışveriş temizlemek için yöntemi sepete ürün satın aldıktan sonra. Ancak şu an için kullanacağınız `GetCount` eklendiğiniz yöntemi *ShoppingCartActions.cs* alışveriş sepetine öğe sayısını belirlemek için dosya.

### <a name="adding-a-shopping-cart-counter"></a>Alışveriş sepeti sayaç ekleme

Alışveriş sepeti toplam öğe sayısını görüntülemek izin vermek için bir sayaç ekleyeceksiniz *Site.Master* sayfası. Bu sayaç, alışveriş sepetine bağlantısını da görecek.

1. İçinde **Çözüm Gezgini**açın *Site.Master* sayfası.
2. Biçimlendirme, aşağıdaki gibi görünecek şekilde sarı Gezinti bölümde gösterildiği alışveriş sepeti sayacı bağlantısı ekleyerek değiştirin:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Ardından, arka plan kod, güncelleştirme *Site.Master.cs* gibi sarı ile vurgulanmış kodu ekleyerek dosyası:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Sayfanın HTML olarak işlenmeden önce `Page_PreRender` olayı oluşturulur. İçinde `Page_PreRender` toplam sayısını, alışveriş sepetine işleyicisini çağırarak belirlenir `GetCount` yöntemi. Döndürülen değer eklenir `cartCount` yayılma biçimlendirmede dahil *Site.Master* sayfası. `<span>` Etiketleri düzgün işlenecek iç öğeleri sağlar. Sitenin herhangi bir sayfanın görüntülendiğinde, alışveriş sepeti toplam görüntülenir. Kullanıcı, alışveriş sepetine görüntülenecek alışveriş sepeti toplam de tıklayabilirsiniz.

## <a name="testing-the-completed-shopping-cart"></a>Tamamlanan alışveriş sepetini test etme

Nasıl ekleyebileceğinizi görmek için uygulamayı şimdi, silme ve güncelleştirme öğe alışveriş sepetine çalıştırabilirsiniz. Tüm öğelerin alışveriş sepetine toplam maliyeti alışveriş sepeti toplam ücreti yansıtılır.

1. Tuşuna **F5** uygulamayı çalıştırın.  
 Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Seçin **otomobiller** kategori Gezinti menüsünde.
3. Tıklayın **Sepete Ekle** ilk ürün yanındaki bağlantı.   
 *ShoppingCart.aspx* toplam sırayı sayfası görüntülenir.
4. Seçin **düzlemleri** kategori Gezinti menüsünde.
5. Tıklayın **Sepete Ekle** ilk ürün yanındaki bağlantı.
6. İlk öğe miktarını alışveriş sepetine 3 olarak ayarlayın ve seçin **öğeyi Kaldır** onay kutusunun ikinci öğe.<a id="a"></a>
7. Tıklayın **güncelleştirme** düğmesine alışveriş sepeti sayfası ve yeni sipariş toplamını görüntüler. 

    ![Alışveriş sepeti - alışveriş sepetini güncelleştirme](shopping-cart/_static/image9.png)

## <a name="summary"></a>Özet

Bu öğreticide, Wingtip Toys Web Forms örnek uygulama için bir alışveriş sepeti oluşturdunuz. Bu öğretici sırasında Entity Framework Code First, veri ek açıklamaları, kesin türü belirtilmiş veri denetimleri ve model bağlama kullandınız.

Alışveriş sepeti, ekleme, silme ve güncelleştirme satın alma için kullanıcının seçtiği öğeleri destekler. Alışveriş sepeti işlevselliği uygulamak yanı sıra, alışveriş sepeti öğeleri görüntülemek nasıl öğrendiniz bir **GridView** denetlemek ve sipariş toplamını hesaplar.

## <a name="addition-information"></a>Ek bilgi

[ASP.NET oturum durumuna genel bakış](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Önceki](display_data_items_and_details.md)
> [İleri](checkout-and-payment-with-paypal.md)
