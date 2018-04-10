---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Kullanıma alma ve PayPal ödeme | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="checkout-and-payment-with-paypal"></a>Kullanıma alma ve PayPal ödeme
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğretici, kullanıcı kimlik doğrulaması, kayıt ve PayPal kullanarak ödeme içerecek şekilde Wingtip Toys örnek uygulamayı değiştirmek açıklar. Oturum açan kullanıcıların ürünleri satın almak için yetkilendirme gerekir. ASP.NET 4.5 Web formları proje şablonun yerleşik kullanıcı kayıt işlevselliği zaten gerekenler çoğunu içerir. PayPal Express Checkout işlevselliği ekleyeceksiniz. Bu öğreticide, gerçek bir fon aktarılacak şekilde sınama ortamında, PayPal Geliştirici kullanıyor. Öğreticinin sonunda alışveriş sepeti, ödeme düğmesini tıklatarak ve verileri PayPal test web sitesine aktarma eklemek için ürün seçerek uygulamayı test. PayPal test web sitesinde sevkiyat ve Ödeme bilgilerinizi doğrulayın ve ardından onaylamak ve satın alma işlemini tamamlamak için yerel Wingtip Toys örnek uygulama için döndürür.

Bu adres ölçeklenebilirlik ve güvenlik çevrimiçi alışveriş specialize birkaç deneyimli üçüncü taraf ödeme işlemciler vardır. ASP.NET geliştiricilerinin bir alışveriş uygulama ve çözüm satın almadan önce bir üçüncü taraf ödeme çözümü kullanan yararları göz önünde bulundurmalısınız.

> [!NOTE] 
> 
> Wingtip Toys örnek uygulama, ASP.NET web geliştiricilerin belirli ASP.NET kavramları ve özellikler kullanılabilir gösterildiği şekilde tasarlanmıştır. Bu örnek uygulama ölçeklenebilirlik ve güvenlik in regard to tüm olası durumlarda iyi duruma getirilmiş değil.


## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Bir klasördeki belirli sayfalara erişimi kısıtlamak nasıl.
- Anonim bir Alışveriş sepetinden bilinen alışveriş sepeti oluşturma
- Nasıl proje için SSL etkinleştirilir.
- OAuth sağlayıcısı projeye ekleme.
- PayPal PayPal sınama ortamı kullanarak ürün satın almak için nasıl kullanılacağını.
- İçinde PayPal ayrıntılarını görüntülemek nasıl bir **DetailsView** denetim.
- PayPal elde edilen ayrıntılarla Wingtip Toys uygulamasının veritabanını güncelleştirmek nasıl.

## <a name="adding-order-tracking"></a>Sipariş İzleme ekleme

Bu öğreticide, bir kullanıcının oluşturduğu siparişte veri izlemek için iki yeni sınıflar oluşturacaksınız. Sınıfları veri aktarma bilgileri, satın alma toplam ve ödeme onayı izler.

### <a name="add-the-order-and-orderdetail-model-classes"></a>OrderDetail modeli sınıfları ve sırası Ekle

Öğretici serisinde daha önce bu kategoriler, ürünler, şema tanımlanan ve alışveriş sepeti öğeleri oluşturarak `Category`, `Product`, ve `CartItem` sınıfları *modelleri* klasör. Artık ürün sırası ve Sipariş ayrıntılarını şemasını tanımlamak için iki yeni sınıflar ekleyeceksiniz.

1. İçinde **modelleri** klasör adında yeni bir sınıf ekleyin *Order.cs*.   
   Yeni sınıf dosyası Düzenleyicisi'nde görüntülenir.
