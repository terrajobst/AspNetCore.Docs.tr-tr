---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Kullanıma alma ve ödeme içeren PayPal | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 3299da33a68f02ac1b3ffe7c037d06d8ece9455e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835637"
---
<a name="checkout-and-payment-with-paypal"></a>Kullanıma alma ve ödeme PayPal ile kasa işlemleri
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide, Wingtip Toys örnek uygulama, kullanıcı kimlik doğrulaması, kayıt ve PayPal kullanarak ödeme içerecek şekilde değiştirmek açıklar. Yalnızca oturum açmış olan kullanıcılar, ürünleri satın almak için yetkilendirme sahip olur. ASP.NET 4.5 Web formları projesi şablonun yerleşik kullanıcı kayıt işlevi zaten gerekenler çoğunu içerir. PayPal Express bırakıyorsa ekleyeceksiniz. Bu öğreticide, hiçbir gerçek fon aktarılır bu nedenle, ortam, test PayPal Geliştirici kullanıyor. Öğreticinin sonunda, alışveriş sepeti ödeme düğmesini tıklatarak ve veri PayPal test web sitesine aktarma eklemek için ürün seçerek uygulamayı test eder. PayPal test web sitesinde, nakliye ve Ödeme bilgilerinizi doğrulayın ve ardından onaylamak ve satın alma işlemini tamamlamak için yerel Wingtip Toys örnek uygulamaya geri dönmek.

Bu adres ölçeklenebilirlik ve güvenlik çevrimiçi alışveriş uzmanlaşmış birkaç deneyimli üçüncü taraf ödeme işlemci vardır. ASP.NET geliştiricilerine alışveriş uygulama ve çözüm satın önce bir üçüncü taraf ödeme çözümü kullanma avantajları göz önünde bulundurmanız gerekir.

> [!NOTE] 
> 
> Wingtip Toys örnek uygulama, ASP.NET ile ilgili belirli kavramları ve özellikler, ASP.NET web geliştiricileri için gösterilen şekilde tasarlanmıştır. Bu örnek uygulama ölçeklenebilirlik ve güvenlik in regard to tüm olası durumlarda iyileştirildiğinde değil.


## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Bir klasörde belirli sayfalara erişimi kısıtlamak nasıl.
- Anonim bir alışveriş sepeti bilinen bir alışveriş sepeti oluşturmak nasıl.
- Proje için SSL'yi etkinleştirecek şekilde nasıl.
- OAuth sağlayıcısı projeye ekleme.
- PayPal, PayPal test ortamını kullanarak ürünleri satın almak için kullanma
- PayPal ayrıntılarını görüntülemek nasıl bir **DetailsView** denetimi.
- PayPal alınan ayrıntılarla Wingtip Toys uygulama veritabanını güncelleştirme yapma.

## <a name="adding-order-tracking"></a>Sipariş İzleme ekleme

Bu öğreticide, bir kullanıcının oluşturduğu siparişin verileri izlemek için iki yeni sınıflar oluşturacaksınız. Sınıfları, gönderim bilgileri, toplam satın alma ve ödeme onayını ilgili verileri izler.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Sırasını ve OrderDetail Model sınıfları ekleme

Daha önce Bu öğretici serisinde, tanımladığınız kategorilerini, ürünleri için şema ve alışveriş sepetine öğe oluşturarak `Category`, `Product`, ve `CartItem` sınıfları *modelleri* klasör. Artık ürün siparişi ve ayrıntılarını sipariş için şema tanımlamak için iki yeni sınıf ekleyeceksiniz.

1. İçinde **modelleri** klasör adında yeni bir sınıf ekleyin *Order.cs*.   
   Yeni bir sınıf dosyası Düzenleyicisi'nde görüntülenir.
