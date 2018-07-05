---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Üyelik ve yönetim | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 58eda260ccf2d0f237aaade65017760ed87a8c4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368304"
---
<a name="membership-and-administration"></a>Üyelik ve yönetim
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide bir özel Rol Ekle ve ASP.NET Identity kullanmak için Wingtip Toys örnek uygulamayı güncelleştirme gösterilmektedir. Ayrıca, içinden bir özel role sahip kullanıcı ekleyebilir ve ürünleri Web sitesinden kaldırma Yönetim sayfası uygulamak nasıl gösterir.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ASP.NET web uygulaması oluşturmak için kullanılan üyelik sisteminin ve ASP.NET 4.5 içinde kullanılabilir. ASP.NET Identity kullanılan şablonlar yanı sıra, Visual Studio 2013 Web formları projesi şablonu [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), ve [ASP.NET tek sayfalık uygulaması](../../../../single-page-application/index.md). Ayrıca, boş bir Web uygulaması ile başlattığınızda NuGet kullanarak ASP.NET kimlik sistemine yükleyebilirsiniz. Ancak bu öğretici serisinde kullanmanız **Web Forms**projecttemplate, ASP.NET kimlik sistemi içerir. ASP.NET Identity kullanıcı özel profil verileri uygulama verileriyle tümleştirmeyi kolaylaştırır. Ayrıca, ASP.NET Identity kullanıcı profilleri için Kalıcılık modeli uygulamanızda seçmenize olanak sağlar. Bir SQL Server veritabanı veya başka bir veri deposuna verileri depolayabilirsiniz dahil olmak üzere *NoSQL* verileri Windows Azure depolama tabloları gibi depolar.

Bu öğreticide, Wingtip Toys öğretici serisinde "Kullanıma alma ve ödeme içeren PayPal" başlıklı önceki öğreticiye üzerine inşa edilmiştir.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Uygulamaya özel bir rol ve bir kullanıcı eklemek için kod kullanma
- Sayfa ve yönetimsel klasör erişimi kısıtlamak nasıl.
- Gezinti özel rolüne ait kullanıcı için nasıl.
- Model bağlama doldurmak için nasıl kullanılacağını bir [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) denetimi ile ürün kategorileri.
- Web uygulaması kullanarak bir dosyayı karşıya yükleme [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) denetimi.
- Giriş doğrulaması uygulamak için doğrulama denetimleri kullanma
- Ekleme ve uygulama kaldırma ürünleri.

## <a name="these-features-are-included-in-the-tutorial"></a>Öğreticinin bu özellikler dahildir:

- ASP.NET Kimlik
- Yapılandırma ve yetkilendirme
- Model bağlama
- Örtük doğrulama

ASP.NET Web Forms üyelik özellikleri sağlar. Varsayılan şablonu kullanarak uygulama çalışırken, hemen kullanabileceğiniz yerleşik üyelik işlevselliğini sahip olursunuz. Bu öğreticide ASP.NET Identity özel bir rol ekleyin ve bir kullanıcı o role atamak için nasıl kullanılacağını gösterir. Yönetim klasörüne erişimi kısıtlama öğreneceksiniz. Bir sayfa ürünleri ekleyip ve eklendikten sonra bir ürün Önizleme için özel bir rol olan bir kullanıcıya izin veren yönetim klasöre ekleyeceksiniz.

## <a name="adding-a-custom-role"></a>Özel bir rol ekleme

ASP.NET Identity kullanılarak, bir özel Rol Ekle ve kullanıcı kodu kullanarak bu role atayın.

1. İçinde **Çözüm Gezgini**, sağ *mantıksal* klasörü ve yeni bir sınıf oluşturun.
2. Yeni bir sınıf adı *RoleActions.cs*.
3. Aşağıdaki gibi görünür, böylece kodu değiştirin:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. İçinde **Çözüm Gezgini**açın *Global.asax.cs* dosya.
5. Değiştirme *Global.asax.cs* sarı renkle vurgulanır, böylece gibi görünen kod ekleyerek dosyası:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Dikkat `AddUserAndRole` kırmızıyla altı çizilir. AddUserAndRole kod çift tıklayın.  
   Vurgulanan yöntemi başındaki "A" harfini altı çizili olacaktır.