2. Varsayılan kodu aşağıdakilerle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Ekleme bir *OrderDetail.cs* sınıfının *modelleri* klasör.
4. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order` Ve `OrderDetail` satın alma ve aktarma için kullanılan sipariş bilgilerini tanımlamak için şema sınıfları içerir.

Ayrıca, varlık sınıflarını yönetir ve veritabanına veri erişimi sağlayan veritabanı bağlamı sınıfının güncelleştirmeniz gerekir. Bunu yapmak için yeni oluşturulan sipariş ekleyecek ve `OrderDetail` model sınıflarına `ProductContext` sınıfı.

1. İçinde **Çözüm Gezgini**, bulma ve açma *ProductContext.cs* dosya.
2. Vurgulanmış kodu ekleyin *ProductContext.cs* dosya aşağıda gösterildiği gibi:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Bu öğretici serisinde kodu daha önce belirtildiği gibi *ProductContext.cs* dosyayı ekler `System.Data.Entity` ad alanı Entity Framework'ün tüm çekirdek işlevlerin erişimi böylece. Bu işlevsellik, sorgu, Ekle, Güncelleştir ve güçlü şekilde yazılan nesnelerin çalışarak verilerini silmek için özelliği içerir. Yukarıdaki kod `ProductContext` sınıfı Entity Framework erişim için yeni eklenen ekler `Order` ve `OrderDetail` sınıfları.

## <a name="adding-checkout-access"></a>Checkout erişim ekleme

Wingtip Toys örnek uygulama gözden geçirin ve ürünleri alışveriş sepetine eklemek anonim kullanıcıların sağlar. Anonim kullanıcılar alışveriş sepetine eklenen ürünlerin seçtiğinizde, ancak bunlar siteye oturum açmalısınız. Bunlar oturum açtıktan sonra bunların checkout işleyen ve satın alma işlemi kısıtlı sayfalarına Web uygulamasının erişebilirsiniz. Bu sınırlı sayfalar içerdiği *Checkout* uygulamanın klasör.

### <a name="add-a-checkout-folder-and-pages"></a>Bir Checkout klasör ve sayfaları ekleme

Şimdi oluşturacak *Checkout* klasör ve sayfaları, Müşteri satın alma işlemi sırasında görürsünüz. Bu öğreticide daha sonra bu sayfaları güncelleştirir.

1. Proje adına sağ tıklayın (**Wingtip Toys**) içinde **Çözüm Gezgini** seçip **yeni bir klasör ekleyin**. 

    ![Kullanıma alma ve ödeme PayPal - yeni klasör](checkout-and-payment-with-paypal/_static/image1.png)
2. Yeni bir klasör adı *Checkout*.
3. Sağ *Checkout* klasörünü ve ardından **Ekle**-&gt;**yeni öğe**. 

    ![Kullanıma alma ve ödeme PayPal - yeni öğe ile](checkout-and-payment-with-paypal/_static/image2.png)
4. **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
5. Seçin **Visual C#**  - &gt; **Web** soldaki templates grubu. Orta bölmesinden seçip **ana sayfa ile Web Form**ve adlandırın *CheckoutStart.aspx*. 

    ![Kullanıma alma ve PayPal - ödemeyle ekleme yeni öğe iletişim kutusu](checkout-and-payment-with-paypal/_static/image3.png)
6. Önceki gibi seçin *Site.Master* ana sayfa olarak dosya.
7. Aşağıdaki ek sayfalara eklemek *Checkout* klasörü yukarıdaki aynı adımları kullanarak:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Bir Web.config dosyası ekler

Yeni bir ekleyerek *Web.config* dosya *Checkout* klasörünü görebilirsiniz klasördeki tüm sayfalar için erişimi kısıtlamak.

1. Sağ *Checkout* klasörü ve select **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki templates grubu. Orta bölmesinden seçip **Web yapılandırma dosyası**, varsayılan adı kabul *Web.config*ve ardından **Ekle**.
3. Varolan XML içeriği Değiştir *Web.config* aşağıdaki dosyasıyla:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Kaydet *Web.config* dosya.

*Web.config* dosyayı belirtir Web uygulamasının tüm bilinmeyen kullanıcılar içinde yer alan sayfaları için erişim reddedildi gerekir *Checkout* klasör. Ancak, kullanıcı hesabınız kayıtlı olan ve oturum açmış, bunlar bilinen bir kullanıcı ve sayfalarına erişebilir *Checkout* klasör.

ASP.NET yapılandırma bir hiyerarşi izlediğini dikkate almak önemlidir burada her *Web.config* dosya olarak klasöre ve alt dizinlerinde tüm yapılandırma ayarlarını uygular.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Proje için SSL etkinleştir

 Güvenli Yuva Katmanı (SSL), Web sunucuları ve Web istemcileri daha güvenli bir şekilde şifreleme kullanımı ile iletişim kurmasına izin vermek için tanımlanmış bir protokoldür. SSL kullanılmadığında istemci ve sunucu arasında gönderilen verilerin ağa fiziksel erişimi olan herkes tarafından algılaması paket açıktır. Ayrıca, birçok ortak kimlik doğrulama şemasını düz HTTP üzerinden güvenli değildir. Özellikle, temel kimlik doğrulaması ve forms kimlik doğrulaması şifrelenmemiş kimlik bilgilerini gönderir. Güvenli olması için bu kimlik doğrulama şemasını SSL kullanmanız gerekir. 

1. İçinde **Çözüm Gezgini**, tıklatın **WingtipToys** proje ve basın **F4** görüntülemek için **özellikleri** penceresi.
2. Değişiklik **SSL özellikli** için `true`.
3. Kopya **SSL URL** daha sonra kullanabilirsiniz.   
 SSL URL `https://localhost:44300/` SSL Web siteleri (aşağıda gösterildiği gibi) daha önceden oluşturduğunuz sürece.   
    ![Proje Özellikleri](checkout-and-payment-with-paypal/_static/image4.png)
4. İçinde **Çözüm Gezgini**, sağ tıklatın **WingtipToys** proje ve tıklatın **özellikleri**.
5. Sol sekmede tıklatın **Web**.
6. Değişiklik **proje URL'sini** kullanmak için **SSL URL** daha önce kaydettiğiniz.   
    ![Proje Web özellikleri](checkout-and-payment-with-paypal/_static/image5.png)
7. Sayfayı tuşlarına basarak kaydetmek **CTRL + S**.
8. Tuşuna **Ctrl + F5** uygulamayı çalıştırın. Visual Studio SSL uyarılarını olanak tanımak için bir seçenek görüntüler.
9. Tıklatın **Evet** IIS Express SSL sertifikasına güvenmesi ve devam edin.   
    ![IIS Express SSL sertifikası ayrıntıları](checkout-and-payment-with-paypal/_static/image6.png)  
 Bir güvenlik uyarısı görüntülenir.
10. Tıklatın **Evet** localhost olarak sertifika yüklemek için.   
    ![Güvenlik Uyarısı iletişim kutusu](checkout-and-payment-with-paypal/_static/image7.png)  
 Bir tarayıcı penceresi görüntülenir.

Şimdi kolayca Web uygulamanızı yerel olarak SSL kullanarak test edebilirsiniz.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Bir OAuth 2.0 Sağlayıcısı Ekle

ASP.NET Web Forms üyeliği ve kimlik doğrulama için Gelişmiş seçenekleri sağlar. Bu geliştirmeler, OAuth içerir. OAuth güvenli yetkilendirme, web, mobil ve Masaüstü uygulamalardan basit ve standart bir yöntem sağlar açık bir protokoldür. ASP.NET Web Forms şablon OAuth Facebook, Twitter, Google ve Microsoft kimlik doğrulama sağlayıcısı olarak kullanıma sunmak için kullanır. Bu öğretici yalnızca Google kimlik doğrulama sağlayıcısı olarak kullansa da, sağlayıcılardan herhangi birinin kullanmak için kodu kolayca değiştirebilirsiniz. Diğer sağlayıcıları uygulamaya yönelik adımlar, bu öğreticide görürsünüz adımları çok benzer.

