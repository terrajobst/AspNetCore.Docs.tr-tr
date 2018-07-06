---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Tek sayfalı uygulama: KnockoutJS şablonu | Microsoft Docs'
author: MikeWasson
description: Knockout şablonu
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 9e48c7033cefe8b6e4c456577b7a6448f9815d0b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832535"
---
<a name="single-page-application-knockoutjs-template"></a>Tek sayfalı uygulama: KnockoutJS şablonu
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Knockout MVC şablonu ASP.NET ve Web Araçları 2012.2 parçasıdır
> 
> [ASP.NET ve Web Araçları 2012.2 indirin](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET ve Web Araçları 2012.2 güncelleştirme, ASP.NET MVC 4 için bir tek sayfalı uygulama (SPA) şablonu içerir. Bu şablon, etkileşimli istemci tarafı web uygulamaları hızlı bir şekilde oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır.

"Tek sayfalı uygulama" (SPA) olduğundan tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yükleniyor yerine güncelleştiren bir web uygulaması için genel bir terimdir. Başlangıç sayfası yükleme sonrası AJAX istekleri aracılığıyla sunucusuyla SPA anlatıyor.

![](knockoutjs-template/_static/image1.png)

AJAX yeni bir şey değildir, ancak bugün oluşturun ve büyük ve karmaşık bir SPA uygulama sürdürmek kolaylaştıran yeni JavaScript çerçevesi vardır. Ayrıca, HTML 5 ve CSS3 zengin kullanıcı arabirimleri oluşturmak kolaylaşır.

Başlamanıza yardımcı olmak için örnek bir "Yapılacaklar listesi" uygulama SPA şablon oluşturur. Bu öğreticide, biz şablonu Kılavuzlu tura katılın. İlk biz Yapılacaklar listesi uygulaması kendisini arayın ve ardından bu hizmetin çalışmasını sağlayan teknolojiyi parçaları inceleyin.

## <a name="create-a-new-spa-template-project"></a>Yeni bir SPA şablonu projesi oluşturma

Gereksinimler:

- Visual Studio 2012 veya Visual Studio Web için Express 2012
- ASP.NET Web Araçları 2012.2 güncelleştirin. Güncelleştirmeyi yüklemeniz [burada](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Visual Studio'yu başlatın ve seçin **yeni proje** başlangıç sayfasından. Veya **dosya** menüsünde **yeni** ardından **proje**.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Projeyi adlandırın ve tıklayın **Tamam**.

![](knockoutjs-template/_static/image2.png)

İçinde **yeni proje** seçin **tek sayfalı uygulama**.

![](knockoutjs-template/_static/image3.png)

Derleme ve uygulamayı çalıştırmak için F5 tuşuna basın. Uygulamayı ilk kez çalıştırdığında, oturum açma ekranını görüntüler.

![](knockoutjs-template/_static/image4.png)

Tıklayın &quot;kaydolun&quot; bağlayın ve yeni bir kullanıcı oluşturun.

![](knockoutjs-template/_static/image5.png)

Oturum açtıktan sonra uygulama ile iki öğe varsayılan Yapılacaklar listesi oluşturur. "Yapılacaklar Listesi Ekle" tıklayarak yeni bir liste eklemek için.

![](knockoutjs-template/_static/image6.png)

Listeyi yeniden adlandırma, listeye öğe eklemek ve onay kutusunu temizleyin. Öğeleri silin veya listesinin tamamını silin. Değişiklikleri otomatik olarak sunucuda bir veritabanına kalıcı (aslında LocalDB bu noktada, uygulamayı yerel olarak çalıştığından).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>SPA şablonu mimarisi

Bu diyagram, uygulama için temel yapı taşlarını gösterir.

![](knockoutjs-template/_static/image8.png)

Sunucu tarafında, ASP.NET MVC HTML işlevi görür ve form tabanlı kimlik doğrulaması da işler.

ASP.NET Web API ToDoLists ve Todoıtems, alma, oluşturma, güncelleştirme ve silme de dahil olmak üzere ilgili tüm istekleri işler. İstemci Web API'si ile verileri JSON biçiminde birbiriyle değiştirir.

Entity Framework (EF), O/RM katmanıdır. Bu, ASP.NET nesne odaklı dünyası ve temel alınan veritabanı arasında aracılık. LocalDB veritabanını kullanır ancak bu Web.config dosyasında değiştirebilirsiniz. Genellikle, LocalDB yerel geliştirme için kullanın ve ardından bir SQL veritabanı sunucusunda, EF code first geçiş kullanarak dağıtın.

İstemci tarafında Knockout.js kitaplığı Sayfa güncelleştirmelerini AJAX istekleri işler. Knockout veri bağlamayı sayfanın en son verileri eşitlemek için kullanır. Böylece, herhangi bir JSON verilerini aracılığıyla size yol gösterir ve DOM güncelleştiren kod yazmanız gerekmez Bunun yerine, Boşaltılan veri sunma hakkında bilgi HTML dosyasındaki bildirim temelli öznitelikleri yerleştirin.

Bu mimarinin büyük bir avantajı, kendisini sunu katmanı uygulama mantığından ayıran olmasıdır. Web API bölümü, web sayfanızın nasıl görüneceğini ilgili hiçbir şeyi bilmeden oluşturabilirsiniz. İstemci tarafında için "Görünüm modeli" oluşturma verileri temsil eder ve HTML olarak bağlamak için Knockout görünüm modeli kullanır. Bu görünüm modeli değiştirmeden HTML kolayca değiştirmenize olanak tanır. (Knockout biraz daha göz atacağız.)

## <a name="models"></a>Modeller

Visual Studio projesinde, sunucu tarafında kullanılan modelleri modeller klasörü içerir. (İstemci tarafında da modeli vardır; biz onlara alırsınız.)

![](knockoutjs-template/_static/image9.png)

**Todoıtem, yapılacaklar listesi**

Veritabanı için Entity Framework Code First modelleri şunlardır. Bu modeller birbirine noktası özellikleri olduğunu fark edeceksiniz. `ToDoList` Todoıtems ve her bir koleksiyonunu içeren `ToDoItem` ToDoList üst geri bir başvuru içeriyor. Bu özellik Gezinti özellikleri olarak adlandırılır ve bire çok ilişkisi bir Yapılacaklar listesi ve kendi Yapılacaklar öğelerini gösterir.

`ToDoItem` Sınıfı da kullandığı **[ForeignKey]** belirtmek için öznitelik `ToDoListId` içine bir yabancı anahtar `ToDoList` tablo. Bu, bir FOREIGN key kısıtlaması veritabanına eklemek için EF bildirir.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Bu sınıfların istemciye gönderilecek verileri tanımlar. "DTO" "için veri aktarımı nesnesi." anlamına gelir. DTO nasıl varlıkları JSON'a seri hale tanımlar. Genel olarak, Dto'lar kullanmak için birkaç nedeni vardır:

- Denetlemek için hangi özelliklerin serileştirilir. Bir etki alanı modelinden özellik alt kümesi DTO içerir. Güvenlik nedenleriyle (hassas verileri gizlemek) ya da yalnızca bunu yapabilirsiniz, gönderdiğiniz veri miktarını azaltmak için.
- Örneğin veri – şeklini değiştirmek için daha karmaşık bir veri yapı düzleştirmektir.
- İş mantığı DTO (ayrımı nettir) dışında tutulacak.
- Herhangi bir nedenden dolayı etki alanı Modellerinizi seri hale getirilemez Var olan bir nesne seri olduğunda, döngüsel başvurular Web API'sindeki bu sorunu gidermek için yol sorunlara yol açabilir (bkz [işleme döngüsel nesne başvuruları](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ancak bir DTO kullanarak yalnızca önler sorunu tamamen.

SPA şablonunda etki alanı modellerini aynı verileri Dto içerir. Ancak, bunlar bunlar Gezinti özelliklerinden döngüsel başvurulardan kaçının ve bunlar genel DTO Düzen göstermek için hala faydalıdır.

**AccountModels.cs**

Bu dosya, site üyeliği modellerini içerir. `UserProfile` Sınıfı üyelik DB kullanıcı profilleri için şema tanımlar. (Bu durumda, yalnızca kullanıcı kimliği ve kullanıcı adıdır.) Bu dosyadaki diğer model sınıfları, kullanıcı kayıt ve oturum açma formlar oluşturmak için kullanılır.

## <a name="entity-framework"></a>Varlık Çerçevesi

SPA şablon EF Code First kullanır. Code First geliştirme modelleri kodda önce tanımlayın ve ardından veritabanını oluşturmak için model EF kullanır. Var olan bir veritabanı EF de kullanabilirsiniz ([veritabanı ilk](https://msdn.microsoft.com/data/jj206878.aspx)).

`TodoItemContext` Modeller klasörü sınıfında türetilir **DbContext**. Bu sınıf, model ve EF arasında "Yapıştırıcı" sağlar. `TodoItemContext` Tutan bir `ToDoItem` koleksiyonu ve `TodoList` koleksiyonu. Veritabanını sorgulamak için yalnızca bu koleksiyonlara yönelik bir LINQ Sorgu yazın. Örneğin, işte, tüm yapılacaklar listesi "Gamze adlı" kullanıcı için nasıl seçebilirsiniz:

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Ayrıca koleksiyona yeni öğeler eklemek, öğeleri güncelleştirme veya öğeleri koleksiyondan silebilir ve veritabanına değişiklikleri kalıcı hale getirmek.

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web API denetleyicisi

ASP.NET Web API'de HTTP isteklerini işleyen nesneleri denetleyicileridir. SPA şablon Web API belirtildiği gibi CRUD işlemleri etkinleştirmek için kullanır. `ToDoList` ve `ToDoItem` örnekleri. Denetleyicileri çözüm denetleyicileri klasöründe bulunur.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Yapılacaklar öğeleri için HTTP isteklerini işler
- `TodoListController`: Yapılacak işler listelerinin için HTTP isteklerini işler.

Web API denetleyici adının URI yolu ile eşleştiği için bu adlar önemlidir. (Web API HTTP isteklerini denetleyicilerine nasıl yönlendirdiğini bilgi edinmek için [ASP.NET Web API'de yönlendirme](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Bakalım `ToDoListController` sınıfı. Bir tek veri üyesi aşağıdakileri içerir:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` Daha önce açıklandığı gibi EF ile iletişim kurmak için kullanılır. Denetleyici üzerinde yöntemleri CRUD işlemleri uygular. Web API denetleyici yöntemlerinde, istemciden gelen HTTP isteklerini şu şekilde eşlenir:

| HTTP isteği | Denetleyici yöntemi | Açıklama |
| --- | --- | --- |
| /Api/TODO Al | `GetTodoLists` | Yapılacak işler listelerinin bir koleksiyonunu alır. |
| GET/API'sitodo/*kimliği* | `GetTodoList` | Kimliğe göre bir Yapılacaklar listesi alır |
| PUT/API'si/todo/*kimliği* | `PutTodoList` | Yapılacaklar listesini güncelleştirir. |
| Todo/api/gönderin | `PostTodoList` | Yeni bir Yapılacaklar listesi oluşturur. |
| SİLME/API'sitodo/*kimliği* | `DeleteTodoList` | Bir Yapılacaklar listesi siler. |

Bazı işlemleri için bir URI'leri kimlik değerini yer tutucuları içeren dikkat edin. Örneğin, bir-listeye 42 kimliği silmek için URI değil `/api/todo/42`.

CRUD işlemleri için Web API kullanma hakkında daha fazla bilgi edinmek için [bu destekler CRUD işlemleri bir Web API'si oluşturma](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Bu denetleyici için kod oldukça basittir. Bazı ilginç noktaları şunlardır:

- `GetTodoLists` Yöntemi bir LINQ Sorgu sonuçlarını oturum açmış kullanıcı Kimliğine göre filtrelemek için kullanır. Bu şekilde, bir kullanıcı yalnızca gitmesini ait verileri görür. Ayrıca, bir Select deyimi dönüştürmek için kullanılır fark `ToDoList` içine örnekler `TodoListDto` örnekleri.
- PUT ve POST yöntemleri, veritabanı değiştirmeden önce model durumunu kontrol edin. Varsa **ModelState.IsValid** yanlış, bu yöntemler, HTTP 400 Hatalı istek döndürür. Sırasında Web API'de model doğrulama hakkında daha fazla bilgiyi [Model doğrulama](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Denetleyici sınıfı da ile donatılmış **[Authorize]** özniteliği. Bu öznitelik, HTTP isteği kimlik doğrulaması olup olmadığını denetler. İstek Kimliği doğrulanmazsa, HTTP 401, istemci aldıktan yetkisiz. Kimlik doğrulama hakkında daha fazla bilgiyi [kimlik doğrulama ve yetkilendirme ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

`TodoController` Sınıftır çok benzer `TodoListController`. En büyük fark herhangi bir GET yöntemi tanımlamaz istemci her bir Yapılacaklar listesi ile birlikte Yapılacaklar öğelerini alırsınız olmasıdır.

## <a name="mvc-controllers-and-views"></a>MVC denetleyicileri ve görünümleri

MVC denetleyicileri da çözümün denetleyicileri klasöründe bulunur. `HomeController` uygulama için ana HTML işler. Giriş denetleyicisine görünümünü Views/Home/Index.cshtml içinde tanımlanır. Giriş görünümü, kullanıcının oturum açtığı bağlı olarak farklı içerik işler:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Kullanıcılar oturum açtığında, ana UI görürler. Aksi halde, oturum açma paneli bakın. Bu koşullu işleme sunucu tarafında olacağını unutmayın. İstemci tarafında hassas içerik gizlemek hiçbir zaman deneyin ve bir HTTP yanıtında gönderdiğiniz #8212anything ham HTTP iletileri izliyor birine görülebilir.

## <a name="client-side-javascript-and-knockoutjs"></a>İstemci tarafı JavaScript ve Knockout.js

Artık istemci uygulamaya sunucu tarafındaki dönelim. SPA şablon, jQuery ve Knockout.js kesintisiz etkileşimli bir kullanıcı Arabirimi oluşturmak için kullanır. Knockout.js HTML veri bağlama kolaylaştıran bir JavaScript kitaplıktır. Knockout.js "Model-View-ViewModel." adlı bir desen kullanır.

- Etki alanı verileri (ToDo listeler ve ToDo öğeleri) modelidir.
- HTML belgesi görünümüdür.
- Görünüm modeli model verileri tutan bir JavaScript nesnesidir. Görünüm modeli, kullanıcı arabiriminin bir kod soyutlamadır. HTML gösteriminin bilgisi var. Bunun yerine, Özet görünümü "Yapılacaklar listesi" gibi özelliklerini temsil eder.

Görünüm veri görünüm modeline bağlı. Görünüm modeli güncelleştirmeler Görünümü'nde otomatik olarak yansıtılır. Bağlamalar ve diğer yöndeki de çalışır. Olayları DOM (tıkladığında gibi) verilere işlevleri için Görünüm modeli üzerinde AJAX çağrıları tetiklemek, bağlı.

SPA şablon istemci tarafı JavaScript üç katmanlara göre düzenler:

- TODO.DataContext.js: AJAX isteği gönderir.
- TODO.model.js: modelleri tanımlar.
- TODO.ViewModel.js: görünüm modeli tanımlar.

![](knockoutjs-template/_static/image11.png)

Bu komut dosyaları, çözümün betikleri/uygulama klasöründe bulunur.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** Web APİ'si denetleyicilerinin tüm AJAX çağrıları işler. (Oturum açma için AJAX çağrıları başka bir yerde ajaxlogin.js içinde tanımlanır.)

**TODO.model.js** yapılacak işler listelerinin için istemci tarafı (tarayıcı) modelleri tanımlar. İki model sınıfı vardır: Todoıtem ve yapılacaklar listesi.

Model sınıfları özelliklerin birçoğu "ko.observable" türüdür. Gözlemlenenler Knockout kendi Sihirli nasıl yaptığını ' dir. Gelen [Knockout belgeleri](http://knockoutjs.com/documentation/introduction.html): observable "aboneleri değişiklikleri bildiren bir JavaScript nesne" dir. Observable değerini değiştirdiğinde bu gözlemlenenler için ilişkili HTML öğelerinin Knockout güncelleştirir. Örneğin, Todoıtem gözlemlenenler başlık ve isDone özelliklerine sahiptir:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Observable kod için abone olabilirsiniz. Örneğin, "isDone" ve "title" özelliklerindeki değişiklikler Todoıtem sınıfı kaydeder:

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Görünüm modeli**

Görünüm modeli todo.viewmodel.js içinde tanımlanır. Görünüm modeli, burada da HTML sayfası öğeleri uygulama etki alanı verilere bağlar merkezi noktasıdır. SPA şablonunda todoLists gözlemlenebilir bir dizi görünüm modeli içerir. Görünüm modeli aşağıdaki kodda bağlamaları uygulamak için Knockout bildirir:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML ve veri bağlama

Ana HTML sayfası için Views/Home/Index.cshtml içinde tanımlanır. Veri bağlama de kullandığınızdan, HTML ne gerçekten işlenen için yalnızca bir şablonudur. Knockout kullanan *bildirim temelli* bağlar. "Data-bind" özniteliği öğeye ekleyerek sayfa öğeleri verilere bağlayın. Knockout belgelerinden geçen çok basit bir örneği aşağıda verilmiştir:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Bu örnekte, Knockout içeriğini güncelleştirir **&lt;span&gt;** değerini öğeyle `myItems.count()`. Bu değer değiştiğinde Knockout belgeyi güncelleştirir.

Knockout birkaç farklı bağlama türleri sağlar. SPA şablonunda kullanılan bağlamaları bazıları şunlardır:

- **foreach**: bir döngü yinelemek ve listedeki her öğeye aynı biçimlendirme uygulamak olanak sağlar. Bu yapılacaklar listesi ve yapılacaklar öğelerini işlemek için kullanılır. İçinde **foreach**, bağlamaları liste öğelerine uygulanır.
- **görünür**: Görünürlüğü değiştirmek için kullanılır. Bir koleksiyon boş olduğunda biçimlendirme gizleyebilir veya hata iletisi görünür yapın.
- **değer**: form değerleri doldurmak için kullanılır.
- **tıklayın**: bir tıklama olayı bir işlev bir görünüm modeli bağlar.

## <a name="anti-csrf-protection"></a>Anti-CSRF koruması

Siteler arası istek sahteciliği (CSRF), burada bir kötü niyetli site burada kullanıcı şu anda oturum savunmasız sitesine bir istek gönderir bir saldırıdır. CSRF saldırılarını önlemeye yardımcı olmak için ASP.NET MVC kullanan *sahteciliğe karşı koruma belirteçleri*, istek doğrulama belirteçleri olarak da bilinir. Sunucu web sayfasına rastgele oluşturulmuş bir belirteç geçirir olur. İstemcinin sunucuya veri gönderdiğinde, bu değer istek iletisinde içermelidir.

Sahteciliğe karşı koruma belirteçleri çalışır, çünkü kötü amaçlı sayfası aynı çıkış noktası ilkeleri nedeniyle kullanıcının belirteçleri okunamıyor. (Aynı çıkış noktası ilkeler birbirlerinin içeriğe erişmesini iki farklı sitelerde barındırılan belge önler.)

ASP.NET MVC ile sahteciliğe karşı koruma belirteçleri için yerleşik destek sağlar [AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) sınıfı ve [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) özniteliği. Şu anda bu işlevselliği'den sonra Web API oluşturulmadı. Ancak SPA şablonu, Web API'si için özel bir uygulamasını içerir. Bu kod tanımlanan `ValidateHttpAntiForgeryTokenAttribute` çözümün filtreleri klasöründe bulunan sınıfı. Anti-CSRF Web API hakkında daha fazla bilgi için bkz: [önleme siteler arası istek sahteciliği (CSRF) saldırılarını](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Sonuç

SPA şablonu, hızlı bir şekilde modern, etkileşimli web uygulamaları yazmaya başlamanıza yardımcı olmak üzere tasarlanmıştır. Knockout.js kitaplığı, veri ve uygulama mantığı (HTML biçimlendirmeyi) sunudan ayırmak için kullanır. Ancak, Knockout bir SPA oluşturmak için kullanabileceğiniz tek bir JavaScript kitaplığı değil. Diğer bir seçenek keşfetmek istiyorsanız, bir göz atın [topluluk tarafından oluşturulan SPA şablonlarını](../templates/index.md).
