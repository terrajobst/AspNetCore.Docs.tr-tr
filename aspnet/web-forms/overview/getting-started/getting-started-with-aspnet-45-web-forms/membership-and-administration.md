---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Üyelik ve yönetim | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 166bc642ea2083f455be0648e424f0b0ae3b082c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="membership-and-administration"></a>Üyelik ve yönetim
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğretici bir özel rolü eklemek ve ASP.NET Identity kullanmak için Wingtip Toys örnek uygulamayı güncelleştirme gösterilmiştir. Ayrıca, hangi özel bir rol ile kullanıcı ekleyip çıkarabilirsiniz ürünleri Web sitesinden bir yönetim sayfası uygulamak nasıl gösterir.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ASP.NET web uygulaması oluşturmak için kullanılan üyelik sistemi ve ASP.NET 4.5 içinde kullanılabilir. ASP.NET kimliği için şablonlar yanı sıra Visual Studio 2013 Web Forms proje şablonu kullanılır [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), ve [ASP.NET tek sayfa uygulaması](../../../../single-page-application/index.md). Boş bir Web uygulamasıyla başlattığınızda NuGet kullanarak ASP.NET Identity sistem özellikle de yükleyebilirsiniz. Ancak, Bu öğretici serisinde kullanmanız **Web Forms**ASP.NET Identity sistem içeren projecttemplate. ASP.NET Identity, kullanıcı özel profil verileri uygulama verileriyle tümleştirmeyi kolaylaştırır. Ayrıca, ASP.NET Identity Kalıcılık modeli kullanıcı profilleri için uygulamanızda seçmenize olanak sağlar. Verileri bir SQL Server veritabanı veya başka bir veri deposunda depolayabilirsiniz dahil olmak üzere *NoSQL* verileri Windows Azure depolama tabloları gibi depolar.

Bu öğretici Wingtip Toys öğretici serisinde "Checkout ve ödeme ile PayPal" başlıklı önceki öğretici inşa edilmiştir.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Uygulamaya özel bir rol ve bir kullanıcı eklemek üzere kod kullanmak nasıl.
- Sayfa ve yönetim klasörü için erişimi kısıtlamak nasıl.
- Özel rolüne ait kullanıcı için Gezinti sağlamak nasıl.
- Model bağlama doldurmak için nasıl kullanılacağını bir [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) ürün kategorileri denetimiyle.
- Web uygulaması kullanarak bir dosyayı karşıya yüklemeyi nasıl [dosya yükleme](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) denetim.
- Giriş doğrulaması uygulamak için doğrulama denetimleri kullanma
- Ekleme ve kaldırma ürünleri uygulamadan.

## <a name="these-features-are-included-in-the-tutorial"></a>Bu özellikleri öğreticide bulunmaktadır:

- ASP.NET Kimlik
- Yapılandırma ve yetkilendirme
- Model bağlama
- Örtük doğrulama

ASP.NET Web Forms üyelik özellikleri sağlar. Varsayılan şablonu kullanarak, uygulama çalıştığında hemen kullanabileceğiniz yerleşik üyelik işlevselliğe sahip. Bu öğreticide, ASP.NET Identity özel bir rol ekleyin ve bir kullanıcı o role atamak için nasıl kullanılacağını gösterir. Yönetim klasörüne erişimi kısıtlamak öğreneceksiniz. Bir sayfa ekleyin ve ürünleri kaldırmak için ve eklendikten sonra bir ürün önizlemek için özel bir rol sahip bir kullanıcı izin veren yönetim klasöre ekleyeceksiniz.

## <a name="adding-a-custom-role"></a>Özel bir rol ekleme

ASP.NET Kimliği'ni kullanarak özel bir rol ekleyin ve bir kullanıcı kodu kullanarak bu role atayın.