Ek olarak, kimlik, öğreticiyi de rolleri yetkilendirme uygulamak için kullanır. Yalnızca eklediğiniz kullanıcılar `canEdit` rolü verileri değiştirmek mümkün olacaktır (oluşturmak, düzenlemek veya kişiler Sil).

> [!NOTE] 
> 
> Oturum açma bilgileri test etmek için bir yerel Web sitesi URL'si kullanamazlar Windows Live uygulamalar yalnızca bir çalışma Web sitesi için Canlı bir URL kabul edin.


Aşağıdaki adımlar, Google kimlik doğrulama sağlayıcısı eklemenize izin verir.

1. Açık *uygulama\_Start\Startup.Auth.cs* dosya.
2. Açıklama karakterlerinden kaldırmak `app.UseGoogleAuthentication()` yöntemi olarak yöntemi görünmesi izler: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Gidin [Google geliştiriciler konsol](https://console.developers.google.com/). Google developer e-posta hesabınızı (gmail.com) ile oturum açması gerekir. Bir Google hesabı yoksa seçin **bir hesap oluşturmanız** bağlantı.   
   Ardından, göreceğiniz **Google geliştiriciler konsol**.   
    ![Google geliştiriciler konsol](checkout-and-payment-with-paypal/_static/image8.png)
4. Tıklatın **proje oluştur** düğmesine tıklayın ve bir proje adı ve kimliği (varsayılan değerleri kullanabilirsiniz) girin. Ardından **sözleşmesi onay kutusunu** ve **oluşturma** düğmesi.  

    ![Google - yeni proje](checkout-and-payment-with-paypal/_static/image9.png)

   Yeni Proje birkaç saniye içinde oluşturulur ve yeni projeler sayfa, tarayıcınızın görüntülenir.
5. Sol sekmede tıklatın **API'leri &amp; auth**ve ardından **kimlik bilgileri**.
6. Tıklatın **yeni istemci kodu oluştur** altında **OAuth**.   
   **İstemci kimliği oluşturma** iletişim kutusu görüntülenir.   
    ![Google - istemci kodu oluştur](checkout-and-payment-with-paypal/_static/image10.png)
7. İçinde **istemci kimliği oluşturma** iletişim kutusunda, varsayılan tutmak **Web uygulaması** uygulama türü için.
8. Ayarlama **yetkili JavaScript çıkış** Bu öğreticide daha önce kullanılan SSL URL'ye (`https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).   
   Uygulamanız için kaynak URL'dir. Bu örnek için yalnızca localhost test URL'sini girer. Ancak, birden fazla hesap localhost ve üretim için URL'lerini girebilirsiniz.
9. Ayarlama **yeniden yönlendirme URI'si yetkili** şu: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Bu değer, ASP.NET OAuth URI'dir kullanıcılar google OAuth sunucusuyla iletişim kurar. Yukarıda kullanılan SSL URL unutmayın ( `https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).
10. Tıklatın **istemci kimliği oluşturma** düğmesi.
11. Google geliştiriciler konsolunun sol menüsünde tıklatın **izin ekran** menü öğesi, ardından e-posta adresi ve ürün adı ayarlayın. Formu doldurduğunuzda tıklatın **kaydetmek**.
12. Tıklatın **API'leri** menü öğesi, aşağı kaydırın ve tıklatın **kapalı** düğmesine **Google + API**.   
    Bu seçenek kabul Google + API olanak sağlar.
13. Aynı zamanda güncelleştirmelisiniz **Microsoft.Owin** 3.0.0 sürümüne NuGet paketi.   
    Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** ve ardından **çözüm için NuGet paketlerini Yönet**.  
    Gelen **NuGet paketlerini Yönet** penceresi, bulma ve güncelleştirme **Microsoft.Owin** 3.0.0 sürümüne paket.
14. Visual Studio'da güncelleştirme `UseGoogleAuthentication` yöntemi *Startup.Auth.cs* sayfa kopyalama ve yapıştırma tarafından **istemci kimliği** ve **gizli** yöntemiyle. **İstemci kimliği** ve **gizli** aşağıda gösterilen değerleri örnekleri ve çalışmaz. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Tuşuna **CTRL + F5** oluşturun ve uygulamayı çalıştırın. Tıklatın **oturum** bağlantı.
16. Altında **oturum açmak için başka bir hizmet kullanın**, tıklatın **Google**.  
    ![Oturum aç](checkout-and-payment-with-paypal/_static/image11.png)
17. Kimlik bilgilerinizi girmeniz gerekiyorsa, kimlik bilgilerinizi gireceğiniz google siteye yönlendirilir.  
    ![Google - oturum açma](checkout-and-payment-with-paypal/_static/image12.png)
18. Kimlik bilgilerinizi girdikten sonra yeni oluşturduğunuz web uygulaması izinleri vermeniz istenir.  
    ![Proje varsayılan hizmet hesabı](checkout-and-payment-with-paypal/_static/image13.png)
19. Tıklatın **kabul**. Artık yeniden yönlendirileceği konum **kaydetmek** sayfasında **WingtipToys** Burada, kaydolabilir Google hesabınız uygulama.  
    ![Google hesabınız ile kaydetme](checkout-and-payment-with-paypal/_static/image14.png)
20. Gmail hesabınız için kullanılan yerel e-posta kayıt adını değiştirme seçeneğiniz vardır, ancak genellikle varsayılan e-posta diğer adı (diğer bir deyişle, kimlik doğrulaması için kullanılan bir) tutmak istiyor. Tıklatın **oturum** yukarıda gösterildiği gibi.

### <a name="modifying-login-functionality"></a>Oturum açma işlevselliği değiştirme

Daha önce belirtildiği gibi Bu öğretici serisinde, kullanıcı kayıt işlevlerinin çoğunu ASP.NET Web Forms şablonunda varsayılan olarak dahil edilmiştir. Varsayılan değiştirecek artık *Login.aspx* ve *Register.aspx* çağırmak için sayfaları `MigrateCart` yöntemi. `MigrateCart` Yöntemi yeni oturum açmış kullanıcının anonim bir alışveriş sepeti ile ilişkilendirir. Kullanıcı ilişkilendirme ve alışveriş sepeti Wingtip Toys örnek uygulama ziyaretleri kullanıcının alışveriş sepeti korumak kuramaz.

1. İçinde **Çözüm Gezgini**, bulma ve açma *hesap* klasör.
2. Adlı arka plan kod sayfası değiştirmek *Login.aspx.cs* şu şekilde görünmesi sarı ile vurgulanmış kodu eklemek için:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Kaydet *Login.aspx.cs* dosya.

Şimdilik, tanım için bu uyarıyı yoksayabilirsiniz `MigrateCart` yöntemi. Bu biraz daha sonra Bu öğreticide eklemek olacaktır.

*Login.aspx.cs* arka plan kod dosyasına bir oturum açma yöntemini destekler. Login.aspx sayfasına inceleyerek bu sayfa "Oturum" düğmesi içerir görürsünüz olduğunda Tetikleyicileri tıklatın `LogIn` arka plan kodu üzerinde işleyici.

Zaman `Login` yöntemi *Login.aspx.cs* çağrılır, alışveriş sepeti adlı yeni bir örneğini `usersShoppingCart` oluşturulur. Alışveriş sepeti (GUID) Kimliğini alınır ve kümesine `cartId` değişkeni. Ardından, `MigrateCart` yöntemi çağrıldığında, her ikisi de geçirme `cartId` ve bu yöntem için oturum açma kullanıcı adı. Alışveriş sepeti geçirildiğinde anonim alışveriş sepeti tanımlamak için kullanılan GUID kullanıcı adı ile değiştirilir.

Değiştirme yanı sıra *Login.aspx.cs* alışveriş sepeti, kullanıcı oturumunu geçirmek için arka plan kodu dosyasını aynı zamanda değiştirmeniz gerekir *Register.aspx.cs arka plan kod dosyasına* alışveriş sepeti geçirmek için Kullanıcı yeni bir hesabı oluşturur ve oturum açtığında.

1. İçinde *hesap* arka plan kodu dosya adlı klasörü, açık *Register.aspx.cs*.
2. Böylece aşağıdaki gibi görünür sarı olarak kod dahil olmak üzere arka plan kodu dosyasını değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Kaydet *Register.aspx.cs* dosya. Bir kez daha, hakkında uyarı yoksayılabilir `MigrateCart` yöntemi.

Kod içinde kullandığınız fark `CreateUser_Click` olay işleyicisi içinde kullanılan kod çok benzer `LogIn` yöntemi. Kullanıcı kayıtları veya oturum açtığında siteye yapılan bir çağrı olduğunda `MigrateCart` yöntemi yapılmayacak.

## <a name="migrating-the-shopping-cart"></a>Alışveriş sepeti geçirme

Güncelleştirilmiş oturum açma ve kayıt işlemine sahip olduğunuza göre alışveriş sepeti kullanarak geçirmek için kod ekleyebilirsiniz `MigrateCart` yöntemi.

1. İçinde **Çözüm Gezgini**, bulma *mantığı* klasörü ve açık *ShoppingCartActions.cs* sınıf dosyası.
2. Var olan kodda sarıya vurgulanmış kodu ekleyin *ShoppingCartActions.cs* dosya, böylece kodu *ShoppingCartActions.cs* dosya şu şekilde görünür:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart` Yöntemi, kullanıcının alışveriş sepeti bulmak için varolan cartId kullanır. Ardından, kod tüm alışveriş sepeti öğeler arasında döngü ve değiştirir `CartId` özelliği (belirtildiği gibi `CartItem` şema) ile oturum açma kullanıcı adı.

### <a name="updating-the-database-connection"></a>Veritabanı bağlantısı güncelleştiriliyor

Bu öğretici kullanarak izliyorsanız **önceden oluşturulmuş** Wingtip Toys örnek uygulama, varsayılan üyelik veritabanının yeniden oluşturmanız gerekir. Varsayılan bağlantı dizesini değiştirerek, üyelik veritabanının uygulama çalıştığında oluşturulur.

1. Açık *Web.config* proje kökündeki dosya.
2. Varsayılan bağlantı dizesini gibi göründüğü şekilde güncelleştirin:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>PayPal tümleştirme

PayPal ödemelerini çevrimiçi satıcı tarafından kabul eden bir web tabanlı fatura platformudur. Bu öğreticinin sonraki PayPal'ın Express Checkout işlevselliği uygulamanıza tümleştirmek açıklanmaktadır. Express Checkout müşterilerinizin PayPal kendi alışveriş sepetine eklemiş olduğunuz öğeleri için ödeme kullanmasını sağlar.

### <a name="create-paylpal-test-accounts"></a>PaylPal Test hesapları oluşturma

Sınama ortamı PayPal kullanmak için oluşturma ve bir geliştirici test hesabı doğrulamanız gerekir. Bir alıcı test hesabınız ve satıcı test hesabınız oluşturmak için geliştirici test hesabını kullanır. Geliştirici testi hesap kimlik bilgilerini de PayPal test ortamını erişmek Wingtip Toys örnek uygulama izin verir.

1. Bir tarayıcıda, site test PayPal Geliştirici gidin:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. PayPal Geliştirici hesabınız yoksa, yeni bir hesap oluşturun **kaydolun**kayıt adımları izleyerek. PayPal Geliştirici hesabınız varsa, tıklayarak oturum **oturum**. PayPal Geliştirici hesabınızda daha sonra Bu öğreticide Wingtip Toys örnek uygulamanızı test etmeniz gerekir.
3. Yalnızca PayPal Geliştirici hesabınız için kaydolduysanız, PayPal, PayPal Geliştirici hesabınızla doğrulamak gerekebilir. E-posta hesabınızı PayPal gönderilen adımları izleyerek hesabınızı doğrulayabilirsiniz. PayPal Geliştirici hesabınızda doğruladıktan sonra site test geri PayPal Geliştirici oturum açın.
4. PayPal Geliştirici siteye zaten yapmazsanız PayPal alıcı sınama hesabı oluşturmanız gerekir, PayPal Geliştirici hesabınızla oturum sonra vardır. Bir alıcı test hesabınız, PayPal site tıklatıldığında oluşturmak için **uygulamaları** sekmesini ve sonra **korumalı alan hesapları**.   
 **Korumalı alan test hesapları** sayfası gösterilir.   

    > [!NOTE] 
    > 
    > PayPal Geliştirici sitesi zaten bir ticari test hesabı sağlar.

    ![Kullanıma alma ve ödeme PayPal - korumalı alan test hesapları ile](checkout-and-payment-with-paypal/_static/image15.png)
5. Korumalı alan test hesapları sayfasında, tıklatın **hesap oluştur**.
6. Üzerinde **test hesap oluştur** sayfa test hesabı e-posta ve parola tercih ettiğiniz bir alıcı seçin.   

    > [!NOTE] 
    > 
    > Alıcı e-posta adreslerini ve bu öğreticinin sonunda Wingtip Toys örnek uygulamayı test etmek için parola gerekir.

    ![Kullanıma alma ve ödeme PayPal - korumalı alan test hesapları ile](checkout-and-payment-with-paypal/_static/image16.png)
7. Tıklayarak alıcı test hesabı oluşturma **hesap oluştur** düğmesi.  
 **Korumalı alan Test hesapları** sayfası görüntülenir. 

    ![Kullanıma alma ve ödeme PayPal - PaylPal hesapları ile](checkout-and-payment-with-paypal/_static/image17.png)
8. Üzerinde **korumalı alan test hesapları** sayfasında, **yönetici** e-posta hesabı.  
    **Profil** ve **bildirim** seçenekler görünür.
9. Seçin **profil** seçeneğini ve ardından **API kimlik** Tüccar sınama hesabı için API kimlik bilgilerinizi görüntülemek için.
10. TEST API'si kimlik bilgilerini Not Defteri'ne kopyalayın.

Sınama ortamı PayPal görüntülenen Klasik TEST API kimlik bilgilerinizi (kullanıcı adı, parola ve imza) API çağrıları Wingtip Toys örnek uygulamadan yapmanız gerekir. Kimlik bilgileri sonraki adımda ekleyeceksiniz.

### <a name="add-paypal-class-and-api-credentials"></a>PayPal sınıfı ve API kimlik bilgileri ekleme

PayPal kod çoğunluğu tek bir sınıf içine yerleştirir. Bu sınıf, PayPal ile iletişim kurmak için kullanılan yöntemler içerir. Ayrıca, bu sınıfa PayPal kimlik bilgilerinizi ekleyeceksiniz.

1. Visual Studio içinde Wingtip Toys örnek uygulamayı sağ **mantığı** klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Altında **Visual C#** gelen **yüklü** seçin sol bölmede **kod**.
3. Orta bölmesinden seçin **sınıfı**. Bu yeni sınıf adını **PayPalFunctions.cs**.
4. **Ekle**'yi tıklatın.  
   Yeni sınıf dosyası Düzenleyicisi'nde görüntülenir.
5. Varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Bu öğreticide daha önce görüntülenen ve böylece PayPal test ortamını işlev çağrılarını yapabileceğiniz satıcı API kimlik bilgilerini (kullanıcı adı, parola ve imza) ekleyin.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Bu örnek uygulamasında bir C# dosyasına (.cs) kimlik bilgilerini yalnızca ekliyorsunuz. Ancak, uygulanan bir çözümde bir yapılandırma dosyası kimlik bilgilerinizi şifrelemek düşünmelisiniz.


NVPAPICaller sınıfı PayPal işlevselliğin çoğu içerir. Sınıfı kodda PayPal sınama ortamından satın test yapmak için gereken yöntemleri sağlar. Aşağıdaki üç PayPal işlevleri, satın alma işlemleri yapmak için kullanılır:

- `SetExpressCheckout` İşlevi
- `GetExpressCheckoutDetails` İşlevi
- `DoExpressCheckoutPayment` İşlevi

`ShortcutExpressCheckout` Yöntemi toplar test satın alma bilgileri ve ürün ayrıntıları alışveriş sepeti ve çağrıları `SetExpressCheckout` PayPal işlevi. `GetCheckoutDetails` Yöntemini onaylar satın alma ayrıntıları ve çağrıları `GetExpressCheckoutDetails` test satın alma yapmadan önce PayPal işlevi. `DoCheckoutPayment` Yöntemi çağrılarak sınama ortamından test satınalma tamamlayan `DoExpressCheckoutPayment` PayPal işlevi. Kalan kod PayPal yöntemleri ve işlemin dizeleri kodlama, dizeleri kod çözme, dizileri işleme ve kimlik bilgilerini belirleme gibi destekler.

> [!NOTE] 
> 
> PayPal göre isteğe bağlı satın alma ayrıntıları dahil etmenize olanak verir [PayPal'ın API belirtimine](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Wingtip Toys örnek uygulama kodunda genişleterek, yerelleştirme ayrıntıları, ürün tanımları, vergi, bir müşteri hizmeti numarası yanı sıra diğer birçok isteğe bağlı alanları içerebilir.


Dikkat belirtilen dönüş ve iptal URL'leri **ShortcutExpressCheckout** yöntemi bir bağlantı noktası numarasını kullanın.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Yaygın olarak Visual Web Developer SSL kullanarak bir web projesi çalıştığında, bağlantı noktası 44300 web sunucusu için kullanılır. Yukarıda gösterildiği gibi bağlantı noktası numarasını 44300 olur. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görebilir. Bağlantı noktası numarası gereksinimlerinizi yapabilmeniz doğru kodda başarılı ayarlanması, bu öğreticinin sonunda Wingtip Toys örnek uygulamayı çalıştırın. Bu öğreticinin sonraki bölümde, yerel ana bilgisayar bağlantı noktası numarasını almak ve PayPal sınıfını güncelleştirme açıklanmaktadır.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Güncelleştirme PayPal sınıfındaki LocalHost bağlantı noktası numarası

Wingtip Toys örnek uygulama, PayPal test siteye gittikten Wingtip Toys örnek uygulamayı yerel örneğine döndürme ürünleri satın alır. URL'nin doğru dönüş PayPal olması için yerel olarak çalışan bağlantı noktası numarasını belirtmeniz gerekir örnek uygulama yukarıda belirtilen PayPal kod.

1. Proje adına sağ tıklayın (**WingtipToys**) içinde **Çözüm Gezgini** seçip **özellikleri**.
2. Sol sütunda seçin **Web** sekmesi.
3. Bağlantı noktası numarasından almak **proje URL'sini** kutusu.
4. Gerekirse, güncelleştirme `returnURL` ve `cancelURL` PayPal sınıfında (`NVPAPICaller`) içinde *PayPalFunctions.cs* web uygulamanızı bağlantı noktası numarasını kullanmak üzere bir dosya:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Şimdi eklediğiniz kodu yerel Web uygulamanız için beklenen bağlantı noktası ile eşleşir. PayPal URL'nin doğru yerel makinenizde iade etme olanağınız olur.

### <a name="add-the-paypal-checkout-button"></a>PayPal ödeme düğme ekleme

Birincil PayPal işlevleri örnek uygulama için eklenene, biçimlendirme ve bu işlevleri çağırmak için gereken kod eklemeye başlayabilirsiniz. İlk olarak, kullanıcı alışveriş sepeti sayfasında göreceğiniz çıkış düğmesi eklemeniz gerekir.

1. Açık *ShoppingCart.aspx* dosya.
2. Dosyanın sonuna kaydırın ve Bul `<!--Checkout Placeholder -->` açıklama.
3. İle açıklamayı değiştirin bir `ImageButton` işaretleme şu şekilde değiştirilir böylece denetleme:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. İçinde *ShoppingCart.aspx.cs* sonra dosya `UpdateBtn_Click` dosyasının sonuna yakın olay işleyicisi ekleme `CheckOutBtn_Click` olay işleyicisi:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Ayrıca, *ShoppingCart.aspx.cs* dosya, bir başvuru ekleyin `CheckoutBtn`, böylece yeni görüntü düğmesi gibi başvuruluyor:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Her ikisi de yaptığınız değişiklikleri kaydetmek *ShoppingCart.aspx* dosya ve *ShoppingCart.aspx.cs* dosya.
7. Menüsünden seçin **hata ayıklama**-&gt;**yapı WingtipToys**.  
   Projeyi yeniden yeni eklenen ile **ImageButton** denetim.

### <a name="send-purchase-details-to-paypal"></a>Satın alma ayrıntıları PayPal için Gönder

Kullanıcı tıkladığında **Checkout** alışveriş sepeti sayfasında düğmesini (*ShoppingCart.aspx*), satın alma işlemini başlarsınız. Aşağıdaki kod ürünleri satın almak için gereken ilk PayPal işlevi çağırır.

1. Gelen *Checkout* arka plan kodu dosya adlı klasörü, açık *CheckoutStart.aspx.cs*.   
   Arka plan kodu dosyayı açmaya emin olun.
2. Var olan kodu aşağıdakilerle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Uygulamanın kullanıcı tıkladığında **Checkout** alışveriş sepeti sayfasında, tarayıcı düğmesi gidin *CheckoutStart.aspx* sayfası. Zaman *CheckoutStart.aspx* sayfa yüklendiğinde `ShortcutExpressCheckout` yöntemi çağrılır. Bu noktada, kullanıcı PayPal test web sitesine aktarılır. PayPal sitesinde kullanıcı PayPal kimlik bilgilerini girer, satın alma ayrıntıları gözden geçirir, PayPal sözleşmesini kabul eder ve Wingtip Toys örnek uygulamaya döndürür nerede `ShortcutExpressCheckout` yöntemi tamamlar. Zaman `ShortcutExpressCheckout` yöntemi tamamlandığında, kullanıcıya yönlendirir *CheckoutReview.aspx* belirtilen sayfa `ShortcutExpressCheckout` yöntemi. Bu kullanıcının Wingtip Toys örnek uygulamasında sipariş ayrıntıları gözden olanak sağlar.

### <a name="review-order-details"></a>Sipariş ayrıntılarını gözden geçir

PayPal döndürmeden sonra *CheckoutReview.aspx* Wingtip Toys örnek uygulaması sayfasında sipariş ayrıntılarını görüntüler. Bu sayfa ürünleri satın almadan önce sipariş ayrıntılarını gözden geçirmek kullanıcı sağlar. *CheckoutReview.aspx* sayfa gibi oluşturulmalıdır:

1. İçinde *Checkout* sayfa adlı klasörü, açık *CheckoutReview.aspx*.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Adlı arka plan kod sayfası açmak *CheckoutReview.aspx.cs* ve var olan kodu aşağıdakilerle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView** denetimi PayPal döndürülen sipariş ayrıntılarını görüntülemek için kullanılır. Ayrıca, yukarıdaki kod Wingtip Toys veritabanına sipariş ayrıntılarını kaydeder bir `OrderDetail` nesnesi. Kullanıcı tıkladığında üzerinde **tam sipariş** düğmesi, bunlar yönlendirilir için *CheckoutComplete.aspx* sayfası.

> [!NOTE] 
> 
> **İpucu**
> 
> Biçimlendirmede *CheckoutReview.aspx* sayfasında, dikkat `<ItemStyle>` etiketi içinde öğelerin stilini değiştirmek için kullanılan **DetailsView** denetim sayfanın yakın. Sayfanın görüntüleyerek **Tasarım görünümünde** (seçerek **tasarım** Visual Studio'nun sol alt köşesindeki), ardından seçerek **DetailsView** denetlemek ve seçme **Akıllı etiket** (üstündeki ok simgesine sağ denetiminin), görmeye olacaktır **DetailsView görevleri**.
> 
> ![Kullanıma alma ve PayPal - ödemeyle alanları Düzenle](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Seçerek **alanları Düzenle**, **alanları** iletişim kutusu görüntülenir. Bu iletişim kutusunda, kolayca görsel özelliklerini gibi denetleyebilirsiniz **ItemStyle**, **DetailsView** denetim.
> 
> ![Kullanıma alma ve PayPal - alanları iletişim ödemeyle](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Satın almanızı tamamlayın

*CheckoutComplete.aspx* sayfası, PayPal satın alma yapar. Yukarıda belirtildiği gibi kullanıcı üzerinde tıklatmalısınız **tam sipariş** uygulama gideceği önce düğmesini *CheckoutComplete.aspx* sayfası.

1. İçinde *Checkout* sayfa adlı klasörü, açık *CheckoutComplete.aspx*.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Adlı arka plan kod sayfası açmak *CheckoutComplete.aspx.cs* ve var olan kodu aşağıdakilerle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Zaman *CheckoutComplete.aspx* sayfa yüklenir, `DoCheckoutPayment` yöntemi çağrılır. Daha önce belirtildiği gibi `DoCheckoutPayment` yöntemi tamamlayan PayPal sınama ortamı'ndan satın alma. PayPal satın alma siparişi tamamlandıktan sonra *CheckoutComplete.aspx* sayfasını görüntüler ödeme işlem `ID` satın alan için.

### <a name="handle-cancel-purchase"></a>Tanıtıcı iptal satın alma

Satın alma iptal etmek kullanıcı karar verirse, bunlar yönlendirilirsiniz *CheckoutCancel.aspx* sayfa burada bunlar görürsünüz: sıralarına iptal edildi.

1. Adlı sayfasını açın *CheckoutCancel.aspx* içinde *Checkout* klasör.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Satın alma hataları işleme

Satın alma işlemi sırasında hatalar tarafından işleneceğini *CheckoutError.aspx* sayfası. Arka plan kod, *CheckoutStart.aspx* sayfasında, *CheckoutReview.aspx* sayfasında ve *CheckoutComplete.aspx* sayfa her yeniden yönlendirmek için  *CheckoutError.aspx* bir hata oluşursa sayfa.

1. Adlı sayfasını açın *CheckoutError.aspx* içinde *Checkout* klasör.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx* kullanıma alma işlemi sırasında bir hata oluştuğunda hata ayrıntılarla sayfası görüntülenir.

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Ürün satın nasıl görmek için uygulamayı çalıştırın. PayPal sınama ortamında çalıştırırsınız olduğunu unutmayın. Hiçbir gerçek para alınıp verilir.

1. Tüm dosyalarınızı Visual Studio'da kaydedilmiş olduğundan emin olun.
2. Bir Web tarayıcısı açın ve gidin [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Bu öğreticide daha önce oluşturduğunuz PayPal Geliştirici hesabınız ile oturum açın.  
   PayPal'ın Geliştirici korumalı alan için oturum açmanız gerekir. [ https://developer.paypal.com ](https://developer.paypal.com/) express checkout test etmek için. Bu yalnızca sınama, PayPal'ın dinamik ortamına değil PayPal'ın Korumalı alan için geçerlidir.
4. Visual Studio'da basın **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
   Veritabanını yeniden oluşturur sonra tarayıcıyı açın Göster ve *Default.aspx* sayfası.
5. "Araba" gibi ürün kategorisi seçerek ve ardından üç farklı ürünler alışveriş sepetine eklemek **Sepete Ekle** her ürünün yanındaki.  
   Alışveriş sepeti seçmiş olduğunuz ürün görüntüler.
6. Tıklatın **PayPal** checkout düğme. 

    ![Kullanıma alma ve PayPal - Sepeti ödemeyle](checkout-and-payment-with-paypal/_static/image20.png)

   Kullanıma Wingtip Toys örnek uygulama için bir kullanıcı hesabına sahip olması gerekir.
7. Tıklatın **Google** sağdaki varolan bir gmail.com e-posta hesabı ile oturum açmanız sayfasının bağlantısı.  
   Bir gmail.com hesabı yoksa, test etmek için bir tane oluşturabilirsiniz [www.gmail.com](https://www.gmail.com/). Standart bir yerel hesap "Register"'i tıklatarak de kullanabilirsiniz. 

    ![Kullanıma alma ve ödeme PayPal ile-oturum açın](checkout-and-payment-with-paypal/_static/image21.png)
8. Gmail hesap ve parola ile oturum açın. 

    ![Kullanıma alma ve ödeme PayPal - Gmail oturum açma ile](checkout-and-payment-with-paypal/_static/image22.png)
9. Tıklatın **oturum** gmail hesabınıza Wingtip Toys örnek uygulama kullanıcı adınız ile kaydetmek için düğmesi. 

    ![Kullanıma alma ve ödeme PayPal - kayıt hesabı ile](checkout-and-payment-with-paypal/_static/image23.png)
10. PayPal sınama sitesinde eklemek, **alıcı** e-posta adresi ve Bu öğreticide daha önce oluşturduğunuz parola ve ardından **oturum** düğmesi. 

    ![Kullanıma alma ve ödeme PayPal - PayPal oturum açma ile](checkout-and-payment-with-paypal/_static/image24.png)
11. Kabul PayPal ilkeyi ve tıklatın **kabul et ve devam et** düğmesi.  
    Bu sayfa yalnızca olduğuna dikkat edin, bu PayPal hesabı kullanmak ilk kez görüntülenir. Bu bir test hesabıdır, gerçek bir para alınıp yeniden unutmayın. 

    ![Kullanıma alma ve ödeme PayPal - PayPal İlkesi ile](checkout-and-payment-with-paypal/_static/image25.png)
12. Ortam gözden geçirme sayfasını ve tıklatın sınama PayPal siparişi bilgileri gözden **devam**. 

    ![Kullanıma alma ve ödeme PayPal - bilgileri gözden geçir](checkout-and-payment-with-paypal/_static/image26.png)
13. Üzerinde *CheckoutReview.aspx* sayfasında sipariş miktarını doğrulayın ve oluşturulan teslimat adresi görüntüleyin. Ardından **tam sipariş** düğmesi. 

    ![Kullanıma alma ve ödeme PayPal - sipariş gözden geçirme](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx** sayfası, bir ödeme işlem kimliğiyle görüntülenir 

    ![Kullanıma alma ve PayPal - Checkout tam ödemeyle](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Veritabanı gözden geçirme

Uygulamayı çalıştırdıktan sonra güncelleştirilmiş verileri Wingtip Toys örnek uygulama veritabanındaki gözden geçirerek, uygulamanın başarıyla ürünleri satın kaydedilen görebilirsiniz.

Bulunan verileri inceleyebilirsiniz *Wingtiptoys.mdf* kullanarak veritabanı dosyası **Database Explorer** penceresi (**Sunucu Gezgini** Visual Studio'daki) yaptığınız gibi daha önce Bu öğretici serisinde.

1. Hala açıksa tarayıcı penceresini kapatın.
2. Visual Studio'da seçin **tüm dosyaları göster** en üstündeki simgesi **Çözüm Gezgini** genişletmenize izin vermek için **uygulama\_veri** klasör.
3. Genişletme **uygulama\_veri** klasör.  
 Seçmek için gerek duyabileceğiniz **tüm dosyaları göster** klasörü simgesi.
4. Sağ *Wingtiptoys.mdf* veritabanı dosyası ve select **açık**.  
    **Sunucu Gezgini** görüntülenir.
5. Genişletme **tabloları** klasör.
6. Sağ **siparişleri**tablo ve seçin **Show Table Data**.  
 **Siparişleri** tablo görüntülenir.
7. Gözden geçirme **PaymentTransactionID** başarılı işlemlerin onaylamak için sütun. 

    ![Kullanıma alma ve ödeme PayPal - gözden geçirme veritabanı ile](checkout-and-payment-with-paypal/_static/image29.png)
8. Kapat **siparişleri** Tablo penceresi.
9. Sunucu Gezgini'nde sağ **sipariş ayrıntıları** tablo ve seçin **Show Table Data**.
10. Gözden geçirme `OrderId` ve `Username` değerler **sipariş ayrıntıları** tablo. Bu değerlerin eşleşmesi Not `OrderId` ve `Username` dahil değerleri **siparişleri** tablo.
11. Kapat **sipariş ayrıntıları** Tablo penceresi.
12. Wingtip Toys veritabanı dosyasına sağ tıklayın (*Wingtiptoys.mdf*) seçip **yakın bağlantı**.
13. Görmüyorsanız, **Çözüm Gezgini** penceresinde tıklatın **Çözüm Gezgini** alt kısmındaki **Sunucu Gezgini** göstermek için pencere **Çözüm Gezgini**  yeniden.

## <a name="summary"></a>Özet

Bu öğreticide sırası ve siparişi ayrıntı şemalarını ürünleri satın izlemek için eklendi. Wingtip Toys örnek uygulamaya PayPal işlevselliği de tümleşik.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET yapılandırmasına genel bakış](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Güvenli ASP.NET Web Forms uygulama üyeliği, OAuth ve SQL veritabanı ile Azure App Service'e dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Sorumluluk reddi

Bu öğreticinin örnek kodunu içerir. Bu örnek kod, hiçbir garanti "olduğu gibi" sağlanmıştır. Buna göre Microsoft doğruluğu, bütünlük ve örnek kod kalitesini garanti etmez. Örnek kod, kendi risk kullanmayı kabul etmiş olursunuz. Hiçbir koşulda Microsoft herhangi bir şekilde size içerik dahil ancak bunlarla sınırlı olmamak üzere, herhangi bir hata veya eksikliklerden herhangi bir örnek kod, içerik veya herhangi kayıp veya hasar herhangi bir örnek kod kullanımı sonucunda sonucunda oluşan herhangi bir türde tüm örnek kod için sorumlu değildir. Burada REDDETMEKTEDİR bildirilir ve burada REDDETMEKTEDİR tazmin, kabul kaydetmek ve Microsoft gelen ve tüm kaybı, talep kaybı, yaralanma veya hasar tür, bunlarla sınırlı olmamak kaydıyla tarafından occasioned dahil olmak üzere veya deftere malzemesini doğan karşı zararsız tutun iletme, kullanın veya, ancak bunlarla sınırlı olmamak, okuduğunuzu ifade görünümleri dahil olmak üzere üzerinde kullanır.

> [!div class="step-by-step"]
> [Önceki](shopping-cart.md)
> [sonraki](membership-and-administration.md)