2. Varsayılan kodu aşağıdakiyle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Ekleme bir *OrderDetail.cs* sınıfının *modelleri* klasör.
4. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order` Ve `OrderDetail` satın alma ve aktarma için kullanılan sipariş bilgilerini tanımlamak için şema sınıflar bulunur.

Ayrıca, varlık sınıflarının yönetir ve veritabanı veri erişim sağlayan veritabanı bağlamı sınıfının güncelleştirmeniz gerekecektir. Bunu yapmak için yeni oluşturulan sipariş ekleyeceksiniz ve `OrderDetail` model sınıflarına `ProductContext` sınıfı.

1. İçinde **Çözüm Gezgini**, bulma ve açma *ProductContext.cs* dosya.
2. Vurgulanmış kodu ekleyin *ProductContext.cs* aşağıda gösterildiği gibi dosya:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Bu öğretici serisindeki kodda daha önce belirtildiği gibi *ProductContext.cs* dosyası ekler `System.Data.Entity` ad alanı Entity Framework'ün tüm çekirdek işlevlerin erişiminiz olduğunu. Bu işlev, sorgulama, ekleme, güncelleştirme ve kesin olarak belirlenmiş nesneler ile çalışma ile verileri silme özelliği içerir. Yukarıdaki kodda `ProductContext` sınıfı Entity Framework erişim için yeni eklenen ekler `Order` ve `OrderDetail` sınıfları.

## <a name="adding-checkout-access"></a>Kullanıma alma erişimi ekleniyor

Wingtip Toys örnek uygulamayı gözden geçirin ve ürünleri bir alışveriş sepetine eklemek anonim kullanıcıların sağlar. Anonim kullanıcılar, alışveriş sepetine eklendi ürün satın seçtiğinizde, ancak bunlar siteye oturum açmanız gerekir. Bunlar açtıktan sonra kullanıma alma işleyen ve satın alma işlemi kısıtlı sayfaları Web uygulamasının erişebilirsiniz. Bu kısıtlı sayfalar içerdiği *kullanıma alma* uygulamanın klasör.

### <a name="add-a-checkout-folder-and-pages"></a>Kullanıma alma klasörü ve sayfalar ekleme

Şimdi oluşturacaksınız *kullanıma alma* klasörü ve sayfaları, kullanıma alma işlemi sırasında müşterinin görürsünüz. Bu sayfalar, bu öğreticinin ilerleyen bölümlerinde güncelleştirir.

1. Proje adına sağ tıklayın (**Wingtip Toys**) içinde **Çözüm Gezgini** seçip **yeni klasör eklemek**. 

    ![Kullanıma alma ve ödeme içeren PayPal - yeni klasör](checkout-and-payment-with-paypal/_static/image1.png)
2. Yeni klasör adı *kullanıma alma*.
3. Sağ *kullanıma alma* klasörünü ve ardından **Ekle**-&gt;**yeni öğe**. 

    ![Kullanıma alma ve ödeme içeren PayPal - yeni öğe](checkout-and-payment-with-paypal/_static/image2.png)
4. **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
5. Seçin **Visual C#**  - &gt; **Web** soldaki şablonları grubu. Ardından, Ortadaki bölmeden **ana sayfa ile Web formu**ve adlandırın *CheckoutStart.aspx*. 

    ![Kullanıma alma ve ödeme içeren PayPal - yeni öğe iletişim kutusu Ekle](checkout-and-payment-with-paypal/_static/image3.png)
6. Önceki örneklerde olduğu gibi seçin *Site.Master* dosyası ana sayfa olarak.
7. Aşağıdaki ek sayfalara ekleme *kullanıma alma* yukarıdaki adımları kullanarak klasör:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Bir Web.config dosyasına ekleyin

Yeni bir ekleyerek *Web.config* dosyasını *kullanıma alma* klasöründe oluşturabileceksiniz klasördeki tüm sayfalar için erişimi kısıtlamak.

1. Sağ *kullanıma alma* klasörü ve select **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki şablonları grubu. Ardından, Ortadaki bölmeden **Web yapılandırma dosyası**, varsayılan adını kabul *Web.config*ve ardından **Ekle**.
3. Var olan XML içeriği değiştirin *Web.config* aşağıdaki dosya:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Kaydet *Web.config* dosya.

*Web.config* dosyasını belirtir tüm bilinmeyen kullanıcılar Web uygulamasının içerdiği sayfalarına erişimi reddedildi gerekir *kullanıma alma* klasör. Ancak, kullanıcı, bir hesap kaydetmiş olduğunu ve oturum açmış bunlar bilinen bir kullanıcı ve sayfalarına erişimi *kullanıma alma* klasör.

ASP.NET yapılandırması bir hiyerarşi izlediğini unutmayın. burada her *Web.config* dosya olarak klasöre ve alt dizinlerinde tüm yapılandırma ayarlarını uygular.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Proje için SSL etkinleştir

 Güvenli Yuva Katmanı (SSL), Web sunucuları ve şifreleme kullanarak daha güvenli bir şekilde iletişim kurmak için Web istemcileri izin verecek şekilde tanımlı bir protokoldür. SSL kullanılmadığında istemci ve sunucu arasında gönderilen verilerin paket ağa fiziksel erişimi olan herkes tarafından algılaması için açıktır. Ayrıca, birçok yaygın kimlik doğrulama düzenleri düz HTTP üzerinden güvenli değildir. Özellikle temel kimlik doğrulaması ve form kimlik doğrulaması şifrelenmemiş kimlik bilgileri gönderin. Güvenli olması için bu kimlik doğrulama düzenleri SSL kullanmanız gerekir. 

1. İçinde **Çözüm Gezgini**, tıklayın **WingtipToys** proje, ardından basın **F4** görüntülenecek **özellikleri** penceresi.
2. Değişiklik **SSL etkin** için `true`.
3. Kopyalama **SSL URL** daha sonra kullanabilirsiniz.   
 SSL URL'si olacaktır `https://localhost:44300/` sürece (aşağıda gösterildiği gibi) daha önce SSL Web sitesi oluşturdunuz.   
    ![Proje Özellikleri](checkout-and-payment-with-paypal/_static/image4.png)
4. İçinde **Çözüm Gezgini**, sağ tıklayın **WingtipToys** projesine **özellikleri**.
5. Sol sekmede tıklayın **Web**.
6. Değişiklik **proje URL'si** kullanılacak **SSL URL** daha önce kaydettiğiniz.   
    ![Proje Web özellikleri](checkout-and-payment-with-paypal/_static/image5.png)
7. Sayfayı tuşlarına basarak kaydedin **CTRL + S**.
8. Tuşuna **Ctrl + F5** uygulamayı çalıştırın. Visual Studio, SSL uyarılarından kaçınmak izin verme seçeneği görüntülenir.
9. Tıklayın **Evet** IIS Express SSL sertifikasına güvenmek ve devam edin.   
    ![IIS Express SSL sertifika ayrıntıları](checkout-and-payment-with-paypal/_static/image6.png)  
 Bir güvenlik uyarısı görüntülenir.
10. Tıklayın **Evet** için localhost sertifikası yüklemek için.   
    ![Güvenlik Uyarısı iletişim kutusu](checkout-and-payment-with-paypal/_static/image7.png)  
 Tarayıcı penceresinde görüntülenir.

Artık yerel olarak SSL kullanarak Web uygulamanızı kolayca sınayabilirsiniz.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>OAuth 2.0 Sağlayıcı Ekle

ASP.NET Web Forms, üyelik ve kimlik doğrulama için Gelişmiş seçenekleri sağlar. Bu geliştirmeler, OAuth içerir. OAuth güvenli yetkilendirme web, mobil ve Masaüstü uygulamalarından basit ve standart bir yöntemini sağlar. açık bir protokoldür. ASP.NET Web Forms şablonu, Facebook, Twitter, Google ve Microsoft kimlik doğrulama sağlayıcıları kullanıma sunmak için OAuth kullanır. Bu öğreticide yalnızca Google kimlik doğrulama sağlayıcısı olarak kullansa da, herhangi bir sağlayıcı kullanmak için kodu kolayca değiştirebilirsiniz. Diğer sağlayıcılar uygulamak için adım çok benzer adımlarla Bu öğreticide görürsünüz.