1. İçinde **Çözüm Gezgini**, sağ tıklayın *mantığı* klasörü ve yeni bir sınıf oluşturun.
2. Yeni sınıf *RoleActions.cs*.
3. Böylece aşağıdaki gibi görünür kodu değiştirin:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. İçinde **Çözüm Gezgini**, açık *Global.asax.cs* dosya.
5. Değiştirme *Global.asax.cs* dosyasını aşağıdaki gibi görünmesi sarı ile vurgulanmış kodu ekleyerek düzenleyin:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Dikkat `AddUserAndRole` kırmızıyla altı çizilir. AddUserAndRole kodu çift tıklayın.  
   Vurgulanan yöntemi başındaki harf "A" altı çizili olacaktır.
7. "A" harfi getirin ve için bir yöntem saplama oluşturmanızı sağlar UI'ı tıklatın `AddUserAndRole` yöntemi. 

    ![Üyelik ve Advministration - yöntemi saplama oluştur](membership-and-administration/_static/image1.png)
8. Başlıklı seçeneği tıklatın:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Açık *RoleActions.cs* dosya *mantığı* klasör.  
   `AddUserAndRole` Yöntemi sınıf dosyasına eklendi.
10. Değiştirme *RoleActions.cs* kaldırarak dosya `NotImplementedeException` ve sarı ile vurgulanmış kodu ekleyerek şu şekilde görünür:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Yukarıdaki kod, üyelik veritabanının veritabanı bağlamının ilk oluşturur. Üyelik veritabanının da olarak saklanan bir *.mdf* dosyasını *uygulama\_veri* klasör. İlk kullanıcı bu web uygulaması için oturum açtıktan sonra bu veritabanını görüntülemek kuramaz. 

> [!NOTE] 
> 
> Ürün verileri birlikte üyelik verileri saklamak isterseniz, aynı kullanarak düşünebilirsiniz **DbContext** yukarıdaki kodda ürün verilerini depolamak için kullanılan.


 *İç* sözcüktür türleri (örneğin, sınıflar) ve (örneğin, yöntemler veya Özellikler) tür üyeleri için bir erişim değiştiricisi. İç türleri veya üyeleri yalnızca aynı bütünleştirilmiş kodda yer alan dosyaları içinde erişilebilir *(.dll* dosyası). Uygulamanız, bir derleme dosyası oluşturduğunuzda *(.dll*) oluşturulan uygulamanızı çalıştırdığınızda yürütülen kod içerir. 

A `RoleStore` rol yönetimi sağlar, nesne veritabanı içeriğine göre oluşturulur.

> [!NOTE] 
> 
> Fark olduğunda `RoleStore` nesnesi oluşturulur genel kullanan `IdentityRole` türü. Bunun anlamı `RoleStore` içerecek şekilde yalnızca izin `IdentityRole` nesneleri. Ayrıca genel türler kullanarak, bellek kaynakları daha iyi işlenir.


Ardından, `RoleManager` nesne, temel alınarak oluşturulur `RoleStore` oluşturduğunuz nesne. `RoleManager` nesne çıkarır rol ilgili otomatik olarak yapılan değişiklikleri kaydetmek için kullanılan API `RoleStore`. `RoleManager` İçerecek şekilde yalnızca izin `IdentityRole` kodu kullandığından nesneleri `<IdentityRole>` genel tür.

Çağırmanız `RoleExists` "canEdit" rol üyelik veritabanında mevcut olup olmadığını belirlemek amacıyla yöntemi. Değilse, rolü oluşturun.

Oluşturma `UserManager` nesne görünüyor daha karmaşıktır `RoleManager` denetlemek, ancak bunu neredeyse aynıdır. Yalnızca tek bir çizgi yerine birkaç kodlanmıştır. Burada, parantez içinde yer alan yeni bir nesne olarak geçirme parametre örnekleme.

Ardından yeni bir oluşturarak "canEditUser" kullanıcı oluşturun `ApplicationUser` nesnesi. Ardından, kullanıcı başarıyla oluşturursanız, yeni rol için kullanıcı ekleyin.

> [!NOTE] 
> 
> Hata işleme sırasında "ASP.NET işleme hatası" öğreticide daha sonra Bu öğretici seri güncelleştirilir.