7. "A" harfini gelin ve kullanıcı Arabirimi için bir yöntem Saplaması olanak tanıyan `AddUserAndRole` yöntemi. 

    ![Üyelik ve Advministration - metot taslağı oluştur](membership-and-administration/_static/image1.png)
8. Seçeneğini tıklatın:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Açık *RoleActions.cs* dosya *mantıksal* klasör.  
   `AddUserAndRole` Yöntemi için sınıf dosyası eklendi.
10. Değiştirme *RoleActions.cs* kaldırarak dosya `NotImplementedeException` ve sarı ile vurgulanmış kod ekleyerek aşağıdaki gibi görünür:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Yukarıdaki kod, önce bir üyelik veritabanı için veritabanı bağlamı oluşturur. Üyelik veritabanı olarak da depolanan bir *.mdf* dosyası *uygulama\_veri* klasör. İlk kullanıcı bu web uygulamasına oturum açıldıktan sonra bu veritabanı görüntülemeniz mümkün olacaktır. 

> [!NOTE] 
> 
> Ürün verileriyle birlikte üyelik veri depolamak istiyorsanız, aynı kullanmayı düşünebilirsiniz **DbContext** yukarıdaki kodda ürün verileri depolamak için kullanılır.


 *İç* anahtar sözcüğü, türler (sınıflar gibi) ve tür üyeleri (örneğin, yöntemler veya Özellikler) için bir erişim değiştiricisidir. İç türleri veya üyeleri yalnızca aynı bütünleştirilmiş kodda yer alan dosyaları erişilebilir *(.dll* dosyası). Uygulamanızı bir derleme dosyası oluşturduğunuzda *(.dll*) oluşturulmuş uygulamanızı çalıştırdığınızda yürütülen kodu içerir. 

A `RoleStore` rol yönetimi sağlayan bir nesne üzerinde veritabanı bağlamı göre oluşturulur.

> [!NOTE] 
> 
> Kullanırken dikkat edin `RoleStore` nesnesi oluşturulur, genel kullanır `IdentityRole` türü. Diğer bir deyişle `RoleStore` içerecek şekilde yalnızca izin `IdentityRole` nesneleri. Ayrıca genel türleri kullanarak, bellek kaynakları daha iyi işlenir.


Ardından, `RoleManager` nesne, temel alınarak oluşturulur `RoleStore` oluşturduğunuz nesne. `RoleManager` nesneyi kullanıma sunan rolü otomatik olarak yapılan değişiklikleri kaydetmek için kullanılan API ilgili `RoleStore`. `RoleManager` İçerecek şekilde yalnızca izin `IdentityRole` kod kullandığından nesneleri `<IdentityRole>` genel tür.

Çağırmanızı `RoleExists` "canEdit" rolü üyelik veritabanında var olup olmadığını belirlemek için yöntemi. Yüklü değilse, rolü oluşturun.

Oluşturma `UserManager` nesne daha karmaşık gibi görünüyor `RoleManager` denetlemek, ancak bunu neredeyse aynıdır. Yalnızca bir satır yerine birkaç kodlanmıştır. Burada, parantez içinde yer alan yeni bir nesne olarak geçirdiğiniz parametre örnekleme.

Ardından yeni bir oluşturarak "canEditUser" kullanıcı oluşturun `ApplicationUser` nesne. Ardından, kullanıcı başarıyla oluşturursanız, kullanıcı yeni role ekleyin.

> [!NOTE] 
> 
> Hata işleme sırasında "ASP.NET hata işleme" öğreticide daha sonra Bu öğretici serisinin güncelleştirilecektir.


