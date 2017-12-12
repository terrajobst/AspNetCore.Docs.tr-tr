---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Forms ASP.NET 4.5 Web yenilikler | Microsoft Docs
author: rick-anderson
description: "ASP.NET Web Forms yeni sürümü bazı verilerle çalışırken, kullanıcı deneyimini geliştirmeye odaklanmış geliştirmeler sunar. Önceki sürümlerinde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 23e38416dc294a1a07cb320cf5ab328fa036d1e8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>ASP.NET 4.5 Web formları yenilikleri
====================
tarafından [Web Camps ekibi](https://twitter.com/webcamps)

> ASP.NET Web Forms yeni sürümü bazı verilerle çalışırken, kullanıcı deneyimini geliştirmeye odaklanmış geliştirmeler sunar.
> 
> Web Forms veri bağlama nesnesi üyesinin değerini yaymak üzere kullanırken, önceki sürümlerde Bind() veya Eval() veri bağlama ifadeleri kullanılır. ASP.NET'ın yeni sürümünde, hangi türde veri yeni bir ItemType özelliğini kullanarak bağlanması için bir denetim geçiyor bildirmek kullanabilirsiniz. Bu özelliği ayarlamak Visual Studio geliştirme deneyimi, IntelliSense, üye gezinti ve derleme zamanı denetimi gibi tüm faydalarını almak için kesin türü belirtilmiş bir değişken kullanmanıza olanak sağlar.
> 
> Veri bağlama denetimleri ile şimdi de seçerek, güncelleştirme, silme ve veri, ekleme için kendi özel yöntemler sayfa denetimleri ve uygulama mantığınızın arasındaki etkileşim basitleştirme belirtebilirsiniz. Ayrıca, model bağlama özellikleri sayfasından verileri doğrudan yöntemi tür parametreleri eşleyebilirsiniz anlamına gelir ASP.NET eklenmiştir.
> 
> Kullanıcı girişini doğrulama da Web Forms en son sürümü ile daha kolay olması gerekir. Şimdi, model sınıflarınızı doğrulama öznitelikleri ile açıklayabilirsiniz **System.ComponentModel.DataAnnotations** ad alanı ve tüm sitenizin denetleyen istek doğrulama bu bilgileri kullanan kullanıcı girişi. Web Forms istemci tarafı doğrulama şimdi temizleyici istemci tarafı kodlar ve örtük JavaScript özellikleri sağlayarak jQuery ile tümleşiktir.
> 
> İstek doğrulama alanında seçmeli olarak uygulamalarınızın belirli kısımlarını ait istek doğrulamayı devre dışı bırakın veya geçersiz istek verileri okuma kolaylaştırmak için iyileştirmeler yapılmıştır.
> 
> Bazı iyileştirmeler HTML5, yeni özelliklerden yararlanmak için sunucu denetimleri Web formları için yapılmıştır:
> 
> - TextBox denetimi metin modu özelliği, e-posta, datetime vb. gibi yeni HTML5 giriş türlerini desteklemek için güncelleştirildi.
> - Dosya yükleme denetimi artık bu HTML5 özelliği destekleyen tarayıcılarda birden çok dosya yüklemelerini destekler.
> - Doğrulayıcı şimdi destek doğrulama HTML5 giriş öğeleri denetler.
> - Bir URL şimdi temsil özniteliklere sahip yeni HTML5 öğeleri destek runat =&quot;server&quot;. Sonuç olarak, URL yollarında ASP.NET kuralları gibi kullanabilirsiniz ~ uygulama kökü temsil etmek için işleci (örneğin, &lt;video runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - UpdatePanel denetim nakil HTML5 giriş alanı desteklemek için düzeltilmiştir.
> 
> Resmi ASP.NET Portalı'nda yeni özelliklerden daha fazla örnek ASP.NET WebForms 4.5 bulabilirsiniz: [ASP.NET 4.5 ve Visual Studio 2012'deki yenilikler](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- Kesin türü belirtilmiş veri bağlama ifadeleri kullanma
- Web Forms yeni model bağlama özelliklerini kullanma
- Arka plan kodu yöntemlere sayfa verileri eşleştirmek için değer sağlayıcıları kullanın
- Kullanıcı girdisi doğrulama için veri ek açıklamaları kullanın
- Web Forms jQuery ile unobstrusive istemci tarafı doğrulama advange alın
- Uygulama ayrıntılı istek doğrulama
- Web Forms işleme zaman uyumsuz sayfası uygulamak

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleme**

Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir. Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek C: kullanarak kod parçacıkları](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:

1. [Alıştırma 1: ASP.NET Web Forms bağlamasında modeli](#Exercise1)
2. [Alıştırma 2: Veri doğrulama](#Exercise2)
3. [Alıştırma 3: Zaman uyumsuz sayfa işleme ASP.NET Web formları](#Exercise3)

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Alıştırma 1: ASP.NET Web Forms bağlamasında modeli

ASP.NET Web Forms yeni sürümünü geliştirmeleri verileriyle çalışırken deneyimini geliştirmeye odaklanmış bir dizi getirmektedir. Bu alıştırmada kesin türü belirtilmiş veri denetimleri hakkında bilgi edinin ve bağlama modeli.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Görev 1 - kesin türü belirtilmiş veri bağlamaları kullanma

Bu görevde, yeni kesin türü belirtilmiş bağlamaları ASP.NET 4.5 içinde kullanılabilir keşfeder.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-ModelBinding/başlangıç/** klasörü.

    1. Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **Customers.aspx** sayfası. Ana denetiminde bir numaralandırılmamış listesi yerleştirin ve her bir müşteri listeleme için içindeki yineleyici denetim içerir. Yineleyici adını ayarlayın **customersRepeater** aşağıdaki kodda gösterildiği gibi.

    Önceki sürümlerinde, Web Forms veri bağlama bir nesne üzerinde bir üyesinin değerini yaymak üzere kullanırken veri bağlama için Eval yöntemine bir çağrı birlikte bir veri bağlama ifadesi geçirme üye adına bir dize olarak kullanmanız.

    Çalışma zamanında Eval çağrıları verilen ada sahip üyesinin değerini okumak için şu anda ilişkili nesne karşı yansıma kullanın ve sonucu HTML'de görüntülemek. Bu yaklaşım rasgele, unshaped veri karşı veri bağlama çok kolay hale getirir.

    Ne yazık ki, IntelliSense de dahil olmak üzere üye adları, gezinti (gibi Tanıma Git) ve derleme zamanı denetimi desteği için Visual Studio harika geliştirme zamanı deneyimi özelliklerinin çoğu kaybedersiniz.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Açık **Customers.aspx.cs** dosya.
4. Aşağıdaki ekleme deyimini kullanarak.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. İçinde **sayfa\_yük** yöntemi, yineleyici müşteriler listesini doldurmak için kodu ekleyin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - bağlı müşteri veri kaynağı*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Çözüm EntityFramework CodeFirst birlikte oluşturmak ve veritabanına erişmek için kullanır. Aşağıdaki kodda customersRepeater veritabanından tüm müşteriler döndürür gerçekleştirilmiş bir sorguya bağlı.
6. Tuşuna **F5** çözümü çalıştırın ve Git **müşteriler** eylem yineleyicideki görmek için sayfayı. Çözüm CodeFirst kullandığından, veritabanı oluşturulur ve uygulama çalıştırıldığında, yerel SQL Express örneği doldurulur.

    ![Bir yineleyici müşterilerle listeleme](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "bir yineleyici müşterilerle listeleme")

    *Bir yineleyici müşterilerle listeleme*

    > [!NOTE]
    > Visual Studio 2012'de IIS Express varsayılan Web geliştirme sunucusudur.
7. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
8. Kesin türü belirtilmiş bağlamaları kullanmak için uygulamayı şimdi değiştirin. Açık **Customers.aspx** sayfasında ve yeni **ItemType** ayarlamak için yineleyici özniteliğinde **müşteri** bağlama türü türü.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    ItemType özelliği, veri türü denetimi bağlanmasını geçecekse ve kesin türü belirtilmiş-içinde veri bağlama denetimi bağlama sayesinde bildirme olanak sağlar.
9. Aşağıdaki kod ile içerik ItemTemplate değiştirin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Yukarıdaki yaklaşımlar ile bir dezavantajı Eval() ve Bind() çağrıları geç özellik adlarını göstermek için dizeleri geçirdiğiniz anlamı bağlama - olmasıdır. Bu üye adları, kod Gezinti (gibi Tanıma Git) desteği ya da derleme zamanı denetimi desteği için IntelliSense almadım anlamına gelir.

    Veri bağlama ifadeleri kapsamında oluşturulacak iki yeni yazılan değişkenler neden ItemType özelliğinin ayarlanması: **öğesi** ve **BindItem**. Veri bağlama ifadelerinde kesin türü belirtilmiş bu değişkenleri kullanmak ve Visual Studio geliştirme deneyimi tüm faydalarını alın.

    &quot; **:** &quot; İfadede kullanılan güvenlik sorunları (örneğin, siteler arası komut dosyası saldırıları) önlemek için çıktı HTML olarak kodlanacak otomatik olarak ayarlanır. Bu gösterim .NET 4'ten beri için yanıt yazılırken kullanılabilir, ancak aynı zamanda veri bağlama ifadelerinde kullanıma sunulmuştur.

    > [!NOTE]
    > Öğe üyesi için tek yönlü bağlama çalışır. İki yönlü bağlama kullanım gerçekleştirmek istiyorsanız **BindItem** üyesi.

    ![Kesin türü belirtilmiş bağlama IntelliSense desteği](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "kesin türü belirtilmiş bağlama IntelliSense desteği")

    *Kesin türü belirtilmiş bağlama IntelliSense desteği*
10. Tuşuna **F5** çözümü çalıştırın ve değişiklikleri beklendiği gibi çalıştığından emin olmak için müşteriler sayfasına gidin.

    ![Müşteri ayrıntıları listeleme](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Müşteri ayrıntıları listeleme")

    *Müşteri ayrıntıları listeleme*
11. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Görev 2 - Web Forms bağlama ile tanışın modeli

ASP.NET Web Forms önceki sürümlerde hem alma ve verileri, güncelleştirme iki yönlü veri bağlamayı gerçekleştirmek istediğinizi olduğunda bir veri kaynağı nesnesi kullanma gereklidir. Bu nesne veri kaynağının, SQL veri kaynağı, bir LINQ veri kaynağı vb. olabilir. Ancak senaryonuza verileri işlemek için özel kod gerekirse, nesnesi veri kaynağını kullanmak için gerekli ve bu bazı dezavantajları getirildi. Örneğin, karmaşık türler önlemek gerekli ve doğrulama mantığını yürütülürken özel durumları işlemek gerekli.

ASP.NET Web Forms'ın yeni sürümünde verilere bağlı denetimler model bağlama destekler. Bu seçin, güncelleştirebilir, Ekle ve arka plan kodu dosyanızı veya başka bir sınıf mantığı çağırmak için doğrudan veri bağlama denetimi yöntemlerini silin, anlamına gelir.

Bu hakkında bilgi için GridView kullanarak yeni ürün kategorilerini liste için kullanacağınız **SelectMethod** özniteliği. Bu öznitelik GridView veri almak için bir yöntem belirtmenize olanak sağlar.

1. Açık **Products.aspx** sayfasında ve içeren bir **GridView**. GridView, kesin türü belirtilmiş bağlamaları kullanın ve sıralama ve disk belleği etkinleştirmek için aşağıda gösterildiği gibi yapılandırın.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Yeni **SelectMethod** çağırmak için GridView yapılandırmak için öznitelik bir **GetCategories** yöntemi verileri seçin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Açık **Products.aspx.cs** arka plan kodu dosya ve aşağıdaki using deyimlerini.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - ad alanları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Özel üye ekleme **ürünleri** sınıfı ve yeni bir örneğini atayın **ProductsContext**. Bu özellik, veritabanına bağlanmak sağlar Entity Framework verileri bağlamındaki depolar.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Oluşturma bir **GetCategories** yöntemi LINQ kullanarak kategorileri listesi alınamadı. Sorgu içerecektir **ürünleri** özelliğini GridView her kategori için ürünleri miktarını gösterebilir. Yöntem olacak şekilde sorguyu temsil eden bir ham Iqueryable nesnesi daha sonra sayfa yaşam döngüsü yürütülen döndürür dikkat edin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > ASP.NET Web Forms önceki sürümlerinde, etkinleştirme sıralama ve bir nesne veri kaynağının bağlamı içinde kendi depo mantığı kullanarak disk belleği, kendi özel kod yazmanıza ve tüm gerekli parametreleri almak için gereklidir. Şimdi, veri bağlama yöntemleri Iqueryable döndürebilir ve bu bir sorguyu temsil hala yürütülmek üzere ASP.NET uygun sıralama eklemek için sorgu ve disk belleği parametrelerini değiştirme ilgilenebilmek.
6. Tuşuna **F5** site hata ayıklamayı Başlat ve ürünler sayfasına gidin. GridView GetCategories yöntemi tarafından döndürülen kategorileri doldurulur görmeniz gerekir.

    ![Model bağlama kullanarak GridView doldurma](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "model bağlama kullanarak GridView doldurma")

    *Model bağlama kullanarak GridView doldurma*
7. Tuşuna **SHIFT**+**F5** hata ayıklamayı durdurun.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Görev 3 - Model bağlama değer sağlayıcıları

Model bağlama yalnızca verilerinizi doğrudan veri bağlama denetimi ile çalışmak için özel yöntemler belirtmenize olanak sağlar, ancak Ayrıca, bu yöntemlerden parametrelerine veri sayfasından eşleştirmenizi sağlar. Yöntem parametresi üzerinde değerinin veri kaynağını belirlemek için değer sağlayıcı özniteliklerini kullanabilirsiniz. Örneğin:

- Sayfadaki denetimleri
- Sorgu dizesi değerleri
- Verileri görüntüleme
- oturum durumu
- Tanımlama bilgileri
- Gönderilen form verileri
- Görünüm durumu
- Özel değer sağlayıcıları de desteklenir

ASP.NET MVC 4 kullandıysanız, model bağlama destek benzer olduğunu fark edeceksiniz. Bu özellikler aslında ve ASP.NET MVC alınan içine taşındı **System.Web** derleme de Web Forms kullanmanız mümkün olmayacaktır.

Bu görevde, model bağlamayla filtre parametresi alma sonuçlarını her kategori için ürünleri miktarına göre filtre uygulamak için GridView güncelleştirir.

1. Geri dönerek **Products.aspx** sayfası.
2. GridView üstüne ekleyin bir **etiket** ve **ComboBox** aşağıda gösterildiği gibi her kategori için ürünleri sayısını seçin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Ekleme bir **EmptyDataTemplate'i** GridView hiçbir kategori ürünleri seçili sayısıyla olduğunda bir ileti göstermek için.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Açık **Products.aspx.cs** arka plan kod ve aşağıdaki ekleme deyimini kullanarak.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Değiştirme **GetCategories** tamsayı almaya yöntemi **minProductsCount** bağımsız değişkeni ve döndürülen sonuçları filtreleyebilirsiniz. Bunu yapmak için yöntem aşağıdaki kodla değiştirin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Yeni **[Denetim]** özniteliği **minProductsCount** bağımsız değişken değeri sayfasında bir denetimi kullanma doldurulmalıdır bilmeniz ASP.NET olanak tanır. ASP.NET (minProductsCount) bağımsız değişkeni adı ile eşleşen herhangi bir denetimi için bakın ve gerekli eşleştirme ve parametre denetimi değeri ile doldurmak için dönüştürme gerçekleştirin.

    Alternatif olarak, öznitelik değerin alınacağı denetiminden belirtmenize olanak tanıyan bir aşırı yüklü Oluşturucu sağlar.

    > [!NOTE]
    > Bir veri bağlama özellikleri sayfasında etkileşim için yazılması gereken kod miktarını azaltmak için hedefidir. [Denetim] değer sağlayıcı dışında yöntemi parametrelerinizi diğer model bağlama sağlayıcılarını kullanabilirsiniz. Bazıları, görev girişte listelenir.
6. Tuşuna **F5** site hata ayıklamayı Başlat ve ürünler sayfasına gidin. Ürünler sayısını aşağı açılan listeden seçin ve GridView şimdi güncelleştirilme dikkat edin.

    ![GridView bir açılır liste değeri ile filtreleme](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView bir açılır liste değeri ile filtreleme")

    *GridView bir açılır liste değeri ile filtreleme*
7. Hata ayıklamayı durdurun.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Görev 4 - kullanarak Model filtreleme için bağlama

Bu görevde, ikinci bir alt Seçilen kategoride ürün göstermek için GridView ekleyeceksiniz.

1. Açık **Products.aspx** sayfasında ve Seç düğmesini otomatik olarak oluşturmak için GridView kategorileri güncelleştirin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. İkinci bir ekleme **GridView** adlı **productsGrid** altındaki. Ayarlama **ItemType** için **WebFormsLab.Model.Product**, **DataKeyNames** için **ProductID** ve **SelectMethod**  için **GetProducts**. Ayarlama **GenerateColumns** için **false** ve ProductID, ProductName, açıklama ve UnitPrice için sütunları ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Açık **Products.aspx.cs** arka plan kod dosyası. Uygulama **GetProducts** GridView kategoriden Kategori Kimliği alma ve ürünleri filtrelemek için yöntem. Model bağlama seçilen satırın kullanarak parametre değeri ayarlayacak **categoriesGrid**. Bağımsız değişken adı ve denetim adı eşleşmiyor olduğundan, Denetim değer sağlayıcısında denetim adını belirtmeniz gerekir.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Bu yaklaşım, birimine kolaylaştırır test bu yöntemleri. Burada Web Forms yürütülmüyor, bir birim testi içeriğine, herhangi bir özel eylem [Denetim] özniteliği gerçekleştirmez.
4. Açık **Products.aspx** sayfasında ve ürünleri GridView bulun. Seçili ürün düzenlemek için bir bağlantı göstermek için GridView ürünleri güncelleştirin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Açık **ProductDetails.aspx** sayfa arka plan kod ve değiştirme **SelectProduct** aşağıdaki kod ile yöntemi.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - SelectProduct yöntemi*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Dikkat **[QueryString]** özniteliği, sorgu dizesi ProductID parametresinde yöntemi parametreyi doldurmak için kullanılır.
6. Tuşuna **F5** site hata ayıklamayı Başlat ve ürünler sayfasına gidin. GridView kategorilerden herhangi bir kategori seçin ve ürünleri GridView güncelleştirilir dikkat edin.

    ![Seçilen kategori ürünlerinden gösteren](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "seçilen kategori ürünlerinden gösterme")

    *Seçilen kategori ürünlerinden gösterme*
7. Tıklatın **Görünüm** ProductDetails.aspx sayfasını açmak için bir ürün bağlantısında.

    Sayfa ürün Sorgu dizesinden ProductID parametresini kullanarak SelectMethod alıyor dikkat edin.

    ![Ürün Ayrıntıları görüntüleme](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "ürün ayrıntılarını görüntüleme")

    *Ürün ayrıntılarını görüntüleme*

    > [!NOTE]
    > Bir HTML açıklama yazın olanağı sonraki alıştırmada uygulanacaktır.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Görev 5 - kullanarak Model güncelleştirme işlemleri için bağlama

Önceki görevde, model bağlama çoğunlukla veri seçmek için kullandığınız, bu görevde, güncelleştirme işlemlerinde model bağlama kullanmayı öğreneceksiniz.

Kategoriler güncelleştirme kullanıcı izin vermek için GridView kategorileri güncelleştirir.

1. Açık **Products.aspx** sayfasında ve Düzenle düğmesini otomatik olarak oluşturmak ve yeni GridView kategorileri güncelleştirme **UpdateMethod** belirtmek için özniteliği bir **UpdateCategory**seçilen öğeyi güncelleştirmek için yöntem.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    GridView DataKeyNames özniteliğinde tanımlayın modeli bağlı nesne benzersizce üyeleri olan ve bu nedenle, hangi güncelleştirme yöntemini en az alması gereken parametrelerdir.
2. Açık **Products.aspx.cs** arka plan kodu dosya ve uygulama **UpdateCategory** yöntemi. Yöntemi, geçerli kategori yüklemek, GridView değerleri doldurmak ve kategori güncelleştirmek için kategori kimliği almanız gerekir.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Yeni **TryUpdateModel** sayfa sınıfının yöntemidir sayfasındaki denetimleri değerleri kullanarak model nesnesi doldurma sorumlu. Bu durumda, içine düzenlenen geçerli GridView satır güncelleştirilmiş değerleri değiştirir **kategori** nesnesi.

    > [!NOTE]
    > Sonraki alıştırmada Nesne düzenlerken kullanıcı tarafından girilen veri doğrulama ModelState.IsValid kullanımını açıklar.
3. Siteyi çalıştırın ve ürünler sayfasına gidin. Bir kategori düzenleyin. Yeni bir ad yazın ve ardından **güncelleştirme** değişiklikleri kalıcı hale getirmek için.

    ![Kategoriler düzenleme](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "kategorileri düzenleme")

    *Kategoriler düzenleme*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Alıştırma 2: Veri doğrulama

Bu alıştırmada, ASP.NET 4.5 içinde yeni veri doğrulama özellikleri hakkında bilgi edineceksiniz. Web Forms yeni örtük doğrulama özelliklerini kontrol eder. Kullanıcı girdisi doğrulama için uygulama modeli sınıflarda veri ek açıklamaları kullanır ve son olarak, istek doğrulama sayfasında tek tek denetimlere açma veya kapatma hakkında bilgi edineceksiniz.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Görev 1 - örtük doğrulama

Doğrulayıcıları dahil olmak üzere karmaşık veri formlarla kodunun yaklaşık % 60 gösterebilir sayfanın içinde çok fazla JavaScript kodu oluşturma eğilimindedir. Etkin örtük doğrulama ile HTML kodunuz temizleyici ve tidier arar.

Bu bölümde, her iki yapılandırmaları tarafından oluşturulan HTML kod karşılaştırmak için ASP.NET örtük doğrulama olanak sağlar.

1. Açık **Visual Studio 2012** ve açın **başlamak** çözüm bulunan **Source\Ex2 Validation\Begin** bu laboratuvarı klasör. Alternatif olarak, önceki alıştırmada varolan çözümünüzden üzerinde çalışmaya devam edebilirsiniz.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bu, Çözüm Gezgini'nde yapmak için **WebFormsLab** proje **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Tuşuna **F5** web uygulamasını başlatmak için. Müşterilere sayfasında ve tıklayın Git **yeni bir müşteri eklemek** bağlantı.
3. Tarayıcı sayfasında sağ tıklatın ve seçin **kaynağı görüntüle** uygulama tarafından oluşturulan HTML kod açmak için seçeneği.

    ![HTML kod sayfasını gösteren](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "HTML kod sayfasını gösteren")

    *HTML kod sayfasını gösteren*
4. Sayfa kaynak kodda kaydırın ve ASP.NET doğrulamaları gerçekleştirmek ve hata listesinin göstermek için JavaScript kodu ve verileri doğrulayıcıları sayfasında eklenmiş dikkat edin.

    ![Doğrulama JavaScript kodu CustomerDetails sayfasındaki](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails sayfa doğrulama JavaScript kodu")

    *CustomerDetails sayfa doğrulama JavaScript kodu*
5. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
6. Şimdi örtük doğrulama olanak sağlar. Açık **Web.Config** ve bulun **ValidationSettings:UnobtrusiveValidationMode** anahtarını **AppSettings** bölüm **.** Anahtar değeri ayarlamak **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Bu özellik ayrıca ayarlamak &quot; **sayfa\_yük** &quot; olay örtük doğrulama yalnızca bazı sayfalar için etkinleştirmek istediğiniz durumda.
7. Açık **CustomerDetails.aspx** ve basın **F5** Web uygulamasını başlatmak için.
8. IE geliştirici araçlarını açmak için F12 tuşuna basın. Geliştirici Araçları açık olduğunda, komut dosyası sekmesini seçin. Seçin **CustomerDetails.aspx** menü ve Al komut dosyalarını jQuery sayfada çalıştırmak için gerekli Not kopyası yüklenmiş yerel siteden tarayıcıda.

    ![JQuery JavaScript yükleme dosyaları doğrudan yerel IIS sunucusundan](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "dosyaları doğrudan yerel IIS sunucusundan jQuery JavaScript yükleniyor")

    *JQuery JavaScript dosyaları doğrudan yerel IIS sunucusundan yükleniyor*
9. Visual Studio'ya dönmek için tarayıcıyı kapatın. Açık **Site.Master** dosyasını yeniden ve bulun **ScriptManager**. Öznitelik Ekle **EnableCdn** değere sahip özelliği **doğru**. Bu çevrimiçi URL'den, yerel sitenin URL'den yüklenmesi jQuery zorlar.
10. Açık **CustomerDetails.aspx** Visual Studio. Siteyi çalıştırmak için F5 tuşuna basın. Internet Explorer açılır sonra geliştirici araçlarını açmak için F12 tuşuna basın. Seçin **betik** sekmesini tıklatın ve sonra aşağı açılan liste göz atın. JQuery JavaScript dosyaları artık yerel siteden, ancak bunun yerine çevrimiçi jQuery CDN yükleniyor unutmayın.

    ![JQuery JavaScript yükleme dosyaları CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "dosyaları CDN jQuery JavaScript yükleniyor")

    *CDN jQuery JavaScript dosyaları yükleniyor*
11. Görünüm kaynak seçeneği tarayıcıda kullanarak yeniden HTML sayfası kaynak kodu açın. Örtük doğrulama sağlayarak ASP.NET eklenen JavaScript kodu verileri - ile yerini aldığını bildirimi \*öznitelikleri.

    ![Örtük doğrulama kodu](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "örtük doğrulama kodu")

    *Örtük doğrulama kodu*

    > [!NOTE]
    > Bu örnekte, nasıl doğrulama ile veri ek açıklamaları özetinin HTML ve JavaScript yalnızca birkaç satır için Basitleştirilmiş gördünüz. Daha önce örtük doğrulama olmadan, eklemenize, daha fazla doğrulama denetimleri arttıkça JavaScript doğrulama kodunuzu büyüyecektir.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Görev 2 - veri ek açıklamaları modeliyle doğrulanıyor

ASP.NET 4.5 Web formları için veri ek açıklamaları doğrulama sunar. Her giriş doğrulama denetime sahip olmak yerine şimdi model sınıflarınızda kısıtlamaları tanımlamak ve tüm web uygulaması arasında kullanın. Bu bölümde, yeni/Düzenle müşteri form doğrulama için veri ek açıklamaları kullanmayı öğreneceksiniz.

1. Açık **CustomerDetail.aspx** sayfası. Müşteri ilk ve ikinci adlandırın dikkat edin **EditItemTemplate** ve **InsertItemTemplate** bölümleri RequiredFieldValidator denetimleri kullanılarak doğrulanır. Her Doğrulayıcı belirli bir koşula ilişkili olduğundan sayıda doğrulayıcıları denetlemek için koşul olarak eklemeniz gerekir.
2. Müşteri model sınıfı doğrulamak için veri ek açıklamaları ekleyin. Açık **Customer.cs** sınıfını **modeli** klasör ve *tasarlamanız* veri ek açıklaması öznitelikleri kullanarak her bir özellik.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex02 - veri ek açıklamaları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 mevcut veri ek açıklama koleksiyonu genişletmiştir. Kullanabileceğiniz veri ek açıklamaları bazıları şunlardır: [CreditCard] [Phone] [EmailAddress] [aralık], [karşılaştırmak], [Url] [FileExtensions], [gerekli] [anahtar], [yanıtta normal ifade].
    > 
    > Bazı kullanım örnekleri:
    > 
    > [anahtar]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Her öznitelik kendi hata iletilerinde de tanımlayabilirsiniz.
3. Açık **CustomerDetails.aspx** ve ilk ve son ad alanları için tüm RequiredFieldvalidators içinde EditItemTemplate ve InsertItemTemplate bölümlerinde FormView denetimini kaldırın.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Doğrulama mantığını uygulama sayfalarınızda yinelenen değil veri ek açıklamaları kullanmanın avantajlarından biri. Modelde bir kez tanımlayın ve verileri işlemek tüm uygulama sayfaları arasında kullanın.
4. Açık **CustomerDetails.aspx** arka plan kod ve Save Customer yöntemini bulun. Bu yöntem yeni bir müşteri eklerken denir ve müşteri parametre FormView denetimi değerleri alır. Sayfa denetimleri ve parametre nesne occurrs arasında eşleme, ASP.NET yürütecek olduğunda tüm veri ek açıklamasını karşı model doğrulama öznitelikleri ve varsa hatalarla karşılaşıldı, ModelState sözlük doldurun.

    ModelState.IsValid yalnızca doğrulama gerçekleştirildikten sonra modeliniz tüm alanlar geçerliyse true döndürür.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Ekleme bir **ValidationSummary** modeli hata listesi göstermek için CustomerDetails sayfa sonunda denetim.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** ValidationSummary yeni bir özellik ayarlandığında denetim olan **doğru**, Denetim ModelState sözlükten hataları gösterir. Bu hatalar veri ek açıklamaları doğrulama dışında gelir.
6. Tuşuna **F5** Web uygulamasını çalıştırmak için. Bazı hatalı değerlerle formu doldurun ve tıklatın **kaydetmek** doğrulama yürütülecek. Alt kısmında Özet hata dikkat edin.

    ![Veri ek açıklamaları ile doğrulama](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "veri ek açıklamaları ile doğrulama")

    *Veri ek açıklamaları ile doğrulama*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Görev 3 - ModelState özel veritabanı hataları işleme

Web Forms önceki sürümünde çok uzun bir dize veya benzersiz bir anahtar ihlali gibi veritabanı hataları işleme, depo kodunuzda özel durumları atma ve ardından, bir hata görüntülemek için arka plan kodu özel durumları işleme ilgili olabilir. Büyük miktarda kod oldukça basit bir şeyler için gereklidir.

Web Forms 4.5 ModelState nesne tutarlı bir şekilde sayfasında, model veya veritabanı hataları görüntülemek için kullanılabilir.

Bu görevde, doğru veritabanı özel durumları işleme ve uygun bir mesaj ModelState nesnesini kullanarak kullanıcı göstermek için kod ekleyeceksiniz.

1. Uygulama çalışmaya devam ederken, yinelenen bir değer kullanarak bir kategori adı güncelleştirmeyi deneyin.

    ![Yinelenen bir ada sahip bir kategori güncelleştirme](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "yinelenen bir ada sahip bir kategori güncelleştiriliyor")

    *Yinelenen bir ada sahip bir kategori güncelleştiriliyor*

    Verilecek bir özel durum bildirimi &quot;benzersiz&quot; kısıtlaması **CategoryName** sütun.

    ![Yinelenen kategori adları için özel durum](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "yinelenen kategori adları için özel durumu")

    *Yinelenen kategori adları için özel durumu*
2. Hata ayıklamayı durdurun. İçinde **Products.aspx.cs** arka plan kod dosyası, güncelleştirme **UpdateCategory** db tarafından oluşturulan özel durumları işlemek için yöntem. SaveChanges() yöntemini çağırın ve bir hata ekler **ModelState** nesnesi.

    Yeni **TryUpdateModel** yöntemi kullanarak kullanıcı tarafından sağlanan form verileri veritabanından alınan kategori nesnesi güncelleştirir.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex02 - UpdateCategory tanıtıcı hataları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > İdeal olarak, DbUpdateException nedenini belirlemeye ve kök nedeni benzersiz bir anahtar kısıtlaması ihlali olup olmadığını denetlemek gerekir.
3. Açık **Products.aspx** ve ekleme bir **ValidationSummary** denetim modeli hata listesi göstermek için GridView kategoriler altında.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Siteyi çalıştırın ve ürünler sayfasına gidin. Yinelenen bir değer kullanarak bir kategori adı güncelleştirmeyi deneyin.

    Özel durumun işlenip ve hata iletisi görünür dikkat edin **ValidationSummary** denetim.

    ![Kategori hata yinelenen](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "kategori hata yineleniyor")

    *Yinelenen kategori hata*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Görev 4 - ASP.NET Web Forms 4.5 doğrulama isteği

ASP.NET istek doğrulama özelliği belirli bir düzeyde varsayılan siteler arası komut dosyası (XSS) saldırılara karşı koruma sağlar. ASP.NET önceki sürümlerinde, istek doğrulama varsayılan olarak etkindir ve bütün bir sayfa için yalnızca devre dışı bırakılamadı. ASP.NET Web Forms yeni sürümü ile artık tek bir denetim için istek doğrulamayı devre dışı bırakmak, yavaş istek doğrulama gerçekleştirebilir veya doğrulanmamış isteği verilere (Bunu yaparsanız dikkatli olun!).

1. Tuşuna **Ctrl + F5** hata ayıklama olmadan siteyi başlatmak ve ürünleri sayfasına gidin. Bir kategori seçin ve ardından **Düzenle** ürünlerden birinin bağlantısında.
2. Tehlikeli olabilecek içeriğe içeren, örneğin HTML etiketlerini dahil olmak üzere bir açıklama yazın. Özel durum nedeniyle isteği doğrulama duyuru alın.

    ![Potansiyel olarak tehlikeli olabilecek içeriğe sahip bir ürün düzenleme](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "potansiyel olarak tehlikeli olabilecek içeriğe sahip bir ürün düzenleme")

    *Potansiyel olarak tehlikeli olabilecek içeriğe sahip bir ürün düzenleme*

    ![Özel durum nedeniyle istek doğrulama](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "özel durum nedeniyle isteği doğrulama")

    *Özel durum nedeniyle isteği doğrulama*
3. Sayfayı kapatın ve Visual Studio'da basın **SHIFT + F5** hata ayıklamasını durdurmak için.
4. Açık **ProductDetails.aspx** sayfasında ve bulun **açıklama** metin kutusu.
5. Yeni Ekle **ValidateRequestMode** TextBox özelliğine ve değerini ayarlama **devre dışı**.

    Yeni **ValidateRequestMode** özniteliği istek doğrulamayı granularly her denetim devre dışı bırakmanıza olanak sağlar. HTML kod almak ancak sayfa geri kalanı için çalışma doğrulama tutmak istediğiniz giriş kullanmak istediğinizde kullanışlıdır.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Tuşuna **F5** web uygulamasını çalıştırmak için. Düzen ürün sayfasını yeniden açın ve HTML etiketlerini dahil olmak üzere bir ürün açıklaması tamamlayın. Açıklamayı HTML içeriği artık ekleyebilirsiniz dikkat edin.

    ![Ürün açıklaması devre dışı doğrulama isteği](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "istek doğrulama için ürün açıklaması devre dışı")

    *İstek doğrulama için ürün açıklaması devre dışı*

    > [!NOTE]
    > Bir üretim uygulamasında yalnızca güvenli HTML etiketleri girilir emin olmak için kullanıcı tarafından girilen HTML kod temizlenmeye (örneğin, var olan hiçbir &lt;betik&gt; etiketleri). Bunu yapmak için kullanabileceğiniz [Microsoft Web koruma Kitaplığı](https://www.nuget.org/packages/AntiXSS).
7. Ürün yeniden düzenleyin. Ad alanında HTML kod yazın ve tıklatın **kaydetmek**. İstek doğrulama için açıklama alanı yalnızca devre dışı bırakıldı ve geri kalan alanları hala potansiyel olarak tehlikeli olabilecek içeriğe karşı doğrulandı dikkat edin.

    ![Alanların geri kalanı etkinleştirilen doğrulama isteği](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "isteği alanların geri kalanı etkinleştirilen doğrulama")

    *Alanların geri kalanı etkin istek doğrulama*

    ASP.NET Web Forms 4.5 gevşek istek doğrulamayı gerçekleştirmek için yeni bir istek doğrulama modu içerir. Ayarlamak istek doğrulama modu ile **4.5**bir kod erişir, *Request.Form [&quot;anahtar&quot;]*, ASP.NET 4.5'ın istek doğrulama yalnızca tetikleyici isteği doğrulama form koleksiyonu belirli bu öğe için.

    Ayrıca, ASP.NET 4.5, artık Microsoft Anti-XSS Kitaplığı v4.0 çekirdek kodlama yordamlarından içerir. Kodlama yordamları yeni tarafından uygulanır Anti-XSS *AntiXssEncoder* yeni bulunan türü **System.Web.Security.AntiXss** ad alanı. İle **encoderType** kullanmak üzere yapılandırılmış parametresi *AntiXssEncoder*, tüm çıktı ASP.NET içinde otomatik olarak kodlaması yeni kodlama yordamları kullanır.
8. ASP.NET 4.5 istek doğrulama isteği verilere doğrulanmamış erişimi de destekler. ASP.NET 4.5 için yeni bir koleksiyon özelliği ekler **HttpRequest** adlı nesne **Unvalidated**. Uygulamasına gidin zaman **HttpRequest.Unvalidated** tüm istek verileri, formlar, QueryStrings, tanımlama bilgileri, URL'ler vb. dahil olmak üzere ortak parçasını erişebilirsiniz.

    ![Request.Unvalidated nesne](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated nesnesi")

    *Request.Unvalidated nesnesi*

    > [!NOTE]
    > **Lütfen HttpRequest.Unvalidated özelliği dikkatli olun!** Tehlikeli metin değil gidiş dönüş ve geri duymayan müşterilere çizilir emin olmak için ham istek verileri dikkatle gerçekleştirdiğiniz özel doğrulama emin olun!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Alıştırma 3: Zaman uyumsuz sayfa işleme ASP.NET Web formları

Bu alıştırmada, ASP.NET Web Forms özelliklerinde işleme yeni zaman uyumsuz sayfaya görülecektir.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Görev 1 - ürün güncelleştirme Ayrıntıları sayfası karşıya yükleme ve görüntüleri göstermek için

Bu görevde, ürün için bir resim URL'si belirtin ve salt okunur görünümde görüntülemek izin vermek için Ürün Ayrıntıları sayfası güncelleştirir. Zaman uyumlu olarak yükleyerek belirtilen görüntü yerel bir kopyasını oluşturur. Sonraki görev zaman uyumsuz olarak çalışması için bu uygulama güncelleştirir.

1. Açık **Visual Studio 2012** ve yük **başlamak** çözüm bulunan **Source\Ex3 Async\Begin** bu Laboratuvar 's klasöründen. Alternatif olarak, var olan bir önceki alıştırmada çözümünüzden üzerinde çalışmaya devam edebilirsiniz.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bu, Çözüm Gezgini'nde yapmak için **WebFormsLab** proje ve seçin **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **ProductDetails.aspx** sayfasında kaynak ve ürün görüntü göstermek için FormView'ın ItemTemplate içinde bir alan ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. FormView'ın EditTemplate resim URL'si belirtmek için bir alan ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Açık **ProductDetails.aspx.cs** arka plan kod dosyasına ve aşağıdaki ad alanı yönergeleri ekleyin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - ad alanları*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Oluşturma bir **UpdateProductImage** yerel uzak görüntüleri saklamak için yöntemi **görüntüleri** klasörü ve güncelleştirme ürün varlığı yeni görüntü konumu değerine sahip.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Güncelleştirme **UpdateProduct** çağrılacak yöntem **UpdateProductImage** yöntemi.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - UpdateProductImage çağrısı*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Uygulamayı çalıştırın ve bir ürün için görüntüyü karşıya yüklemeyi deneyin. Örneğin, Office küçük Arts aşağıdaki görüntü URL'sini kullanabilirsiniz: [ [http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg)

    ![Bir ürün için görüntü ayarlama](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "bir ürün için görüntü ayarlama")

    *Bir ürün için görüntü ayarlama*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Görev 2 - Ürün Ayrıntıları sayfasına işleme zaman uyumsuz ekleme

Bu görevde, zaman uyumsuz olarak çalışması için Ürün Ayrıntıları sayfası güncelleştirir. ASP.NET 4.5 zaman uyumsuz sayfa işleme kullanarak uzun süre çalışan görev - görüntü indirme işlemini - ekleyeceksiniz.

Web uygulamalarında zaman uyumsuz yöntemleri, ASP.NET iş parçacığı havuzları kullanılan şekilde en iyi duruma getirmek için kullanılabilir. ASP.NET var. olan katılımın iş parçacığı havuzu iş parçacıkları, sınırlı sayıda istekleri, bu nedenle, tüm iş parçacıkları meşgul ASP.NET yeni istekleri reddedecek şekilde başlatır, uygulama hata iletileri gönderir ve sitenizi kullanılamaz hale getirir.

Web sitenizde uzun süren işlem bunlar atanan iş parçacığı uzun bir süredir kaplaması nedeniyle, zaman uyumsuz programlama için harika bir aday değildir. Bu, uzun çalışan istek, çok sayıda farklı öğeler sayfaları ve çevrimdışı işlemleri, böyle bir veritabanını sorgulama veya bir dış web sunucusuna erişim gerektiren sayfalar içerir. Avantajı sayfasını işlerken bu işlemler için zaman uyumsuz yöntemleri kullanırsanız, iş parçacığı olması serbest ve iş parçacığına döndürülen sağlamasıdır havuzu ve yeni bir sayfa isteği katılmak için kullanılabilir. Bunun anlamı sayfa iş parçacığı havuzunun bir iş parçacığından işleme başlar ve zaman uyumsuz işlem tamamlandıktan sonra başka bir işlemde tamamlayabilir.

1. Açık **ProductDetails.aspx** sayfası. Ekleme **zaman uyumsuz** özniteliğini **sayfa** öğesi ve ayarlamak **doğru**. Bu öznitelik IHttpAsyncHandler arabirimini uygulaması için ASP.NET söyler.


    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Bir etiket sayfa çalışan iş parçacıklarının ayrıntılarını görüntülemek için sayfanın altındaki ekleyin.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Açık **ProductDetails.aspx.cs** ve aşağıdaki ad alanı yönergeleri ekleyin.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - ad alanları 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Değiştirme **UpdateProductImage** yöntemi zaman uyumsuz bir görevi görüntüsüyle indirin. Yerini alacak **WebClient** **DownloadFile** yöntemiyle **DownloadFileTaskAsync** yöntemi ve dahil **await** anahtar sözcüğü.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - UpdateProductImage zaman uyumsuz*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask farklı bir iş parçacığında yürütülmek üzere yeni bir sayfa zaman uyumsuz görev kaydeder. Lambda ifadesi çalıştırılacak görev (t) ile alır. **Await** anahtar sözcük **DownloadFileTaskAsync** yöntemi dönüştürür yöntemi geri kalan zaman uyumsuz olarak sonra çağrılan bir geri çağırma **DownloadFileTaskAsync** yöntemi tamamlandı. ASP.NET, otomatik olarak tüm HTTP isteği özgün değerler tutarak yönteminin yürütülmesi devam edecek. Yeni zaman uyumsuz programlama modeli .NET 4.5, zaman uyumlu kod gibi çok benzer görünür zaman uyumsuz kod yazma ve geri arama işlevleri veya devamlılık kodu zorluklar işlemek derleyici olanak sağlar.
    > [!NOTE]
    > Zaten RegisterAsyncTask ve PageAsyncTask .NET 2.0 itibaren kullanılabilir. Bekleme anahtar .NET 4.5 zaman uyumsuz programlama modeli yenidir ve .NET WebClient nesnesinden yeni TaskAsync yöntemleri ile birlikte kullanılabilir.
5. Kod başlatıldı ve yürütme tamamlandı iş parçacıklarının görüntülemek için kodu ekleyin. Bunu yapmak için güncelleştirme **UpdateProductImage** aşağıdaki kod ile yöntemi.

    (Kod parçacığını - *Web Forms Laboratuvar - Ex03 - Göster iş parçacığı*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Web sitesinin açmak **Web.config** dosya. Aşağıdaki appSetting değişkeni ekleyin.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Tuşuna **F5** uygulamayı çalıştırın ve ürün için bir görüntüyü karşıya yükleyin. Burada başlama ve bitiş kodu farklı olabilir iş parçacığı kimliği dikkat edin. ASP.NET iş parçacığı havuzu ayrı bir iş parçacığı üzerinde çalışan zaman uyumsuz görevler olmasıdır. Görev tamamlandığında, ASP.NET görev geri sıraya koyar ve herhangi bir kullanılabilir iş parçacıklarının atar.

    ![Görüntüyü zaman uyumsuz olarak indirme](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "görüntüyü zaman uyumsuz indirme")

    *Görüntüyü zaman uyumsuz indirme*

> [!NOTE]
> Ayrıca, Azure aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuar ortamında aşağıdaki kavramlarını ele ve gösterilmektedir:

- Kesin türü belirtilmiş veri bağlama ifadeleri kullanma
- Web Forms yeni model bağlama özelliklerini kullanma
- Arka plan kodu yöntemlere sayfa verileri eşleştirmek için değer sağlayıcıları kullanın
- Kullanıcı girdisi doğrulama için veri ek açıklamaları kullanın
- Web Forms jQuery ile unobstrusive istemci tarafı doğrulama advange alın
- Uygulama ayrıntılı istek doğrulama
- Web Forms işleme zaman uyumsuz sayfası uygulamak

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** . Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Azure SDK'sı Web*&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express Web döşemeye*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek B: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

Bu ekte Azure Portal'dan yeni bir web sitesi oluşturma ve Laboratuvar izleyerek Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Görev 1 - Azure portalından yeni bir Web sitesi oluşturma

1. Git [Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure portalında oturum açtığı](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Windows Azure Portal'da oturum açın")

    *Portal'da oturum açın*
2. Tıklatın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklatın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.

    > [!NOTE]
    > Azure, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için bir ana bilgisayardır. Hızlı oluştur seçeneği Azure portal dışındaki bir tamamlanmış bir web uygulamasını dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun. Yeni Web sitesi çalıştığından emin olun.

    ![Yeni web sitesi için gözatma](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "yeni web sitesi için gözatma")

    *Yeni web sitesi için gözatma*

    ![Çalışan Web sitesi](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.

    > [!NOTE]
    > *Yayımlama profili* Azure her etkin yayımlama yöntemi için bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Azure Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi Yükleniyor](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "yayımlama profili web sitesi yükleniyor")

    *Yayımlama profili Web sitesi yükleniyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada Azure Visual Studio'dan bir web uygulamasına yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.

    ![Yayımlama profili dosyasını kaydetme](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "yayımlama profilini kaydetme")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.

1. Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir. Aboneliğiniz Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme. Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi. Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucusu Pano](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**. Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) düğmesi.

    ![İstemci IP adresi ekleme](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylamak](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Değişiklikleri onaylamak*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.

    ![Uygulama yayımlama](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklatın **bağlantısı doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.

    ![Bağlantı doğrulama](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "bağlantısı doğrulanıyor")

    *Doğrulama bağlantısı*
4. İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

    - İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.
    - İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.
    - İçinde **parola** sunucu yönetici oturum açma parolasını yazın.
    - Yeni bir veritabanı adı yazın.

    ![Hedef bağlantı dizesi yapılandırma](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "hedef bağlantı dizesi yapılandırma")

    *Hedef bağlantı dizesi yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturuluyor*
7. Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanına işaret eden bağlantı dizesi](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanına işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında, **Yayımla**.

    ![Web uygulaması yayımlama](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.

1. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
2. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*