Uygulamayı bir sonraki başlatılışında "canEditUser" adlı kullanıcı uygulamanın "canEdit" adlı rolü olarak eklenir. Bu öğreticide daha sonra Bu öğreticide sırasında eklenen olacak ek özellikleri görüntülemek için "canEditUser" kullanıcı olarak oturum. ASP.NET kimliği hakkında API Ayrıntılar için bkz: [Microsoft.ASPNET.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). ASP.NET kimlik sistemi başlatma hakkında daha fazla bilgi için bkz: [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Yönetim sayfasına erişimi kısıtlama

Wingtip Toys örnek uygulama, anonim kullanıcılar hem de oturum açan kullanıcılar görüntülemek ve ürünleri satın almak sağlar. Ancak, özel "canEdit" rolüne sahip oturum açma kullanıcı ekleyin ve ürün kaldırmak için kısıtlı bir sayfayı erişebilir.

#### <a name="add-an-administration-folder-and-page"></a>Bir yönetim klasörü ve sayfa ekleyin

Ardından, adında bir klasör oluşturur *yönetici* Wingtip Toys özel role ait "canEditUser" kullanıcı için örnek uygulama.

1. Proje adına sağ tıklayın (**Wingtip Toys**) içinde **Çözüm Gezgini** seçip **Ekle**  - &gt; **yeni klasör**.
2. Yeni bir klasör adı *yönetici*.
3. Sağ *yönetici* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
4. Seçin <strong>Visual C#</strong> - &gt; <strong>Web</strong> soldaki templates grubu. Orta listesinden <strong>ana sayfa ile Web Form</strong>, adlandırın <em>AdminPage.aspx</em><strong>,</strong> ve ardından <strong>Ekle</strong>.
5. Seçin *Site.Master* dosya ana sayfa ve ardından **Tamam**.

#### <a name="add-a-webconfig-file"></a>Bir Web.config dosyası ekler

Ekleyerek bir *Web.config* dosya *yönetici* klasörü, klasördeki sayfasına erişimi kısıtlayabilir.

1. Sağ *yönetici* klasörü ve select **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Visual C# web şablonları listesinden seçin <strong>Web yapılandırma dosyası</strong>Orta listeden varsayılan adı kabul <em>Web.config</em><strong>,</strong> seçip <strong>Eklemek</strong>.
3. Varolan XML içeriği Değiştir *Web.config* aşağıdaki dosyasıyla:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Kaydet *Web.config* dosya. *Web.config* dosyayı belirtir uygulama "canEdit" role ait yalnızca kullanıcı içinde yer alan sayfa erişebilir *yönetici* klasör.

### <a name="including-custom-role-navigation"></a>Özel rol Gezinti dahil

Uygulama Yönetim bölümüne gidin kullanıcıya özel "canEdit" rolünü etkinleştirmek için bir bağlantı Ekle *Site.Master* sayfası. "CanEdit" role ait kullanıcılar görmeye yalnızca **yönetici** bağlantı ve Yönetim bölümünde erişebilirsiniz.

1. Çözüm Gezgini'nde bulup açın *Site.Master* sayfası.
2. "CanEdit" rolünün bir kullanıcı için bir bağlantı oluşturmak için aşağıdaki sırasız liste sarıya vurgulanan biçimlendirme eklemek `<ul>` öğe listesi olarak görünür, böylece izler:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Açık *Site.Master.cs* dosya. Olun **yönetici** bağlantı sarıya vurgulanan kod ekleyerek yalnızca "canEditUser" kullanıcıya görünen `Page_Load` işleyicisi. `Page_Load` İşleyici şu şekilde görünür:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Sayfa yüklendiğinde kodunu oturum açma kullanıcı "canEdit" rolüne sahip olup olmadığını denetler. Kullanıcı bağlantısı içeren bir span öğesi "canEdit" role aitse *AdminPage.aspx* sayfa (ve bu nedenle aralık bağlantıda) hale görünür.

### <a name="enabling-product-administration"></a>Ürün Yönetimi etkinleştirme

Şu ana kadar "canEdit" rolü oluşturduktan ve bir "canEditUser" kullanıcı, bir yönetim klasörü ve bir yönetim sayfası eklenir. Sayfa ve yönetim klasörü için erişim haklarını ayarladıktan ve "canEdit" rolü kullanıcı Gezinti bağlantısı uygulamaya eklediniz. Sonra biçimlendirme ekleyeceksiniz *AdminPage.aspx* sayfasında ve kodu *AdminPage.aspx.cs* eklemek ve ürünleri kaldırmak "canEdit" rolüne sahip kullanıcı etkinleştirecek arka plan kod dosyası.

1. İçinde **Çözüm Gezgini**, açık *AdminPage.aspx* dosya *yönetici* klasör.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Ardından, açık *AdminPage.aspx.cs* sağ tıklanarak arka plan kodu dosya *AdminPage.aspx* tıklatıp **görünümü kodu**.
4. Varolan kodla *AdminPage.aspx.cs* aşağıdaki kod ile arka plan kod dosyası:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

İçin girdiğiniz kod *AdminPage.aspx.cs* arka plan kod dosyası adlı bir sınıf `AddProducts` veritabanına ürünleri ekleme asıl işi yapar. Bu sınıf, şimdi oluşturacak şekilde henüz yok.

1. İçinde **Çözüm Gezgini**, sağ *mantığı* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **kod** soldaki templates grubu. Ardından, seçin **sınıfı**ortasından listelemek ve adlandırın *AddProducts.cs*.   
   Yeni sınıf dosyası görüntülenir.
3. Var olan kodu aşağıdakilerle değiştirin:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx* sayfası eklemek ve ürünleri kaldırmak "canEdit" rolüne ait kullanıcı sağlar. Yeni bir ürün eklendiğinde, ürünle ilgili ayrıntıları doğrulandı ve veritabanına girilir. Yeni ürün web uygulamasının tüm kullanıcılar için hemen kullanılabilir.

#### <a name="unobtrusive-validation"></a>Örtük doğrulama

Kullanıcı sağlar ürün ayrıntıları *AdminPage.aspx* sayfa doğrulama denetimlerini kullanarak doğrulanır (`RequiredFieldValidator` ve `RegularExpressionValidator`). Bu denetimler, otomatik olarak örtük doğrulama kullanın. Örtük doğrulama sayfa seyahat doğrulanması için sunucuya gerektirmez anlamına gelir istemci tarafı doğrulama mantığı için JavaScript kullanmak doğrulama denetimleri sağlar. Varsayılan olarak, örtük doğrulama dahil *Web.config* dosya tabanlı üzerinde şu yapılandırma ayarı:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Normal İfadeler

Ürün Fiyat *AdminPage.aspx* sayfa doğrulanmış kullanarak bir **RegularExpressionValidator** denetim. Bu denetim ilişkili giriş denetiminin ("AddProductPrice" TextBox) değeri normal ifadeyle belirtilen desen ile eşleşip eşleşmediğini onaylar. Normal bir ifade hızla bulmak ve özel karakter düzenleri eşleşir olanak tanıyan bir desen eşleştirme bir gösterimidir. **RegularExpressionValidator** denetim adlı bir özellik içerdiğini `ValidationExpression` aşağıda gösterildiği gibi fiyat giriş doğrulamak için kullanılan normal ifade içerir:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Dosya yükleme denetimi

Eklediğiniz giriş ve doğrulama denetimleri yanı sıra **dosya yükleme** denetimini *AdminPage.aspx* sayfası. Bu denetim dosyaları karşıya yükleme yeteneği sağlar. Bu durumda, yalnızca resim dosyaları karşıya yüklenecek sağlamaktadır. Arka plan kod dosyasına (*AdminPage.aspx.cs*), `AddProductButton` tıklandığında, kod denetimleri `HasFile` özelliği **dosya yükleme** denetim. Denetim bir dosya varsa ve dosya türünü (dosya uzantısına göre) izin veriliyorsa, görüntünün kaydedilir *görüntüleri* klasör ve *görüntüleri/başparmak* uygulamanın klasör.

#### <a name="model-binding"></a>Model bağlama

Bu öğretici serisinde daha önce model bağlama doldurmak için kullanılan bir **ListView** denetimi, bir **FormsView** denetimi, bir **GridView** denetimi ve  **DetailView** denetim. Bu öğreticide, model bağlama doldurmak için kullandığınız bir **DropDownList** denetimi ile ürün kategorileri listesi.

Eklediğiniz biçimlendirme *AdminPage.aspx* dosyasını içeren bir **DropDownList** adlı Denetim `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Model bağlama bu doldurmak için kullandığınız **DropDownList** ayarlayarak `ItemType` özniteliği ve `SelectMethod` özniteliği. `ItemType` Özniteliği belirtir, kullandığınız `WingtipToys.Models.Category` denetimi doldurulurken yazın. Oluşturarak başında bu türü Bu öğretici dizi tanımlanmış `Category` sınıfı (aşağıda gösterilen). `Category` Sınıfı olan *modelleri* klasöre *Category.cs* dosya.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod` Özniteliği **DropDownList** denetim belirtir, kullandığınız `GetCategories` arka plan kod dosyasına dahil (aşağıda gösterilen) yöntemi yani (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Bu yöntem belirleyen bir `IQueryable` arabirimi bir sorgu değerlendirmek için kullanılan bir `Category` türü. Döndürülen değer doldurmak için kullanılan **DropDownList** sayfasının biçimlendirmede (*AdminPage.aspx*).

Listedeki her öğeye ayarlayarak belirtilen için görüntülenen metin `DataTextField` özniteliği. `DataTextField` Özniteliğini kullanır `CategoryName` , `Category` sınıfı (her kategoride görüntülemek için yukarıda) **DropDownList** denetim. Bir öğe seçildiğinde, geçirilen gerçek değer **DropDownList** denetim temel `DataValueField` özniteliği. `DataValueField` Özniteliği `CategoryID` tanımlama gibi `Category` sınıfı (yukarıda gösterilen).

### <a name="how-the-application-will-work"></a>Uygulamayı nasıl çalışır

"CanEdit" rolüne ait kullanıcı ilk kez sayfaya gittiğinde `DropDownAddCategory` **DropDownList** denetim yukarıda açıklandığı gibi doldurulur. `DropDownRemoveProduct` **DropDownList** denetimi de aynı yaklaşımı kullanarak ürünleri ile doldurulur. "CanEdit" rolüne ait kullanıcı kategorisi türünü seçer ve ürün ayrıntıları ekler (**adı**, **açıklama**, **fiyat**, ve **görüntü dosyası**). "CanEdit" rolüne ait kullanıcı tıkladığında **Ürün Ekle** düğmesini `AddProductButton_Click` olay işleyicisi tetiklenir. `AddProductButton_Click` Olay işleyicisi bulunan arka plan kod dosyasına (*AdminPage.aspx.cs*) izin verilen dosya türleri eşleştiğinden emin olmak için görüntü dosyası denetler *(.gif*, *.png*, *.jpeg*, veya *.jpg*). Ardından, görüntü dosyası Wingtip Toys örnek uygulama klasörüne kaydedilir. Ardından, yeni ürün veritabanına eklenir. Yeni bir ürün, yeni bir örneğini ekleme gerçekleştirmek için `AddProducts` sınıfı oluşturulur ve ürünleri adlandırılır. `AddProducts` Sınıfına sahip adlı bir yöntem `AddProduct`, ve ürünleri nesne ürünleri veritabanına eklemek için bu yöntemi çağırır.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Kod veritabanına başarıyla yeni ürün eklerse, sayfa sorgu dize değeri ile yeniden `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Sayfa yüklediğinde, sorgu dizesi URL'de dahil edilir. Sayfa yeniden yükleniyor "canEdit" rolüne ait kullanıcı hemen güncelleştirmeleri görebilirsiniz **DropDownList** denetimlerini *AdminPage.aspx* sayfası. Ayrıca, URL sorgu dizesi dahil olmak üzere sayfa bir başarı iletisi için "canEdit" rolüne ait kullanıcı görüntüleyebilirsiniz.

Zaman *AdminPage.aspx* sayfa yeniden yükler, `Page_Load` olay çağrılır.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load` Olay işleyicisi sorgu dizesi değerini denetler ve bir başarı iletisi gösterilip gösterilmeyeceğini belirler.

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Alışveriş sepeti içinde nasıl ekleyebileceğiniz görmek için uygulamayı şimdi, silme ve güncelleştirme öğeleri çalıştırabilirsiniz. Alışveriş sepeti toplam alışveriş sepeti tüm öğelerde toplam maliyeti yansıtır.

1. Çözüm Gezgini'nde basın **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
   Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Tıklatın **oturum** sayfanın üst kısmındaki bağlantı. 

    ![Üyelik ve yönetim - oturum bağlantısı](membership-and-administration/_static/image2.png)

   *Login.aspx* sayfası görüntülenir.
3. Aşağıdaki kullanıcı adını ve parolasını kullanın:  
   Kullanıcı adı: canEditUser@wingtiptoys.com  
   Password: Pa$$word1 

    ![Üyelik ve yönetim - oturum açma sayfasının](membership-and-administration/_static/image3.png)
4. Tıklatın **oturum** düğmesi sayfanın yakın.
5. Sonraki sayfanın en üstünde seçin **yönetici** gitmek için bağlantı *AdminPage.aspx* sayfası. 

    ![Üyelik ve yönetim - yönetim bağlantı](membership-and-administration/_static/image4.png)
6. Giriş doğrulaması sınamak için **Ürün Ekle** tüm ürün ayrıntıları eklemeden düğmesi. 

    ![Üyelik ve yönetim - Yönetim sayfası](membership-and-administration/_static/image5.png)

   Gerekli alan iletileri görüntülenir dikkat edin.
7. Yeni bir ürün ayrıntılarını ekleyin ve ardından **Ürün Ekle** düğmesi. 

    ![Üyelik ve yönetim - ürün ekleme](membership-and-administration/_static/image6.png)
8. Seçin **ürünleri** eklediğiniz yeni ürün görüntülemek için üst gezinti menüsünde. 

    ![Üyelik ve yönetim - yeni ürün Göster](membership-and-administration/_static/image7.png)
9. Tıklatın **yönetici** Yönetim sayfasına geri dönmek için bağlantı.
10. İçinde **kaldırmak ürün** bölümde eklediğiniz yeni ürün seçme sayfası, **DropDownListBox**.
11. Tıklatın **kaldırmak ürün** uygulamadan yeni ürün kaldırmak için düğmeyi. 

    ![Üyelik ve yönetim - Kaldır ürün](membership-and-administration/_static/image8.png)
12. Seçin **ürünleri** Ürün kaldırıldı onaylamak için üst gezinti menüsünde.
13. Tıklatın **oturumunu** yönetim modu bulunmasını.   
    Üst gezinti bölmesi artık gösterilmemektedir bildirimi **yönetici** menü öğesi.

## <a name="summary"></a>Özet

Bu öğreticide, özel bir rol ve özel rol, yönetim klasörü ve sayfa, kısıtlı erişim ait olan bir kullanıcı eklendi ve gezinti özel rolüne ait kullanıcı için sağlanan. Model bağlama doldurmak için kullanılan bir **DropDownList** verilerle denetim. Uygulanan **dosya yükleme** ve doğrulama denetimi. Ayrıca, ekleme ve ürün veritabanından kaldırmak öğrendiniz. Sonraki öğreticide ASP.NET yönlendirme uygulamak öğreneceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[Web.config - yetkilendirme öğesi](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET kimliği](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Bir Azure Web sitesine Güvenli ASP.NET Web Forms uygulama üyeliği, OAuth ve SQL veritabanı ile dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Önceki](checkout-and-payment-with-paypal.md)
> [sonraki](url-routing.md)
