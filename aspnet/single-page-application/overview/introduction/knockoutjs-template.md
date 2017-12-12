---
uid: single-page-application/overview/introduction/knockoutjs-template
title: "Tek sayfa uygulaması: Çakıştırmaları şablonu | Microsoft Docs"
author: MikeWasson
description: "Boşaltılan şablonu"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 6e84dcc16345e33fcd3a3f83c4b35bc993c03ca6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="single-page-application-knockoutjs-template"></a>Tek sayfa uygulaması: Çakıştırmaları şablonu
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Boşaltılan MVC şablonu ASP.NET ve Web Araçları 2012.2 parçasıdır
> 
> [ASP.NET ve Web Araçları 2012.2 indirme](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET ve Web Araçları 2012.2 güncelleştirme ASP.NET MVC 4 için bir tek sayfalı uygulama (SPA) şablonu içerir. Bu şablon, hızlı bir şekilde etkileşimli istemci tarafı web uygulamaları oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır.

"Tek sayfalı uygulama" (SPA) olan tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yüklenirken yerine güncelleştiren bir web uygulaması için genel bir terim. İlk sayfa yükleme, AJAX istekleri aracılığıyla sunucusuyla SPA alınmaktadır.

![](knockoutjs-template/_static/image1.png)

AJAX yeni bir şey değildir, ancak bugün oluşturmasına ve büyük ve karmaşık bir SPA uygulama korumasına kolaylaştırmak JavaScript çerçeveleri vardır. Ayrıca, HTML 5 ve CSS3 zengin Uı'lar oluşturmak kolaylaştırarak.

Başlamanıza yardımcı olmak için örnek bir "Yapılacaklar listesi" uygulama SPA şablon oluşturur. Bu öğreticide, biz Kılavuzlu Tur şablonun sürer. İlk biz Yapılacaklar listesi uygulaması, kendisini arayın ve sonra çalışması teknolojisi parçaları inceleyin.

## <a name="create-a-new-spa-template-project"></a>Yeni bir SPA şablonu projesi oluşturma

Gereksinimleri:

- Visual Studio 2012 veya Visual Studio Express 2012 için Web
- ASP.NET Web Araçları 2012.2 güncelleştirin. Güncelleştirmeyi yüklemeniz [burada](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Visual Studio'yu başlatın ve seçin **yeni proje** başlangıç sayfasından. Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#**seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Proje adı ve'ı tıklatın **Tamam**.

![](knockoutjs-template/_static/image2.png)

İçinde **yeni proje** seçin **tek sayfa uygulaması**.

![](knockoutjs-template/_static/image3.png)

Derleme ve uygulamayı çalıştırmak için F5 tuşuna basın. Uygulamayı ilk kez çalıştırdığında, oturum açma ekranı görüntüler.

![](knockoutjs-template/_static/image4.png)

Tıklatın &quot;kaydolun&quot; bağlayın ve yeni bir kullanıcı oluşturun.

![](knockoutjs-template/_static/image5.png)

Oturum açtıktan sonra uygulama ile iki öğe varsayılan bir Yapılacaklar listesi oluşturur. "Yapılacaklar Listesi Ekle" tıklatabilirsiniz yeni bir liste eklemek için.

![](knockoutjs-template/_static/image6.png)

Listeyi yeniden adlandırma, öğeleri listeye ekleyin ve onay kutusunu temizleyin. Ayrıca, öğeleri silin veya listesinin tümünü silebilirsiniz. Değişiklikler otomatik olarak sunucudaki bir veritabanına kalıcı (gerçekten LocalDB bu noktada, uygulamayı yerel olarak çalıştırdığınız için).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>SPA şablonu mimarisi

Bu diyagramda, uygulama için temel yapı taşlarını gösterilmektedir.

![](knockoutjs-template/_static/image8.png)

Sunucu tarafında ASP.NET MVC HTML hizmet verir ve ayrıca form tabanlı kimlik doğrulaması gerçekleştirir.

ASP.NET Web API ToDoLists ve Todoıtems, alma, oluşturma, güncelleştirme ve silme de dahil olmak üzere ilgili tüm istekleri işler. İstemci Web API ile JSON biçiminde veri değiş tokuş eder.

Entity Framework (EF) O/RM katmanıdır. ASP.NET nesne yönelimli dünyasına ve temel veritabanı arasında aracılık. Yerel veritabanı veritabanı kullanır ancak bu Web.config dosyasında değiştirebilirsiniz. Genellikle, LocalDB yerel geliştirme için kullanabilir ve sonra sunucuda, bir SQL veritabanına EF kodu ilk geçişi kullanarak dağıtabilirsiniz.

İstemci tarafında Knockout.js kitaplığı Sayfa güncelleştirmelerini AJAX istekleri işler. Boşaltılan veri bağlama sayfanın en son veriler ile eşitlenmesi için kullanır. Böylece, herhangi bir JSON verilerine anlatılmaktadır ve DOM güncelleştiren kodu yazmak zorunda değilsiniz Bunun yerine, verileri sunmak nasıl Boşaltılan anlatın HTML'de bildirim temelli öznitelikleri yerleştirin.

Bu mimarinin büyük bir avantajı, sunu katmanı uygulama mantığından ayıran olmasıdır. Web sayfanızın nasıl görüneceğine hakkında hiçbir şey bilmeden Web API bölümü oluşturabilirsiniz. İstemci tarafında için "Görünüm modeli" oluşturun, verileri temsil eder ve HTML olarak bağlamak için Boşaltılan görünüm modeli kullanır. Bu görünüm modeli değiştirmeden HTML kolayca değiştirmenize olanak verir. (Biz altını biraz daha sonra bakacağız.)

## <a name="models"></a>Modeller

Visual Studio projesini sunucu tarafında kullanılan modelleri modeller klasörü içerir. (İstemci tarafında da modelleri vardır; Biz bu elde edersiniz.)

![](knockoutjs-template/_static/image9.png)

**Todoıtem, yapılacaklar listesi**

Entity Framework Code First veritabanı modellerini bunlar. Bu modeller birbirlerine noktası özelliklerine sahip dikkat edin. `ToDoList`Todoıtems ve her bir koleksiyonunu içeren `ToDoItem` üst ToDoList geri bir başvuru içeriyor. Bu özellikleri Gezinti özellikleri adı verilir ve bunlar bir-çok ilişkisi Yapılacaklar listesi ve onun Yapılacaklar öğelerini temsil eder.

`ToDoItem` Sınıfı ayrıca kullanır **[ForeignKey]** belirtmek için öznitelik `ToDoListId` bir yabancı anahtar içine `ToDoList` tablo. Bir yabancı anahtar kısıtlaması veritabanına eklemek için EF söyler.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Bu sınıfların istemciye gönderilen veri tanımlayın. "İçin veri aktarım nesnesini." "DTO" anlamına gelir DTO nasıl varlıkları JSON'a seri hale getirilir tanımlar. Genel olarak, DTOs kullanmak için birkaç nedeni vardır:

- Denetlemek için hangi özelliklerin serileştirilir. DTO bir alt etki alanı modeli özelliklerinden birini içerebilir. Güvenlik nedenleriyle (hassas verilerini gizlemek) ya da yalnızca bunu yapabilirsiniz gönderdiğiniz veri miktarını azaltmak için.
- Örneğin veri – şeklini değiştirmek için düzleştirilecek daha karmaşık bir veri yapısıdır.
- İş mantığı (sorunları ayrılması) DTO dışında tutulacak.
- Herhangi bir nedenden dolayı etki alanı Modellerinizi seri hale getirilemez Var olan bir nesneyi serileştirme, örneğin, döngüsel başvurulara Web API'deki bu sorunu ele yolları sorunlara yol açabilir (bkz [işleme döngüsel nesne başvuruları](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ancak bir DTO kullanarak yalnızca geçmez sorunu tamamen.

SPA şablonunda DTOs etki alanı modelleri aynı verileri içerir. Ancak, bunlar döngüsel başvuru Gezinti Özellikleri'nden önlemek ve genel DTO düzeni göstermek için hala faydalıdır.

**AccountModels.cs**

Bu dosya, site üyeliği modellerinde içerir. `UserProfile` Sınıfı üyelik DB kullanıcı profilleri için şema tanımlar. (Bu durumda, bilgiler yalnızca kullanıcı kimliği ve kullanıcı adıdır.) Bu dosyadaki diğer modeli sınıfları, kullanıcı kayıt ve oturum açma formları oluşturmak için kullanılır.

## <a name="entity-framework"></a>Varlık Çerçevesi

SPA şablon EF Code First kullanır. Code First geliştirme modeller kodda ilk tanımlamak ve ardından veritabanını oluşturmak için model EF kullanır. Var olan bir veritabanı EF de kullanabilirsiniz ([veritabanı ilk](https://msdn.microsoft.com/en-us/data/jj206878.aspx)).

`TodoItemContext` Modeller klasörü sınıfında türer **DbContext**. Bu sınıf EF ve modelleri arasında "Yapıştır" sağlar. `TodoItemContext` Tutan bir `ToDoItem` koleksiyonu ve bir `TodoList` koleksiyonu. Veritabanını sorgulamak için yalnızca bu koleksiyonlara yönelik bir LINQ Sorgu yazma. Örneğin, işte, tüm kullanıcı "Alice" Yapılacaklar listesi nasıl seçebilirsiniz:

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Ayrıca koleksiyona yeni öğeler eklemek, öğeleri, güncelleştirebilir veya koleksiyondan öğeleri silmek ve değişiklikler veritabanına kalır.

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web API denetleyicisi

ASP.NET Web API denetleyicilerinin, HTTP isteklerini işler nesneleridir. Belirtildiği gibi SPA şablonu CRUD işlemleri etkinleştirmek için Web API'sini kullanır `ToDoList` ve `ToDoItem` örnekleri. Denetleyicileri çözümü denetleyicileri klasöründe bulunur.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Yapılacaklar öğelerini için HTTP isteklerini işler
- `TodoListController`: Yapılacaklar listesi için HTTP isteklerini işler.

Web API Denetleyici adı URI yolu eşleştiğinden bu adları önemlidir. (Web API HTTP isteklerini denetleyicilerine nasıl yönlendirir öğrenmek için bkz: [ASP.NET Web API'de yönlendirme](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Bakalım `ToDoListController` sınıfı. Bir tek veri üyesi içerir:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` Daha önce açıklandığı gibi EF ile iletişim kurmak için kullanılır. CRUD işlemleri denetleyicisinde yöntemleri uygulayın. Web API denetleyicisi yöntemleri, istemciden gelen HTTP isteklerini şu şekilde eşlenir:

| HTTP isteği | Denetleyici yöntemi | Açıklama |
| --- | --- | --- |
| /Api/TODO Al | `GetTodoLists` | Yapılacaklar listesi koleksiyonunu alır. |
| GET/api/todo/*kimliği* | `GetTodoList` | Kimliğe göre bir Yapılacaklar listesi alır |
| PUT/api/todo/*kimliği* | `PutTodoList` | Yapılacaklar listesini güncelleştirir. |
| / Api/todo sonrası | `PostTodoList` | Yeni bir Yapılacaklar listesi oluşturur. |
| DELETE/api/todo/*kimliği* | `DeleteTodoList` | Yapılacaklar listesi siler. |

URI'ler bazı işlemler için kimliği değeri için yer tutucuları içeren dikkat edin. Örneğin, bir-listeye 42 kimliği, URI'nin silmektir `/api/todo/42`.

CRUD işlemleri için Web API kullanma hakkında daha fazla bilgi edinmek için [CRUD işlemleri destekler bir Web API oluşturma](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Bu denetleyici için kodu oldukça basittir. İlginç bazı noktalar şunlardır:

- `GetTodoLists` Yöntemi oturum açma kullanıcı Kimliğine göre sonuçlarını filtrelemek için bir LINQ sorgusu kullanır. Bu şekilde, bir kullanıcı yalnızca o verip müdürün ait olan verileri görür. Ayrıca, bir Select deyimi dönüştürmek için kullanılmıştır `ToDoList` içine örnekleri `TodoListDto` örnekleri.
- PUT ve POST yöntemleri, veritabanı değiştirmeden önce model durumunu kontrol edin. Varsa **ModelState.IsValid** false, bu yöntemleri HTTP 400 Hatalı istek döndürür. Web API model doğrulama hakkında daha fazla bilgiyi [Model doğrulama](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Denetleyici sınıfı ayrıca ile donatılmış **[Authorize]** özniteliği. Bu öznitelik, HTTP isteği kimliği doğrulanmış olup olmadığını denetler. İstemci istek kimliği doğrulanmazsa, HTTP 401 alır yetkisiz. Kimlik doğrulama hakkında daha fazla bilgiyi [kimlik doğrulama ve yetkilendirme ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

`TodoController` Sınıftır çok benzer `TodoListController`. İstemci her Yapılacaklar listesi birlikte Yapılacaklar öğelerini alır çünkü büyük herhangi bir GET yöntem tanımlamıyor farktır.

## <a name="mvc-controllers-and-views"></a>MVC denetleyicileri ve görünümler

MVC denetleyicileri da çözüm denetleyicileri klasöründe bulunur. `HomeController`uygulama için ana HTML işler. Giriş denetleyicisi görünümünü Views/Home/Index.cshtml tanımlanır. Giriş görünümü, kullanıcının oturum açtığı bağlı olarak farklı içerik oluşturur:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Kullanıcılar oturum açtığında ana UI görürler. Aksi takdirde, oturum açma paneli bakın. Bu koşullu işleme sunucu tarafında olacağını unutmayın. İstemci tarafında hassas içeriği gizlemek asla denemezler & bir HTTP yanıt olarak gönderdiğiniz #8212anything ham HTTP iletileri izliyor birine görülebilir.

## <a name="client-side-javascript-and-knockoutjs"></a>İstemci tarafı JavaScript ve Knockout.js

Şimdi istemci uygulamaya sunucu tarafındaki dönelim. SPA şablonu bir kesintisiz, etkileşimli kullanıcı Arabirimi oluşturmak için jQuery ve Knockout.js bir bileşimini kullanır. Knockout.js HTML veri bağlamak kolaylaştıran bir JavaScript kitaplıktır. Knockout.js "Model-View-ViewModel." adı verilen bir modeli kullanır.

- Etki alanı verileri (Yapılacaklar listesi ve yapılacaklar öğelerini) modelidir.
- HTML belgesi görünümüdür.
- Görünüm modeli model verileri tutan bir JavaScript nesnesidir. Görünüm modeli UI kod soyutlamadır. HTML temsilini olanağıyla sahiptir. Bunun yerine, "Yapılacaklar öğelerini listesi" gibi görünümün soyut özellikleri temsil eder.

Görünüm veri görünüm modeline bağlı. Görünüm modeli güncelleştirmeleri otomatik olarak görünümünde yansıtılır. Bağlama başka bir yöne de çalışır. DOM (tıklar gibi) olayları verilere işlevlere görünüm model üzerinde AJAX çağrıları tetiklemek bağlı.

İstemci tarafı JavaScript SPA şablon üç katmanlara düzenler:

- TODO.DataContext.js: AJAX isteği gönderir.
- TODO.model.js: modelleri tanımlar.
- TODO.ViewModel.js: görünüm modeli tanımlar.

![](knockoutjs-template/_static/image11.png)

Bu komut dosyaları çözüm betikleri/uygulama klasöründe bulunur.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** Web API denetleyicilerinin tüm AJAX çağrıları işler. (Oturum açma için AJAX çağrıları başka bir yerde ajaxlogin.js tanımlanır.)

**TODO.model.js** Yapılacaklar listelerinde için istemci tarafı (tarayıcı) modelleri tanımlar. İki model sınıfları vardır: Todoıtem ve todoList.

Model sınıfları özelliklerin çoğunu "ko.observable" türüdür. Gözlemlenenler nasıl Boşaltılan kendi Sihirli mu ' dir. Gelen [Boşaltılan belgelerine](http://knockoutjs.com/documentation/introduction.html): observable "aboneleri değişiklikler hakkında bilgilendirebilir bir JavaScript." nesnedir Observable değeri değiştiğinde Boşaltılan bu gözlemlenenler ilişkili HTML öğelerinin güncelleştirir. Örneğin, Todoıtem gözlemlenenler başlık ve isDone özellikleri vardır:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Observable kodda için abone olabilirsiniz. Örneğin, "isDone" ve "title" özelliklerindeki değişiklikler Todoıtem sınıfı kaydeder:

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Görünüm modeli**

Görünüm modeli todo.viewmodel.js içinde tanımlanmıştır. Burada uygulama etki alanı veri HTML sayfası öğeleri bağlar orta noktası görünüm modelidir. SPA şablonunda görünüm modeli todoLists observable dizisi içerir. Görünüm modeli aşağıdaki kodda bağlamaları uygulamak için çakıştırma bildirir:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML ve veri bağlama

Ana HTML sayfası için Views/Home/Index.cshtml içinde tanımlanmıştır. Veri bağlama de kullandığınızdan, HTML ne gerçekte işlendiğini için yalnızca bir şablondur. Boşaltılan kullanan *bildirim temelli* bağlar. Sayfa öğelerini öğesine "data-bind" özniteliği ekleyerek verilere bağlayın. Boşaltılan belgelerinden geçen çok basit bir örnek aşağıda verilmiştir:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Bu örnekte, çakıştırma içeriğini güncelleştirir  **&lt;span&gt;**  değerini bir öğesiyle `myItems.count()`. Bu değer her değiştiğinde, çakıştırma belge güncelleştirir.

Boşaltılan farklı bağlama türleri sayısını sağlar. SPA şablonda kullanılan bağlamaları bazıları şunlardır:

- **foreach**: üzerinden bir döngü yinelemek ve listedeki her öğeye aynı biçimlendirme uygulamak olanak tanır. Bu yapılacaklar listesi ve yapılacaklar öğelerini işlemek için kullanılır. İçinde **foreach**, bağlamaları liste öğelerine uygulanır.
- **görünür**: Görünürlüğü değiştirmek için kullanılır. Bir koleksiyonu boş olduğunda biçimlendirme gizleyin veya hata iletisi görünür yapın.
- **değer**: form değerleri doldurmak için kullanılır.
- **tıklatın**: click olayını işlevi görünüm modeli bağlar.

## <a name="anti-csrf-protection"></a>Anti-CSRF koruması

Siteler arası istek sahteciliği (CSRF), burada kötü amaçlı bir site olduğu kullanıcı şu anda oturum savunmasız bir siteye bir istek gönderir bir saldırı aracıdır. CSRF saldırılarını önlemeye yardımcı olmak için ASP.NET MVC kullanan *sahteciliğe karşı koruma belirteçleri*, doğrulama belirteçleri isteği olarak da bilinir. Sunucu bir web sayfasına rastgele oluşturulmuş bir belirteç koyar olur. İstemci sunucuya veri gönderdiğinde, bu değer istek iletisinde içermesi gerekir.

Kötü amaçlı sayfası kaynak aynı ilkeleri nedeniyle kullanıcının belirteçleri okunamıyor çünkü sahteciliğe karşı koruma belirteçleri çalışır. (Kaynak aynı ilkeler birbirlerinin içeriğe erişmesini iki farklı sitelerde barındırılan belgelere önledi.)

ASP.NET MVC aracılığıyla sahteciliğe karşı koruma belirteçleri için yerleşik destek sağlar [AntiForgery](https://msdn.microsoft.com/en-us/library/system.web.helpers.antiforgery.aspx) sınıfı ve [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute.aspx) özniteliği. Şu anda bu işlevselliği Web API yerleşik olmayan. Ancak, SPA şablon özel bir uygulama için Web API içerir. Bu kod tanımlanan `ValidateHttpAntiForgeryTokenAttribute` çözümü filtreleri klasöründe bulunan sınıfı. Anti-CSRF Web API'sinde hakkında daha fazla bilgi için bkz: [önleme siteler arası istek sahteciliği (CSRF) saldırılarını](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Sonuç

SPA şablon hızla modern, etkileşimli web uygulamaları yazma başlamanıza yardımcı olmak için tasarlanmıştır. Knockout.js kitaplığı veri ve uygulama mantığı (HTML biçimlendirmesi) sunudan ayırmak için kullanır. Ancak Boşaltılan bir SPA oluşturmak için kullanabileceğiniz yalnızca JavaScript kitaplığı değil. Diğer bir seçenek keşfetmek istiyorsanız, bir göz atalım [topluluk oluşturulan SPA şablonlar](../templates/index.md).