Kimlik doğrulamanın yanı sıra, öğreticiyi de rolleri yetkilendirme uygulamak için kullanır. Yalnızca eklediğiniz kullanıcıları `canEdit` rol verileri değiştirmesi mümkün olacaktır (oluşturma, düzenleme veya silme kişiler).

> [!NOTE] 
> 
> Oturum açma bilgileri test etmek için bir yerel Web sitesi URL'si kullanamazsınız. Bu nedenle Windows Canlı uygulamaları yalnızca bir çalışan Web sitesi için Canlı bir URL kabul edin.


Aşağıdaki adımlar, bir Google kimlik doğrulama sağlayıcısı eklemenize olanak sağlar.

1. Açık *uygulama\_Start\Startup.Auth.cs* dosya.
2. Açıklama karakterleri Kaldır `app.UseGoogleAuthentication()` yöntemi olarak görünür, böylece yöntemi izler: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Gidin [Google geliştiriciler konsol](https://console.developers.google.com/). Google developer e-posta hesabınızla (gmail.com) oturum açmanız gerekir. Bir Google hesabı yoksa seçin **hesap oluşturma** bağlantı.   
   Ardından, gördüğünüz **Google geliştiriciler konsol**.   
    ![Google geliştiriciler Konsolu](checkout-and-payment-with-paypal/_static/image8.png)
4. Tıklayın **proje oluştur** düğme ve bir proje adı ve kimliği (varsayılan değerler kullanabilirsiniz) girin. ' A tıklayarak **sözleşmesi onay kutusu** ve **Oluştur** düğmesi.  

    ![Google - yeni proje](checkout-and-payment-with-paypal/_static/image9.png)

   Birkaç saniye içinde yeni proje oluşturulur ve tarayıcınızın yeni projeler sayfası görüntülenir.
5. Sol sekmede tıklayın **API'leri &amp; auth**ve ardından **kimlik bilgilerini**.
6. Tıklayın **yeni istemci kodu oluşturma** altında **OAuth**.   
   **İstemci kodu oluşturma** iletişim kutusu görüntülenir.   
    ![Google - istemci kodu oluşturma](checkout-and-payment-with-paypal/_static/image10.png)
7. İçinde **istemci kodu oluşturma** iletişim kutusunda, varsayılan tutun **Web uygulaması** uygulama türü için.
8. Ayarlama **yetkili JavaScript kaynakları** Bu öğreticide daha önce kullanılan SSL URL'sine (`https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).   
   Uygulamanız için başlangıç URL'dir. Bu örnek için yalnızca localhost test URL'sini girer. Ancak, hesap localhost ve üretim için birden çok URL girin.
9. Ayarlama **yeniden yönlendirme URI'SİNİN yetkili** aşağıdaki: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Bu değer, ASP.NET OAuth URI, kullanıcıların google OAuth sunucusuyla iletişim kurar. Yukarıda kullanılan SSL URL unutmayın ( `https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).
10. Tıklayın **istemci kodu oluşturma** düğmesi.
11. Google geliştiriciler konsolun sol menüsünde **onay ekranında** menü öğesi, e-posta adresi ve ürün adı ayarlayın. Formu tamamladığınızda, tıklayın **Kaydet**.
12. Tıklayın **API'leri** menü öğesi, aşağı kaydırın ve tıklatın **kapalı** düğmesinin yanındaki **Google + API**.   
    Bu seçenek kabul Google + API olanak sağlar.
13. Aynı zamanda güncelleştirmelisiniz **Microsoft.Owin** 3.0.0 sürümüne NuGet paketi.   
    Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** seçip **çözüm için NuGet paketlerini Yönet**.  
    Gelen **NuGet paketlerini Yönet** pencere, bulma ve güncelleştirme **Microsoft.Owin** 3.0.0 sürümüne paket.
14. Visual Studio'da güncelleştirme `UseGoogleAuthentication` yöntemi *Startup.Auth.cs* sayfası kopyalayıp yapıştırarak tarafından **istemci kimliği** ve **gizli** içine yöntemi. **İstemci kimliği** ve **gizli** aşağıda gösterilen değerler örnekleri ve çalışmaz. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Tuşuna **CTRL + F5** oluşturun ve uygulamayı çalıştırın. Tıklayın **oturum** bağlantı.
16. Altında **başka bir hizmete oturum açmak için kullandığınız**, tıklayın **Google**.  
    ![Oturum aç](checkout-and-payment-with-paypal/_static/image11.png)
17. Kimlik bilgilerinizi girmeniz gerekiyorsa, kimlik bilgilerinizi gireceğiniz google siteye yönlendirileceksiniz.  
    ![Google - oturum açma](checkout-and-payment-with-paypal/_static/image12.png)
18. Kimlik bilgilerinizi girdikten sonra yeni oluşturduğunuz web uygulamasına izin vermek için istenir.  
    ![Proje varsayılan hizmet hesabı](checkout-and-payment-with-paypal/_static/image13.png)
19. Tıklayın **kabul**. Artık geri yönlendirilirsiniz **kaydetme** sayfasının **WingtipToys** Burada, kaydedebilir Google hesabınızı uygulama.  
    ![Google hesabınızla kaydolun](checkout-and-payment-with-paypal/_static/image14.png)
20. Gmail hesabınızı için kullanılan yerel e-posta kayıt adı değiştirme seçeneğiniz vardır, ancak genellikle varsayılan e-posta diğer adı (diğer bir deyişle, bir kimlik doğrulaması için kullanılan) tutmak istersiniz. Tıklayın **oturum** yukarıda da gösterildiği gibi.

### <a name="modifying-login-functionality"></a>Oturum açma işlevselliği değiştirme

Daha önce belirtildiği gibi Bu öğretici serisinde, kullanıcı kayıt işlevselliğinin ASP.NET Web Forms şablon varsayılan olarak dahil edilmiştir. Varsayılan değiştirecek artık *Login.aspx* ve *Register.aspx* çağrılacak sayfaları `MigrateCart` yöntemi. `MigrateCart` Yöntemi yeni oturum açmış kullanıcının anonim bir alışveriş sepeti ile ilişkilendirir. Kullanıcı ilişkilendirme ve alışveriş sepeti Wingtip Toys örnek uygulama ziyaretleri kullanıcının alışveriş sepetini tutmak mümkün olacaktır.

1. İçinde **Çözüm Gezgini**, bulma ve açma *hesabı* klasör.
2. Adlı arka plan kod sayfasını değiştirin *Login.aspx.cs* aşağıdaki gibi görünmesi, sarı ile vurgulanmış kodu eklemek için:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Kaydet *Login.aspx.cs* dosya.

Şimdilik tanımı yok için uyarıyı göz ardı edebilirsiniz `MigrateCart` yöntemi. Biraz daha sonra Bu öğreticide, onu eklenecektir.

*Login.aspx.cs* arka plan kod dosyasında bir oturum açma yöntemini destekler. Login.aspx sayfasına inceleyerek bu sayfada bir "Oturum Aç" düğmesine içerdiğini göreceksiniz olduğunda Tetikleyicileri tıklayın `LogIn` arka plan kod işleyicisi.

Zaman `Login` metodunda *Login.aspx.cs* çağrılır, alışveriş sepetine adlı yeni bir örneğini `usersShoppingCart` oluşturulur. Kimliği (GUID) alışveriş sepetini alınır ve kümesine `cartId` değişkeni. Ardından, `MigrateCart` yöntemi çağrıldığında, her ikisi de geçirme `cartId` ve bu yöntem için oturum açma kullanıcı adı. Alışveriş sepeti taşındığında, anonim alışveriş sepetini tanımlamak için kullanılan GUID kullanıcı adı ile değiştirilir.

Değiştirme yanı sıra *Login.aspx.cs* kullanıcı oturum açtığında alışveriş sepetini geçirme için arka plan kod dosyasını da değiştirmeniz gerekir *Register.aspx.cs arka plan kod dosyası* alışveriş sepetini geçirmek için Kullanıcı yeni bir hesabı oluşturur ve açar.

1. İçinde *hesabı* arka plan kod dosyası adında klasör, açık *Register.aspx.cs*.
2. Aşağıdaki gibi görünür, böylece arka plan kod dosyası sarı renkli kod dahil olmak üzere değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Kaydet *Register.aspx.cs* dosya. Bir kez daha, hakkında uyarı yoksayılabilir `MigrateCart` yöntemi.

Kod içinde kullandığınız fark `CreateUser_Click` olay işleyicisi, kullanılan kodu çok benzer `LogIn` yöntemi. Kullanıcı kaydeder veya bir çağrı sitesine oturum açması `MigrateCart` yöntemi oluşturulacak.

## <a name="migrating-the-shopping-cart"></a>Alışveriş sepetini geçirme

Güncelleştirilmiş oturum açma ve kayıt sürecini olduğuna göre alışveriş sepeti kullanarak geçirmek için kod ekleyebilirsiniz `MigrateCart` yöntemi.

1. İçinde **Çözüm Gezgini**, bulma *mantıksal* klasörü ve açık *ShoppingCartActions.cs* sınıf dosyası.
2. Varolan kodda için sarı renkle vurgulanmış kodu ekleyin *ShoppingCartActions.cs* dosya, böylece kodda *ShoppingCartActions.cs* dosya şu şekilde görünür:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart` Yöntemi, kullanıcının alışveriş sepetini bulmak için mevcut cartId kullanır. Ardından, kod tüm alışveriş sepeti öğeler arasında döngü ve değiştirir `CartId` özelliği (belirtildiği gibi `CartItem` şema) oturum açmış kullanıcı adına sahip.

### <a name="updating-the-database-connection"></a>Veritabanı bağlantısı güncelleştiriliyor

Kullanarak bu öğreticiyi takip ediyorsanız **önceden oluşturulmuş** Wingtip Toys örnek uygulama, varsayılan üyelik veritabanını yeniden oluşturmanız gerekir. Varsayılan bağlantı dizesini değiştirerek uygulamanın sonraki açılışında üyelik veritabanı oluşturulacak.

1. Açık *Web.config* projenin köküne dosya.
2. Varsayılan bağlantı dizesini güncelleştirin, böylece şu şekilde görünür:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>PayPal tümleştirme

PayPal ödemelerini çevrimiçi satıcı tarafından kabul eden bir web tabanlı faturalandırma platformudur. Bu öğreticinin sonraki PayPal'ın Express bırakıyorsa uygulamanızla tümleştirme açıklanmaktadır. Express kullanıma alma müşterileriniz, PayPal, alışveriş sepetine eklendi öğeler için ödemenize olanak tanır.

### <a name="create-paylpal-test-accounts"></a>PaylPal Test hesapları oluşturma

Sınama ortamı PayPal kullanmak için oluşturma ve bir geliştirici test hesabının doğrulanması gerekir. Bir alıcı test hesabı ve satıcı test hesabı oluşturmak için geliştirici test hesabı kullanır. Geliştirici hesabı kimlik bilgilerini sına PayPal sınama ortamına erişmek Wingtip Toys örnek uygulamayı da izin verir.

1. Bir tarayıcıda, PayPal Geliştirici sitesi test için gidin:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Tıklayarak bir PayPal Geliştirici hesabınız yoksa, yeni bir hesap oluşturun **Kaydol**kayıt adımları izleyerek. PayPal Geliştirici hesabınız varsa, tıklayarak oturum **oturum**. PayPal Geliştirici hesabınızın bu öğreticinin ilerleyen bölümlerinde Wingtip Toys örnek uygulamanızı test etmeniz gerekir.
3. Yalnızca PayPal Geliştirici hesabınız için kaydolduysanız, PayPal içeren PayPal Geliştirici hesabınızı doğrulamak gerekebilir. PayPal, e-posta hesabınıza gönderilen adımları izleyerek hesabınızı doğrulayabilirsiniz. PayPal Geliştirici hesabınızın doğruladıktan sonra site geri PayPal Geliştirici testleri oturum açın.
4. Sonra PayPal Geliştirici sitesi için zaten yoksa, PayPal alıcı test hesabı oluşturmanız gerekir, PayPal Geliştirici hesabınızla oturum vardır. PayPal site tıklayarak bir alıcı test hesabı oluşturmak için **uygulamaları** sekmesine ve ardından **korumalı alan hesapları**.   
 **Korumalı alan test hesapları** sayfası gösterilir.   

    > [!NOTE] 
    > 
    > PayPal Geliştirici sitesi zaten bir satıcı test hesabının sağlar.

    ![Kullanıma alma ve ödeme içeren PayPal - korumalı alan test hesapları](checkout-and-payment-with-paypal/_static/image15.png)
5. Korumalı alan test hesapları sayfasında tıklayın **hesabı oluştur**.
6. Üzerinde **test hesap oluştur** test hesabının e-posta ve seçtiğiniz parolayı bir alıcı sayfayı seçin.   

    > [!NOTE] 
    > 
    > Alıcı e-posta adreslerini ve bu öğreticinin sonunda Wingtip Toys örnek uygulamayı test etmek için parola gerekir.

    ![Kullanıma alma ve ödeme içeren PayPal - korumalı alan test hesapları](checkout-and-payment-with-paypal/_static/image16.png)
7. Alıcı test hesabının tıklayarak oluşturun **hesabı oluştur** düğmesi.  
 **Korumalı alan Test hesapları** sayfası görüntülenir. 

    ![Kullanıma alma ve ödeme içeren PayPal - PaylPal hesapları](checkout-and-payment-with-paypal/_static/image17.png)
8. Üzerinde **korumalı alan test hesapları** sayfasında **kolaylaştırıcısına** e-posta hesabı.  
    **Profili** ve **bildirim** seçenekler görüntülenir.
9. Seçin **profili** seçeneğini belirleyin, ardından tıklayın **API kimlik bilgileri** ticari test hesabının API kimlik bilgilerinizi görüntülemek için.
10. API'yi test et kimlik bilgilerini Not Defteri'ne kopyalayın.

Sınama ortamı PayPal görüntülenen API'yi test et Klasik kimlik bilgilerinizi (kullanıcı adı, parola ve imza) Wingtip Toys örnek uygulamadan API çağrısı gerekir. Sonraki adımda, kimlik bilgilerini ekleyeceksiniz.

### <a name="add-paypal-class-and-api-credentials"></a>PayPal sınıfı ve API kimlik bilgileri ekleme

PayPal kod çoğunu tek bir sınıfta yerleştirmeniz gerekir. Bu sınıf, PayPal ile iletişim kurmak için kullanılan yöntemler içerir. Ayrıca, PayPal kimlik bilgilerinizi bu sınıfa ekleyeceksiniz.

1. Visual Studio içinden Wingtip Toys örnek uygulamayı sağ **mantıksal** klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Altında **Visual C#** gelen **yüklü** seçin sol bölmede **kod**.
3. Ortadaki bölmeden **sınıfı**. Bu yeni bir sınıf adı **PayPalFunctions.cs**.
4. **Ekle**'yi tıklatın.  
   Yeni bir sınıf dosyası Düzenleyicisi'nde görüntülenir.
5. Varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. İşlev çağrıları PayPal sınama ortamına yapabilmeleri için Bu öğreticide daha önce görüntülenen satıcı API kimlik (kullanıcı adı, parola ve imza) ekleyin.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Bu örnek uygulamasında bir C# dosyasına (.cs) kimlik bilgilerini yalnızca ekliyoruz. Ancak, uygulanan bir çözüm içinde bir yapılandırma dosyasında kimlik bilgilerinizi şifreleme düşünmelisiniz.


PayPal işlevselliğin büyük bölümü NVPAPICaller sınıfı içerir. Kod içinde sınıf PayPal sınama ortamından satın test yapmak için gereken yöntemleri sağlar. Aşağıdaki üç PayPal işlevleri satın alımları gerçekleştirmek için kullanılır:

- `SetExpressCheckout` işlevi
- `GetExpressCheckoutDetails` işlevi
- `DoExpressCheckoutPayment` işlevi

`ShortcutExpressCheckout` Yöntemi test satın alma bilgileri ve ürün ayrıntıları alışveriş sepeti ve çağrıları toplar `SetExpressCheckout` PayPal işlevi. `GetCheckoutDetails` Yöntemi onaylar satın alma ayrıntıları ve çağrı `GetExpressCheckoutDetails` test satın almadan önce PayPal işlevi. `DoCheckoutPayment` Yöntemi çağırarak test ortamı'ndan test satın tamamlandığında `DoExpressCheckoutPayment` PayPal işlevi. Geri kalan kod, PayPal yöntemleri ve dizeleri kodlama, dizeleri kod çözme, dizileri işleme ve kimlik bilgilerini belirleme gibi işlemi destekler.

> [!NOTE] 
> 
> PayPal göre isteğe bağlı bir satın alma ayrıntıları dahil etmenize olanak verir [PayPal'ın API belirtimine](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Wingtip Toys örnek uygulamadaki kodu genişleterek, yerelleştirme ayrıntıları, ürün tanımları, vergi, bir müşteri hizmeti numarası yanı sıra diğer birçok isteğe bağlı alanlar içerebilir.


Dikkat belirtilen dönüş ve iptal URL'leri **ShortcutExpressCheckout** yöntemi bir bağlantı noktası numarasını kullanın.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Yaygın olarak bağlantı noktası 44300 Visual Web Developer SSL kullanarak bir web projesi çalıştığında, web sunucusu için kullanılır. Yukarıda gösterildiği gibi bağlantı noktası numarasını 44300 olur. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarasını görebilirsiniz. Bağlantı noktası numarası gereksinimlerinizi destesinin doğru kodda başarılı ayarlamak için bu öğreticinin sonunda Wingtip Toys örnek uygulamayı çalıştırın. Bu öğreticinin sonraki bölümünde, yerel ana bilgisayar bağlantı noktası numarasını almak ve PayPal sınıfını güncelleştirme açıklanmaktadır.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>PayPal sınıfında LocalHost bağlantı noktası numarasını güncelleştir

Wingtip Toys örnek uygulama PayPal test sitesine gidip Wingtip Toys örnek uygulamayı yerel örneğine döndüren ürünleri satın alabilir. Yerel olarak çalıştırma bağlantı noktası numarası belirtmenize gerek doğru URL'ye geri PayPal sahip olmak için örnek uygulama yukarıda belirtilen PayPal kod.

1. Proje adına sağ tıklayın (**WingtipToys**) içinde **Çözüm Gezgini** seçip **özellikleri**.
2. Sol sütunda seçin **Web** sekmesi.
3. Bağlantı noktası numarasından almak **proje URL'si** kutusu.
4. Gerekirse, güncelleştirme `returnURL` ve `cancelURL` PayPal sınıfında (`NVPAPICaller`) içinde *PayPalFunctions.cs* dosyasını web uygulamanıza bağlantı noktası numarasını kullanın:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Artık eklemiş olduğunuz koddan yerel Web uygulamanız için beklenen bağlantı noktası eşleşir. PayPal, yerel makinenizde URL'nin doğru döndürülecek mümkün olacaktır.

### <a name="add-the-paypal-checkout-button"></a>PayPal ödeme düğmesi ekleme

Birincil PayPal işlevleri örnek uygulamaya eklenen, biçimlendirme ve bu işlevleri çağırmak için gereken kod ekleme başlayabilirsiniz. İlk olarak, kullanıcı alışveriş sepeti sayfasında göreceği çıkış düğmesi eklemeniz gerekir.

1. Açık *ShoppingCart.aspx* dosya.
2. Dosyasının alt kısmına kaydırın ve bulma `<!--Checkout Placeholder -->` açıklaması.
3. Açıklamanın değiştirin bir `ImageButton` böylece işaretleme şu şekilde değiştirilir:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. İçinde *ShoppingCart.aspx.cs* sonra dosya `UpdateBtn_Click` dosyasının sonuna yakın olay işleyicisi ekleme `CheckOutBtn_Click` olay işleyicisi:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Ayrıca *ShoppingCart.aspx.cs* dosyasında, bir başvuru ekleyin `CheckoutBtn`, böylece yeni görüntüyü düğme gibi başvurulur:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Her ikisi de yaptığınız değişiklikleri kaydetmek *ShoppingCart.aspx* dosya ve *ShoppingCart.aspx.cs* dosya.
7. Menüden **hata ayıklama**-&gt;**derleme WingtipToys**.  
   Proje yeniden oluşturulacak yeni eklenen ile **ImageButton** denetimi.

### <a name="send-purchase-details-to-paypal"></a>Satın alma ayrıntıları için PayPal Gönder

Kullanıcı tıkladığında **kullanıma alma** alışveriş sepeti sayfası düğmesine (*ShoppingCart.aspx*), satın alma işlemi başlatmak. Aşağıdaki kod, ürünleri satın almak için gereken ilk PayPal işlevi çağırır.

1. Gelen *kullanıma alma* arka plan kod dosyası adında klasör, açık *CheckoutStart.aspx.cs*.   
   Arka plan kod dosyasını açın emin olun.
2. Varolan kodu aşağıdakiyle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Uygulamanın kullanıcı tıkladığında **kullanıma alma** , tarayıcı alışveriş sepeti sayfası düğmesine gidin *CheckoutStart.aspx* sayfası. Zaman *CheckoutStart.aspx* sayfa yüklendiğinde `ShortcutExpressCheckout` yöntemi çağrılır. Bu noktada, kullanıcı, PayPal test web sitesine aktarılır. PayPal sitesinde, kullanıcı, PayPal kimlik bilgilerini girer, satın alma ayrıntıları gözden geçirmeleri, PayPal anlaşmasını kabul eder ve Wingtip Toys örnek uygulamaya döndürür burada `ShortcutExpressCheckout` yöntemi tamamlar. Zaman `ShortcutExpressCheckout` yöntemi tamamlandığında, kullanıcıya yönlendireceği *CheckoutReview.aspx* belirtilen sayfa `ShortcutExpressCheckout` yöntemi. Bu, kullanıcının Wingtip Toys örnek uygulama sipariş ayrıntıları gözden izin verir.

### <a name="review-order-details"></a>Sipariş ayrıntılarını gözden geçir

PayPal döndüren sonra *CheckoutReview.aspx* Wingtip Toys örnek uygulamanın sayfasında sipariş ayrıntılarını görüntüler. Bu sayfa, ürünleri satın almadan önce sipariş ayrıntılarını gözden geçirmek kullanıcı sağlar. *CheckoutReview.aspx* sayfası şu şekilde oluşturulmalıdır:

1. İçinde *kullanıma alma* klasör, açık sayfanın adlı *CheckoutReview.aspx*.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Adlı arka plan kod sayfasını açın *CheckoutReview.aspx.cs* ve varolan kodu aşağıdakiyle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView** denetimi, PayPal döndürülen sipariş ayrıntılarını görüntülemek için kullanılır. Ayrıca, yukarıdaki kod Wingtip Toys veritabanına sipariş ayrıntılarını kaydeder bir `OrderDetail` nesne. Kullanıcı tıkladığında üzerinde **tam sipariş** düğmesi, bunların yönlendirilir için *CheckoutComplete.aspx* sayfası.

> [!NOTE] 
> 
> **İpucu**
> 
> Biçimlendirme *CheckoutReview.aspx* sayfasında, dikkat `<ItemStyle>` etiketi öğelerin stilini değiştirmek için kullanılan **DetailsView** sayfanın alt kısmındaki denetim. Sayfanın görüntüleyerek **Tasarım görünümü** (seçerek **tasarım** Visual Studio'nun sol alt köşesindeki), ardından seçerek **DetailsView** denetlemek ve seçme **Akıllı etiket** (ok simgesine tıklayın denetimin sağ), görüyor olmanız **DetailsView görevleri**.
> 
> ![Kullanıma alma ve ödeme içeren PayPal - alanları Düzenle](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Seçerek **alanları Düzenle**, **alanları** iletişim kutusu görüntülenir. Bu iletişim kutusunda, kolay görsel özelliklerini aşağıdaki gibi denetleyebilirsiniz **ItemStyle**, biri **DetailsView** denetimi.
> 
> ![Kullanıma alma ve ödeme içeren PayPal - alanları iletişim](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Satın alma işlemi

*CheckoutComplete.aspx* sayfası, PayPal ' satın yapar. Yukarıda belirtildiği gibi kullanıcı üzerinde tıklatmalısınız **tam sipariş** uygulama gideceği önce düğme *CheckoutComplete.aspx* sayfası.

1. İçinde *kullanıma alma* klasör, açık sayfanın adlı *CheckoutComplete.aspx*.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Adlı arka plan kod sayfasını açın *CheckoutComplete.aspx.cs* ve varolan kodu aşağıdakiyle değiştirin:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Zaman *CheckoutComplete.aspx* sayfası yüklendikten `DoCheckoutPayment` yöntemi çağrılır. Daha önce belirtildiği `DoCheckoutPayment` yöntemi PayPal test ortamı'ndan satın alma işlemini tamamlar. PayPal satın alma siparişinin tamamlandıktan sonra *CheckoutComplete.aspx* sayfası görüntüler ödeme işlem `ID` yerleştirmesi için.

### <a name="handle-cancel-purchase"></a>Tanıtıcı iptal satın alma

Satın alma işlemini iptal etmek kullanıcı karar verirse, bunlar yönlendirilirsiniz *CheckoutCancel.aspx* sayfa burada görürsünüz, siparişi iptal edildi.

1. Sayfa adında açın *CheckoutCancel.aspx* içinde *kullanıma alma* klasör.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Satın alma hataları işleme

Satın alma işlemi sırasında hatalar olarak işleneceğini *CheckoutError.aspx* sayfası. Arka plan kod, *CheckoutStart.aspx* sayfasında *CheckoutReview.aspx* sayfasında ve *CheckoutComplete.aspx* sayfası her yönlendirir ve için  *CheckoutError.aspx* bir hata oluşursa sayfasında.

1. Sayfa adında açın *CheckoutError.aspx* içinde *kullanıma alma* klasör.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx* kullanıma alma işlemi sırasında bir hata oluştuğunda ile hata ayrıntılarını sayfası görüntülenir.

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Ürünler satın alma görmek için uygulamayı çalıştırın. PayPal sınama ortamı çalışıyor olacak olduğunu unutmayın. Hiçbir gerçek parayı değiştirilen.

1. Tüm dosyaları Visual Studio'da kaydedilir emin olun.
2. Bir Web tarayıcısı açın ve gidin [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Bu öğreticide daha önce oluşturduğunuz PayPal Geliştirici hesabınız ile oturum açın.  
   PayPal'ın Geliştirici korumalı alan için oturum açmış olmanız gerekir. [ https://developer.paypal.com ](https://developer.paypal.com/) express kullanıma almayı test etmek için. Bu yalnızca sınama, PayPal'ın Canlı ortamına değil PayPal'ın Korumalı alan için geçerlidir.
4. Visual Studio'da **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
   Veritabanını yeniden oluşturur sonra tarayıcıyı açın Göster ve *Default.aspx* sayfası.
5. "Arabalar" gibi bir ürün kategorisini seçtiğinizde ve ardından üç farklı ürün alışveriş sepetine eklemek **Sepete Ekle** her ürün yanında.  
   Alışveriş sepeti seçtiğiniz ürün görüntüler.
6. Tıklayın **PayPal** kullanıma düğmesi. 

    ![Kullanıma alma ve ödeme içeren PayPal - Sepeti](checkout-and-payment-with-paypal/_static/image20.png)

   Kullanıma Wingtip Toys örnek uygulama için bir kullanıcı hesabı olması gerekir.
7. Tıklayın **Google** mevcut gmail.com e-posta hesabınızla oturum açmanız sayfanın sağ taraftaki bağlantı.  
   Gmail.com hesabı yoksa, test etmek için bir tane oluşturabilirsiniz [www.gmail.com](https://www.gmail.com/). "Kaydet"'e tıklayarak standart bir yerel hesap de kullanabilirsiniz. 

    ![Kullanıma alma ve ödeme içeren PayPal - oturum açın](checkout-and-payment-with-paypal/_static/image21.png)
8. Gmail hesabınızı ve parola ile oturum açın. 

    ![Kullanıma alma ve ödeme içeren PayPal - Gmail oturum açma](checkout-and-payment-with-paypal/_static/image22.png)
9. Tıklayın **oturum** düğmesini gmail hesabınızı Wingtip Toys örnek uygulama kullanıcı adıyla kaydedin. 

    ![Kullanıma alma ve ödeme içeren PayPal - kayıt hesabı](checkout-and-payment-with-paypal/_static/image23.png)
10. PayPal test sitesinde ekleyin, **alıcı** e-posta adresi ve Bu öğreticide daha önce oluşturduğunuz parola'a tıklayın **oturum** düğmesi. 

    ![Kullanıma alma ve ödeme içeren PayPal - PayPal oturum açma](checkout-and-payment-with-paypal/_static/image24.png)
11. PayPal ilkeyi kabul edin ve tıklayın **kabul et ve devam et** düğmesi.  
    Bu sayfa yalnızca olduğuna dikkat edin, bu PayPal hesabını ilk kez görüntülenir. Bu bir test hesabı olduğundan, hiçbir gerçek para değiştirilir yeniden unutmayın. 

    ![Kullanıma alma ve ödeme içeren PayPal - PayPal İlkesi](checkout-and-payment-with-paypal/_static/image25.png)
12. Test ortamı İncele sayfası ve tıklatın PayPal sipariş bilgileri gözden geçirin **devam**. 

    ![Kullanıma alma ve ödeme içeren PayPal - bilgilerini gözden geçirme](checkout-and-payment-with-paypal/_static/image26.png)
13. Üzerinde *CheckoutReview.aspx* sayfasında sipariş miktarı doğrulayın ve oluşturulan teslimat adresini görüntüleyin. ' A tıklayarak **tam sipariş** düğmesi. 

    ![Kullanıma alma ve ödeme içeren PayPal - siparişi gözden geçirme](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx** sayfası, bir ödeme işlem kimliğiyle görüntülenir 

    ![Kullanıma alma ve ödeme içeren PayPal - kullanıma alma tamamlandı](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Veritabanı gözden geçirme

Uygulamayı çalıştırdıktan sonra Wingtip Toys örnek uygulama veritabanında güncelleştirilmiş veriler inceleyerek, uygulamanın başarıyla ürünleri satın kaydedilen görebilirsiniz.

Bulunan verileri inceleyebilirsiniz *Wingtiptoys.mdf* veritabanı dosyasını kullanarak **veritabanı Gezgini** penceresi (**Sunucu Gezgini** Visual Studio'daki) yaptığınız gibi daha önce Bu öğretici serisinde.

1. Hala açık değilse tarayıcı penceresini kapatın.
2. Visual Studio'da **tüm dosyaları göster** simgesi en üstündeki **Çözüm Gezgini** genişletmenize olanak **uygulama\_veri** klasör.
3. Genişletin **uygulama\_veri** klasör.  
 Seçmek için gerek duyabileceğiniz **tüm dosyaları göster** klasörü simgesi.
4. Sağ *Wingtiptoys.mdf* veritabanı dosyası ve select **açık**.  
    **Sunucu Gezgini** görüntülenir.
5. Genişletin **tabloları** klasör.
6. Sağ **siparişler**tablosunu seçip **tablo verilerini Göster**.  
 **Siparişler** tablo görüntülenir.
7. Gözden geçirme **PaymentTransactionID** başarılı işlem onaylamak için sütun. 

    ![Kullanıma alma ve ödeme içeren PayPal - gözden geçirme veritabanı](checkout-and-payment-with-paypal/_static/image29.png)
8. Kapat **siparişler** Tablo penceresi.
9. Sunucu Gezgini'nde sağ **OrderDetails** tablosunu seçip **tablo verilerini Göster**.
10. Gözden geçirme `OrderId` ve `Username` değerler **OrderDetails** tablo. Bu değerlerin eşleşmesi Not `OrderId` ve `Username` değerleri dahil **siparişler** tablo.
11. Kapat **OrderDetails** Tablo penceresi.
12. Wingtip Toys veritabanı dosyasına sağ tıklayın (*Wingtiptoys.mdf*) seçip **yakın bağlantı**.
13. Görmüyorsanız, **Çözüm Gezgini** penceresinde tıklayın **Çözüm Gezgini** kısmındaki **Sunucu Gezgini** gösterilecek penceresi **Çözüm Gezgini**  yeniden.

## <a name="summary"></a>Özet

Bu öğreticide, sırası ve Sipariş Ayrıntısı şemaları ürün satın alma işlemini izlemek için eklendi. Wingtip Toys örnek uygulamaya PayPal işlevselliği de tümleştirilen.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET yapılandırmasına genel bakış](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Azure App Service'e bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET Web Forms uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Sorumluluk reddi

Bu öğreticide, örnek kod içerir. Bu örnek kod, herhangi bir garanti olmaksızın "olduğu gibi" sağlanır. Buna uygun olarak, Microsoft doğruluğu, bütünlüğünü veya örnek kod kalitesini garanti etmez. Kendi sorumluluğunuzdadır örnek kodu kullanmak kabul etmiş olursunuz. Hiçbir koşulda Microsoft herhangi bir yolla, tüm örnek kodlar, ancak bunlarla sınırlı olmamak üzere, herhangi bir hata veya eksikliklerden herhangi bir örnek kod, içerik veya tüm kaybı veya zarar sonucunda herhangi bir örnek kod kullanımı sonucunda herhangi bir türde içerik için tutulamaz. İşbu sözleşme ile bildirilir ve ücretsiz olduğu için kabul kaydedin ve Microsoft gelen ve tüm kaybı, veri kaybı, yaralanma veya tür, ancak bunlarla sınırlı olmaksızın tarafından occasioned dahil olmak üzere veya gönderdiğiniz malzemesini kaynaklanan hasarın taleplerini karşı görmememizi iletme kullanın veya, ancak bunlarla sınırlı olmamak sıralamadaki bildirilmeksizin dahil olmak üzere üzerinde güvenin.

> [!div class="step-by-step"]
> [Önceki](shopping-cart.md)
> [İleri](membership-and-administration.md)