Uygulamayı bir sonraki başlatılışında "canEditUser" adlı kullanıcı, uygulamasının "canEdit" adlı rol olarak eklenir. Bu öğreticinin ilerleyen bölümlerinde Bu öğretici sırasında eklenen olacak ek özelliklerini görüntülemek için "canEditUser" kullanıcı olarak oturum. API ASP.NET kimliği hakkında bilgi için [Microsoft.ASPNET.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). ASP.NET kimlik sistemi başlatma hakkında ek ayrıntılar için bkz. [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Yönetim sayfasına erişimi kısıtlama

Wingtip Toys örnek uygulama, anonim kullanıcılar ve oturum açmış kullanıcılar görüntülemek ve ürünleri satın sağlar. Ancak özel "canEdit" rolüne sahip oturum açmış kullanıcı ekleme ve kaldırma ürünler için kısıtlanmış bir sayfaya erişebilir.

#### <a name="add-an-administration-folder-and-page"></a>Bir yönetim klasörü ve sayfa ekleyin

Ardından, adlı bir klasör oluşturur *yönetici* örnek uygulaması için Wingtip Toys özel role ait "canEditUser" kullanıcı.

1. Proje adına sağ tıklayın (**Wingtip Toys**) içinde **Çözüm Gezgini** seçip **Ekle**  - &gt; **Yeniklasör**.
2. Yeni klasör adı *yönetici*.
3. Sağ *yönetici* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
4. Seçin <strong>Visual C#</strong> - &gt; <strong>Web</strong> soldaki şablonları grubu. Orta listesinden <strong>ana sayfa ile Web formu</strong>, adlandırın <em>AdminPage.aspx</em><strong>,</strong> seçip <strong>Ekle</strong>.
5. Seçin *Site.Master* dosyası ana sayfa olarak ve ardından **Tamam**.

#### <a name="add-a-webconfig-file"></a>Bir Web.config dosyasına ekleyin

Ekleyerek bir *Web.config* dosyasını *yönetici* klasöründe klasördeki sayfaya erişimi kısıtlayabilir.

1. Sağ *yönetici* klasörü ve select **Ekle**  - &gt; **yeni öğe**.  
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Visual C# web şablonları listesinden seçin <strong>Web yapılandırma dosyası</strong>Orta listeden varsayılan adını kabul <em>Web.config</em><strong>,</strong> seçip <strong>Ekleme</strong>.
3. Var olan XML içeriği değiştirin *Web.config* aşağıdaki dosya:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Kaydet *Web.config* dosya. *Web.config* dosyasını belirtir uygulamanın "canEdit" role ait yalnızca kullanıcı yer alan sayfa erişebilir *yönetici* klasör.

### <a name="including-custom-role-navigation"></a>Özel rol Gezinti dahil olmak üzere

Uygulama Yönetim kısmına gitmek kullanıcının özel "canEdit" rolü etkinleştirmek için bir bağlantı ekleyin *Site.Master* sayfası. "CanEdit" rolüne ait olan kullanıcıların görmeye yalnızca **yönetici** bağlantı ve yönetim bölümündeki erişebilirsiniz.

1. Çözüm Gezgini içinde bulma ve açma *Site.Master* sayfası.
2. Kullanıcı "canEdit" rolü için bir bağlantı oluşturmak için aşağıdaki sırasız liste için sarı renkle vurgulanmış biçimlendirme eklemek `<ul>` öğe listesi olarak görünür, böylece izler:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Açık *Site.Master.cs* dosya. Olun **yönetici** bağlantısını için sarı renkle vurgulanmış kodu ekleyerek yalnızca "canEditUser" kullanıcıya görünür `Page_Load` işleyici. `Page_Load` İşleyicisi aşağıdaki gibi görünür:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Sayfa yüklendiğinde, kod, oturum açmış kullanıcının "canEdit" rolünü olup olmadığını denetler. Kullanıcı bağlantısı içeren bir span öğesi "canEdit" rolüne aitse *AdminPage.aspx* sayfa (ve sonuç olarak aralık içinde bağlantı) yapılan görünür.

### <a name="enabling-product-administration"></a>Ürün Yönetimi etkinleştirme

Şu ana kadar "canEdit" rolü oluşturduğunuz ve bir "canEditUser" kullanıcı, bir yönetim klasör ve bir yönetim sayfası eklendi. Erişim Hakları Yönetimi klasörü ve sayfa için ayarladığınız ve uygulamaya kullanıcı "canEdit" rolü için bir gezinti bağlantısı ekledik. Sonra işaretlemede ekleyeceksiniz *AdminPage.aspx* sayfasında ve kodu *AdminPage.aspx.cs* ekleme ve kaldırma ürünleri "canEdit" rolüne sahip kullanıcının arka plan kod dosyası.

1. İçinde **Çözüm Gezgini**açın *AdminPage.aspx* dosya *yönetici* klasör.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Ardından, açık *AdminPage.aspx.cs* arka plan kod dosyasına sağ tıklayarak *AdminPage.aspx* tıklayıp **kodu görüntüle**.
4. Varolan kodda değiştirin *AdminPage.aspx.cs* arka plan kod dosyasına aşağıdaki kod ile:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

İçin girdiğiniz kod *AdminPage.aspx.cs* arka plan kod dosyası, adında bir sınıf `AddProducts` ürünleri veritabanına ekleme asıl işi yapar. Bu sınıf, şimdi oluşturacak şekilde henüz mevcut değil.

1. İçinde **Çözüm Gezgini**, sağ *mantıksal* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **kod** soldaki şablonları grubu. Ardından, **sınıfı**ortasından listesinde ve adlandırın *AddProducts.cs*.   
   Yeni bir sınıf dosyası görüntülenir.
3. Varolan kodu aşağıdakiyle değiştirin:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx* sayfası ekleme ve kaldırma ürünleri "canEdit" rolüne ait kullanıcı sağlar. Yeni ürün eklendiğinde, ürün hakkındaki ayrıntıları doğrulanır ve ardından veritabanına girilir. Yeni ürün web uygulamasının tüm kullanıcılar için hemen kullanılabilir.

#### <a name="unobtrusive-validation"></a>Örtük doğrulama

Kullanıcı sağlar ürün ayrıntıları *AdminPage.aspx* sayfa doğrulama denetimleri kullanarak doğrulanır (`RequiredFieldValidator` ve `RegularExpressionValidator`). Bu denetimleri otomatik olarak örtük doğrulama kullanın. Örtük doğrulama sayfanın bir seyahat doğrulanması için sunucuya gerektirmez anlamına istemci tarafı doğrulama mantığı için JavaScript kullanılacak doğrulama denetimleri sağlar. Varsayılan olarak, örtük doğrulama dahil *Web.config* dosya tabanlı aşağıdaki yapılandırma ayarları:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Normal İfadeler

Ürün fiyatı temel alınarak *AdminPage.aspx* sayfası doğrulanmış kullanarak bir **RegularExpressionValidator** denetimi. Bu denetim, ilişkili giriş denetiminin ("AddProductPrice" metin kutusu) değeri normal ifade tarafından belirtilen desenle eşleşip eşleşmediğini onaylar. Hızla bulup belirli karakter desenlerini eşleşen olanak tanıyan bir desen eşleştirme gösterimi bir düzenli ifadedir. **RegularExpressionValidator** denetim adlı bir özellik içerdiğini `ValidationExpression` aşağıda gösterildiği gibi fiyat giriş doğrulamak için kullanılan normal ifade içeriyor:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Dosya yükleme denetimi

Giriş ve doğrulama denetimleri yanı sıra, eklediğiniz **FileUpload** denetimini *AdminPage.aspx* sayfası. Bu denetim, dosyaları karşıya yükleme olanağı sağlar. Bu durumda, yalnızca görüntü dosyalarını karşıya yüklenecek vermiş olursunuz. Arka plan kod dosyasında (*AdminPage.aspx.cs*), `AddProductButton` tıklandığında, kod denetimleri `HasFile` özelliği **FileUpload** denetimi. Denetim bir dosya varsa ve dosya türü (dosya uzantısına bağlı olarak) izin veriliyorsa, görüntünün kaydedilen *görüntüleri* klasörü ve *görüntüleri/Thumbs* uygulamanın klasör.

#### <a name="model-binding"></a>Model bağlama

Daha önce Bu öğretici serisinde, model bağlama doldurmak için kullanılan bir **ListView** denetimi, bir **FormsView** denetimi, bir **GridView** denetimi ve bir  **Detailview'u** denetimi. Bu öğreticide, model bağlama doldurmak için kullandığınız bir **DropDownList** denetimi ile ürün kategorileri listesi.

Eklediğiniz biçimlendirmeyi *AdminPage.aspx* dosyasını içeren bir **DropDownList** adlı Denetim `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Model bağlama bu doldurmak için kullandığınız **DropDownList** ayarlayarak `ItemType` özniteliği ve `SelectMethod` özniteliği. `ItemType` Özniteliği belirtir, kullandığınız `WingtipToys.Models.Category` denetim doldurulurken yazın. Bu öğretici serisinin bu tür başında oluşturarak tanımlanan `Category` sınıfı (aşağıda gösterilmiştir). `Category` Sınıfı *modelleri* klasörün içine *Category.cs* dosya.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod` Özniteliği **DropDownList** denetimi belirtir, kullandığınız `GetCategories` arka plan kod dosyasına dahil (aşağıda gösterilen) yöntemi diğer bir deyişle (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Bu yöntem belirten bir `IQueryable` arabirimi yönelik bir sorgu değerlendirmek için kullanılan bir `Category` türü. Döndürülen değer doldurmak için kullanılan **DropDownList** sayfanın biçimlendirmede (*AdminPage.aspx*).

Ayarlayarak belirtilen listedeki her öğe için görüntülenen metni `DataTextField` özniteliği. `DataTextField` Özniteliğini kullanan `CategoryName` , `Category` sınıfı (her kategoride görüntülemek için yukarıda) **DropDownList** denetimi. Bir öğe seçildiğinde, geçirilen gerçek değeri **DropDownList** denetim temel `DataValueField` özniteliği. `DataValueField` Özniteliği `CategoryID` tanımlamak olarak `Category` sınıfı (yukarıda gösterilmiştir).

### <a name="how-the-application-will-work"></a>Uygulamayı nasıl çalışır

"CanEdit" rolüne ait kullanıcı ilk kez, sayfaya gittiğinde `DropDownAddCategory` **DropDownList** denetimi, yukarıda açıklanan şekilde doldurulur. `DropDownRemoveProduct` **DropDownList** denetim de aynı yaklaşımı kullanarak ürünleri ile doldurulur. "CanEdit" rolüne ait kullanıcı kategorisi türünü seçer ve ürün ayrıntıları ekler (**adı**, **açıklama**, **fiyat**, ve **resimdosyası**). "CanEdit" rolüne ait kullanıcı tıkladığında **Ürün Ekle** düğme `AddProductButton_Click` olay işleyicisi tetiklenir. `AddProductButton_Click` Bulunan arka plan kod dosyasında olay işleyicisi (*AdminPage.aspx.cs*) izin verilen dosya türleri eşleştiğinden emin olmak için resim dosyası denetler *(.gif*, *.png*, *.jpeg*, veya *.jpg*). Ardından, görüntü dosyası Wingtip Toys örnek uygulamanın bir klasöre kaydedilir. Ardından, yeni ürün veritabanına eklenir. Yeni bir ürün, yeni bir örneğini ekleme yapmanın `AddProducts` sınıf oluşturulur ve ürün adı. `AddProducts` Sınıfında adlı bir yöntem `AddProduct`, ve ürünleri nesne ürünleri veritabanına eklemek için bu yöntemi çağırır.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Kod veritabanına başarıyla yeni ürün ekler, sayfa ile sorgu dizesi değerini yeniden `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Sayfa yüklediğinde, sorgu dizesini URL'ye eklenir. Sayfayı yeniden yüklemeyi "canEdit" rolüne ait kullanıcı hemen güncelleştirmeleri görebilirsiniz **DropDownList** denetimlerini *AdminPage.aspx* sayfası. Ayrıca, URL sorgu dizesi dahil olmak üzere sayfa bir başarı iletisi "canEdit" rolüne ait kullanıcı görüntüleyebilirsiniz.

Zaman *AdminPage.aspx* sayfasında yeniden yükler, `Page_Load` olay çağrılır.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load` Olay işleyicisi, sorgu dizesi değerini denetler ve bir başarı iletisi gösterilip gösterilmeyeceğini belirler.

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Nasıl ekleyebileceğinizi görmek için uygulamayı şimdi, silme ve güncelleştirme öğe alışveriş sepetine çalıştırabilirsiniz. Tüm öğelerin alışveriş sepetine toplam maliyeti alışveriş sepeti toplam ücreti yansıtılır.

1. Çözüm Gezgini'nde basın **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
   Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Tıklayın **oturum** sayfanın üstündeki bağlantısı. 

    ![Üyelik ve yönetim -, bağlantısında oturum](membership-and-administration/_static/image2.png)

   *Login.aspx* sayfası görüntülenir.
3. Aşağıdaki kullanıcı adını ve parolayı kullanın:  
   Kullanıcı adı: canEditUser@wingtiptoys.com  
   Password: Pa$$word1 

    ![Üyelik ve yönetim - oturum açma sayfasının](membership-and-administration/_static/image3.png)
4. Tıklayın **oturum** sayfanın alt kısmındaki düğmesi.
5. Sonraki sayfanın üstünde seçin **yönetici** gitmek için bağlantıyı *AdminPage.aspx* sayfası. 

    ![Üyelik ve yönetim - yönetici bağlantısı](membership-and-administration/_static/image4.png)
6. Giriş doğrulaması test etmek için **Ürün Ekle** tüm ürün ayrıntıları eklemeden düğmesi. 

    ![Üyelik ve yönetim - Yönetim sayfası](membership-and-administration/_static/image5.png)

   Gerekli alan iletileri görüntülendiğini dikkat edin.
7. Yeni ürün ayrıntılarını ekleyin ve ardından **Ürün Ekle** düğmesi. 

    ![Üyelik ve yönetim - Ürün Ekle](membership-and-administration/_static/image6.png)
8. Seçin **ürünleri** eklediğiniz yeni ürünü görüntülemek için üst gezinti menüsünde. 

    ![Üyelik ve yönetim - yeni ürün Göster](membership-and-administration/_static/image7.png)
9. Tıklayın **yönetici** Yönetim sayfasına dönmek için bağlantı.
10. İçinde **kaldırmak ürün** bölümü seçme sayfası, eklediğiniz yeni ürünü **DropDownListBox**.
11. Tıklayın **kaldırmak ürün** uygulamadan yeni ürünü kaldırmak için düğmeyi. 

    ![Üyelik ve yönetim - Remove ürün](membership-and-administration/_static/image8.png)
12. Seçin **ürünleri** ürün kaldırıldığını doğrulamak için üst gezinti menüsünde.
13. Tıklayın **oturumunu** yönetim modu mevcut.   
    Üst gezinti bölmesi artık gösteren uyarı **yönetici** menü öğesi.

## <a name="summary"></a>Özet

Bu öğreticide, özel bir rol ve özel rol, kısıtlı erişim sayfasının ve yönetim klasörü ait bir kullanıcı eklendi ve gezinti özel rolüne ait kullanıcı için sağlanan. Model bağlama doldurmak için kullanılan bir **DropDownList** verilerle denetimi. Uygulanan **FileUpload** ve doğrulama denetimi. Ayrıca, ekleyin ve ürünleri bir veritabanından kaldırma öğrendiniz. Sonraki öğreticide, ASP.NET yönlendirmesi uygulamak öğreneceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[Web.config - yetkilendirme öğesi](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET kimlik](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Bir Azure Web sitesine bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET Web Forms uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Önceki](checkout-and-payment-with-paypal.md)
> [İleri](url-routing.md)
