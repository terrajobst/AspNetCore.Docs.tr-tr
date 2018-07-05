---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: ASP.NET AJAX Web hizmetlerini anlama | Microsoft Docs
author: scottcate
description: Web Hizmetleri, dağıtılmış sistemler arasında veri değişimi için platformlar arası bir çözüm sağlayan .NET framework'ün ayrılmaz bir parçasıdır. Ancak Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: c3874e19ef55ffd4949f9bc280d32a3da7a00271
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376391"
---
<a name="understanding-aspnet-ajax-web-services"></a>ASP.NET AJAX Web hizmetlerini anlama
====================
tarafından [Scott Cate](https://github.com/scottcate)

[PDF'yi indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Web Hizmetleri, dağıtılmış sistemler arasında veri değişimi için platformlar arası bir çözüm sağlayan .NET framework'ün ayrılmaz bir parçasıdır. Web Hizmetleri, normalde farklı işletim sistemleri, nesne modelleri ve veri göndermek ve almak için programlama dilleri izin vermek için kullanılsa dinamik olarak bir ASP.NET AJAX sayfasına veri ekleme ya da veri sayfadan bir arka uç sistemine göndermek için de kullanılabilir. Tüm bu işlemleri geri gönderme başvurmadan yapılabilir.


## <a name="calling-web-services-with-aspnet-ajax"></a>ASP.NET AJAX ile arama Web Hizmetleri

Dan Wahlin

Web Hizmetleri, dağıtılmış sistemler arasında veri değişimi için platformlar arası bir çözüm sağlayan .NET framework'ün ayrılmaz bir parçasıdır. Web Hizmetleri, normalde farklı işletim sistemleri, nesne modelleri ve veri göndermek ve almak için programlama dilleri izin vermek için kullanılsa dinamik olarak bir ASP.NET AJAX sayfasına veri ekleme ya da veri sayfadan bir arka uç sistemine göndermek için de kullanılabilir. Tüm bu işlemleri geri gönderme başvurmadan yapılabilir.

ASP.NET AJAX UpdatePanel denetimine AJAX için basit bir yol sağlarken herhangi bir ASP.NET sayfasında etkinleştirmek için dinamik olarak UpdatePanel kullanmadan sunucusundaki veri erişim gerektiğinde zamanlar olabilir. Bu makalede, bu oluşturarak ve sayfalar arasında ASP.NET AJAX Web hizmetlerini kullanma yerine getirmeyi görürsünüz.

Bu makalede, ASP.NET AJAX Araç Seti AutoCompleteExtender adlı bir Web hizmeti etkinleştirildi denetiminde yanı sıra çekirdek ASP.NET AJAX uzantılarını kullanılabilen işlevler üzerinde odaklanır. Kapsanan konular şunlardır: AJAX etkinleştirilmiş Web hizmetlerini tanımlama, istemci proxy'leri oluşturma ve JavaScript ile Web hizmetlerini çağırma. Ayrıca, nasıl ASP.NET sayfası yöntemlerini doğrudan Web hizmeti çağrıları yapılabilir görürsünüz.

## <a name="web-services-configuration"></a>Web Hizmetleri Yapılandırması

Visual Studio 2008 ile yeni bir Web sitesi projesi oluşturulduğunda, Visual Studio'nun önceki sürümlerini kullanıcılara tanıdık yeni eklemeler bir dizi web.config dosyasına sahiptir. Diğer gerekli HttpHandlers ve HttpModules tanımlama sırasında sayfalarında kullanılabilmesi için bu değişikliklerden bazıları "asp" ön eki için ASP.NET AJAX denetimleri eşleyin. 1 kod, yapılan değişiklikleri gösterir `<httpHandlers>` Web hizmeti çağrılarını etkiler web.config öğesinde. HttpHandler .asmx çağrıları işlemek için kullanılan varsayılan kaldırılır ve System.Web.Extensions.dll derlemesinde bulunan ScriptHandlerFactory sınıfı ile değiştirilmiştir. System.Web.Extensions.dll tüm ASP.NET AJAX tarafından kullanılan çekirdek işlevselliğini içerir.

**1 listesi. ASP.NET AJAX Web hizmeti işleyici yapılandırması**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Bu HttpHandler değiştirme, JavaScript Web hizmeti proxy kullanarak JavaScript nesne gösterimi (JSON) çağrıları .NET Web Hizmetleri için ASP.NET AJAX sayfalarından yapılmasına izin vermek üzere kurulur. ASP.NET AJAX, genellikle Web hizmetlerinizle ilişkili standart Basit Nesne Erişim Protokolü (SOAP) çağrısı aksine, Web Hizmetleri için JSON iletileri gönderir. Bu, daha küçük bir istek ve yanıt iletilerinde genel sonuçlanır. ASP.NET AJAX JavaScript kitaplığı JSON nesneleriyle çalışmaya iyileştirilmiş olduğundan veri daha verimli istemci tarafı işleme için de sağlar. 2 ve 3 listeleme show örnekler JSON biçiminde seri hale getirilmiş Web hizmetine istek ve yanıt iletilerinin listesi. Bir müşteri nesneleri dizisi ve ilişkili özellikleri listeleme 3 yanıt iletisinde geçirir listeleme 2'de gösterilen istek iletisi "Belçika" değerine sahip bir ülke parametresi geçirir.

**2 listesi. JSON olarak serileştirilmiş web hizmeti istek iletisi**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] işlem adı web hizmetinin URL'sini bir parçası olarak tanımlanır; Ayrıca, istek iletilerinin her zaman JSON gönderilen değil. Web Hizmetleri aracılığıyla geçirilecek parametreler neden true olarak ayarlanmış UseHttpGet parametresiyle ScriptMethod öznitelik kullanabiliyor bir sorgu dizesi parametreleri.*


**3 listesi. JSON olarak serileştirilmiş web hizmeti yanıt iletisi**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Sonraki bölümde Web hizmetleri hem basit hem de karmaşık türleri ile yanıt ve JSON istek iletilerini işleme yeteneğine oluşturulacağını öğreneceksiniz.

## <a name="creating-ajax-enabled-web-services"></a>AJAX içerebilen Web hizmetleri oluşturma

ASP.NET AJAX framework Web hizmetlerini çağırmak için çeşitli yollar sunar. AutoCompleteExtender denetimi (ASP.NET AJAX Araç Seti kullanılabilir) veya JavaScript kullanabilirsiniz. Böylece istemci-komut dosyası kod tarafından çağrılabilir ancak, bir hizmeti çağırmadan önce AJAX-etkin olması.

ASP.NET Web Hizmetleri için yeni olsun veya olmasın, onu oluşturmak basit ve AJAX Etkinleştirme Hizmetleri bulabilirsiniz. ASP.NET Web Hizmetleri oluşturulmasını 2002'de, ilk sürümünden bu yana desteklenen .NET framework ve ASP.NET AJAX uzantılarını derlemeler .NET framework'ün varsayılan özellikler kümesi üzerinde ek AJAX işlevselliği sağlar. Visual Studio .NET 2008 Beta 2 .asmx Web hizmeti dosyaları oluşturmak için yerleşik destek içerir ve otomatik olarak ilişkili kod sınıfları yanında System.Web.Services.WebService'in sınıfından türetilir. Sınıfına yöntemler gibi Web hizmeti tüketiciler tarafından çağrılacak edebilmeleri WebMethod özniteliği uygulamanız gerekir.

4 listeleme WebMethod özniteliği GetCustomersByCountry() adlı bir yöntem uygulayarak bir örnek gösterilmektedir.

**4 listesi. WebMethod özniteliği kullanarak bir Web hizmeti**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

GetCustomersByCountry() yöntemi, bir ülke parametre kabul eder ve müşteri nesne dizisi döndürür. Metodun Metoda geçilen ülke değeri veritabanından veri almak için bir veri katmanı sınıfı müşteri nesne özellikleri ile veri doldurun ve bir dizi döndürür çağrıları buna karşılık, iş katmanı sınıfına iletilir.

## <a name="using-the-scriptservice-attribute"></a>ScriptService özniteliğini kullanma

WebMethod özniteliği sağlayan standart SOAP iletilerini göndermek için Web hizmeti istemcileri tarafından çağrılacak GetCustomersByCountry() yöntemi eklenirken, JSON çağrıları hazır ASP.NET AJAX uygulamalardan yapılmasına izin vermez. ASP.NET AJAX uzantının uygulamak sahip yapılacak JSON çağrılarına izin vermek için `ScriptService` özniteliği için Web hizmeti sınıfı. Bu, biçimlendirilmiş JSON kullanarak yanıt iletilerini göndermek bir Web hizmetini etkinleştirir ve JSON iletiler göndererek bir hizmeti çağırmak amacıyla istemci tarafı komut dosyası sağlar.

5 ScriptService özniteliği CustomersService adlı bir Web hizmeti sınıfı için uygulama örneği gösterir.

**5 listesi. AJAX özellikli bir Web hizmeti için ScriptService özniteliğini kullanma**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

AJAX komut dosyası koddan çağrılabilir gösteren bir işaret ScriptService öznitelik görür. Bu aslında arka planda gerçekleşen JSON serileştirme veya seri durumundan çıkarma görevlerden herhangi birini işlemiyor. (Web.config dosyasında yapılandırılmış) ScriptHandlerFactory ve diğer ilgili sınıflar toplu JSON işlem yapabilirsiniz.

## <a name="using-the-scriptmethod-attribute"></a>ScriptMethod özniteliğini kullanma

Bir .NET Web hizmetindeki ASP.NET AJAX sayfaları tarafından kullanılacak sırayla tanımlanacak olan tek ASP.NET AJAX özniteliği ScriptService özniteliğidir. Ancak, ScriptMethod adlı başka bir özniteliği doğrudan Hizmet Web yöntemleri için de uygulanabilir. ScriptMethod tanımlar dahil olmak üzere üç özellik `UseHttpGet`, `ResponseFormat` ve `XmlSerializeString`. Bu özelliklerin değerlerini değiştirerek bir Web hizmeti tarafından kabul edilen bir istek türünü gereken yere GET, Web yöntemi biçiminde ham XML verileri döndürmek gerektiğinde değiştirilmesi durumlarda yararlı olabilir bir `XmlDocument` veya `XmlElement` nesne veya veri döndürüldüğü ne zaman bir  hizmeti her zaman JSON yerine XML olarak serileştirilmiş.

Web yöntemi kabul etmelidir özelliği kullanılabilir UseHttpGet POST istekleri aksine istekleri alın. QueryString parametreleri için bir URL kullanarak Web yöntemi giriş parametreleriyle dönüştürülen gönderdiği istekleri. Özelliği varsayılan olarak false olarak ve yalnızca UseHttpGet ayarlanması `true` zaman operations bilinir güvenli ve ne zaman hassas verileri bir Web hizmetine geçirilir. 6 listeleme UseHttpGet özelliğiyle ScriptMethod özniteliğini kullanarak bir örnek gösterilmektedir.

**6 listesi. ScriptMethod öznitelik UseHttpGet özelliği ile kullanarak.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

HttpGetEcho Web listeleme 6'da gösterilen yöntemi çağrıldığında gönderilen üst bilgiler örneği gösterilir sonraki:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

XML yanıtlar bir hizmet JSON yerine döndürülecek gerektiğinde Web HTTP GET isteklerini kabul etmek yöntemleri yayımlamanızı ScriptMethod özniteliği de kullanılabilir. Örneğin, bir Web hizmeti uzak bir siteden bir RSS akışı almak ve XmlDocument veya XmlElement bir nesne olarak döndürür. Veri XML işlemeye istemcide ardından oluşabilir.

7 listeleme XML verileri Web yöntemden döndürülmesi gerektiğini belirtmek için ResponseFormat özelliğini kullanarak bir örnek gösterilmektedir.

**7 listesi. ScriptMethod öznitelik ResponseFormat özelliği ile kullanarak.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

ResponseFormat özelliği de XmlSerializeString özelliği ile birlikte kullanılabilir. Tüm Web yönteminden döndürülen dizelerin dışındaki türleri XML olarak serileştirilme döndürecek anlamına gelir. false varsayılan değerini XmlSerializeString özelliğine sahip olduğunda `ResponseFormat` özelliği `ResponseFormat.Xml`. Zaman `XmlSerializeString` ayarlanır `true`, Web yöntemi tarafından döndürülen tüm türleri, dize türleri dahil olmak üzere XML olarak serileştirilme şeklini. ResponseFormat özellik değeri varsa `ResponseFormat.Json` XmlSerializeString özelliği göz ardı edilir.

8 listeleme XML olarak serileştirilmiş için dizeleri zorlamak için XmlSerializeString özelliğini kullanarak bir örnek gösterilmektedir.

**8 listesi. ScriptMethod öznitelik XmlSerializeString özelliği ile kullanarak**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Listeleme 8'de gösterilen GetXmlString Web yöntemini çağırma döndürülen değer, sonraki gösterilir:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Varsayılan JSON biçimindeki istek ve yanıt iletilerinin toplam boyutunu en aza indirir ve tarayıcılar arası bir biçimde ASP.NET AJAX istemciler tarafından tüketilen daha kolay olsa da, ResponseFormat ve XmlSerializeString özelliklerini olabilir durumlarda kullanılan istemci uygulamalar gibi Internet Explorer 5 veya üzeri Web yöntemden döndürüleceği XML verileri bekler.

## <a name="working-with-complex-types"></a>Karmaşık türleri ile çalışma

5 listeleyen bir Web hizmetinden müşteri adlı bir karmaşık türü döndüren bir örnek gösterilmiştir. Müşteri sınıfı birkaç farklı basit türler, FirstName ve LastName gibi özellikleri olarak dahili olarak tanımlar. Giriş parametresi olarak kullanılan karmaşık türleri veya dönüş türü bir AJAX içerebilen Web yöntemini de otomatik olarak serileştirilir JSON'a istemci-tarafı gönderilmeden önce. Ancak, iç içe geçmiş karmaşık türler (Bu dahili olarak başka bir türde tanımlanan) istemciye tek başına nesneler olarak varsayılan olarak kullanılabilir duruma getirilmez.

Burada bir Web hizmeti tarafından kullanılan iç içe geçmiş bir karmaşık türü de bir istemci sayfasında kullanılmalıdır durumlarda, ASP.NET AJAX GenerateScriptType öznitelik Web hizmetine eklenebilir. Örneğin, adresi ve cinsiyet özellikleri listeleme 9'da gösterilen CustomerDetails sınıfı içeren hangi ***karmaşık iç içe geçmiş türleri temsil eder.***

**9 listesi. Burada gösterilen CustomerDetails sınıfı iki iç içe geçmiş karmaşık türleri içerir.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

İç içe geçmiş türler olduğundan listeleme 9'da gösterilen CustomerDetails sınıfı içinde tanımlanan adres ve cinsiyet nesneleri otomatik olarak istemci tarafı JavaScript aracılığıyla kullanıma yapılır olmaz (adres bir sınıf, cinsiyet ise bir sabit listesi). Burada bir Web hizmeti içinde kullanılan iç içe bir tür üzerinde istemci tarafı kullanılabilir olmalıdır durumlarda, daha önce bahsedilen GenerateScriptType özniteliği kullanılabilir (10 listeleme bakın). Bu öznitelik, farklı iç içe geçmiş karmaşık türler hizmetten döndürülen burada durumlarda birden çok kez eklenebilir. Doğrudan Web hizmeti sınıfı veya belirli Web yöntemleri üzerinde uygulanabilir.

**10 listesi. İstemcinin kullanılabilir olması gerektiğini iç içe geçmiş türleri tanımlamak için GenerateScriptService özniteliğini kullanarak.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Uygulayarak `GenerateScriptType` özniteliği Web hizmeti, adresi ve cinsiyet türlerine otomatik olarak oluşturulacak kullanılabilmesi için istemci tarafı ASP.NET AJAX JavaScript koduna göre. Listeleme 11'de otomatik olarak oluşturulan ve bir Web hizmeti GenerateScriptType öznitelik ekleyerek istemciye gönderilen JavaScript örneği gösterilmektedir. Karmaşık iç içe geçmiş türler kullanma makalenin sonraki bölümlerinde görürsünüz.

**11 listesi. İç içe geçmiş karmaşık türler bir ASP.NET AJAX sayfasına kullanılabilir.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Web hizmetleri oluşturma ve ASP.NET AJAX sayfalarına erişebilmesini gördüğünüze göre oluşturma ve JavaScript proxy'leri kullanabilir, böylece veri alınan ya da Web hizmetlerine gönderilen bir göz atalım.

## <a name="creating-javascript-proxies"></a>JavaScript proxy'leri oluşturma

SOAP isteği ve yanıt iletileri gönderme karmaşıklıkları ayrıntılarından korur bir proxy nesnesi oluşturma (.NET veya başka bir platform) standart bir Web hizmeti genellikle çağırma içerir. ASP.NET AJAX Web hizmeti çağrıları ile JavaScript proxy'leri oluşturulur ve kolayca serileştirme ve JSON iletilerini seri kaldırma hakkında endişelenmeden hizmetlerini çağırmak için kullanılır. ASP.NET AJAX ScriptManager denetimini kullanarak JavaScript proxy'leri otomatik olarak oluşturulabilir.

Web Hizmetleri çağırabilen bir JavaScript proxy'si oluşturuluyor ScriptManager'ın Services özelliği kullanılarak gerçekleştirilir. Bu özellik, bir ASP.NET AJAX sayfasına gönderin veya geri gönderme işlemleri gerek kalmadan veri almak için zaman uyumsuz olarak çağırabilirsiniz bir veya daha fazla hizmet tanımlamanızı sağlar. ASP.NET AJAX'ı kullanarak bir hizmet tanımlama `ServiceReference` denetimi ve denetimin Web hizmeti URL'si atama `Path` özelliği. 12 listeleme CustomersService.asmx adlı hizmet başvuran bir örnek gösterilmektedir.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**12 listesi. Bir ASP.NET AJAX sayfa içinde kullanılan bir Web hizmeti tanımlama.**

Bir ScriptManager denetimi aracılığıyla CustomersService.asmx başvuru eklemeyi dinamik olarak oluşturulan ve sayfa tarafından başvurulan bir JavaScript proxy'si neden olur. Proxy kullanarak katıştırılmış &lt;betik&gt; CustomersService.asmx dosya çağırarak ve /js, sonuna ekleme dinamik olarak yüklendi ve etiketleyin. Aşağıdaki örnek, hata ayıklama web.config dosyasında devre dışı bırakıldığında JavaScript proxy'si sayfaya nasıl eklenmiştir gösterir:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Oluşturulan gerçek JavaScript proxy kodu görmek istiyorsanız, Internet Explorer'ın adres kutusuna istediğiniz .NET Web hizmetinin URL'sini yazın ve /js bunu sona ekleyin.*


JavaScript proxy'si hata ayıklama sürümü sayfasındaki katıştırılmış web.config dosyasında hata ayıklama etkinse, sonraki adımda gösterilen:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

ScriptManager tarafından oluşturulan JavaScript proxy'si doğrudan sayfasına eklenebilir yerine kullanılarak başvurulan &lt;betik&gt; etiketin src özniteliği. (Varsayılan: false) true olarak ServiceReference denetimin Inlinescript özelliği ayarlanarak bu yapılabilir. Bir ara sunucu ve ağ çağrıları sunucuya yapılan sayısını azaltmak istediğiniz zaman birden çok sayfada paylaşılmayan bu yararlı olabilir. False varsayılan değerini proxy'nin birden çok sayfada bir ASP.NET AJAX uygulama tarafından kullanıldığı durumlarda önerilir Inlinescript proxy betiği true olarak ayarlandığında tarayıcı tarafından önbelleğe olmaz. Inlinescript özelliğini kullanarak bir örnek, sonraki gösterilir:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>JavaScript proxy'leri kullanma

ScriptManager denetimini kullanarak bir ASP.NET AJAX sayfa tarafından başvurulan bir Web hizmeti sonra Web hizmetine çağrı yapılabilmesi için ve döndürülen veriler, geri çağırma işlevleri kullanılarak işlenebilir. Bir Web hizmeti (varsa), ad alanı başvuruyor, sınıf adını ve Web yöntemi adı tarafından çağrılır. Döndürülen verileri işleyen bir geri çağırma işlevini yanı sıra Web hizmetine geçirilen parametrelerin tanımlanabilir.

Listeleme 13'te GetCustomersByCountry() adlı Web yöntemini çağırmak için bir JavaScript proxy'si kullanarak bir örnek gösterilmektedir. Son Kullanıcı sayfasında bir düğmeyi tıkladığında GetCustomersByCountry() işlev çağrılır.

**13 listesi. Bir Web hizmeti bir JavaScript proxy'si ile çağrılıyor.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Bu çağrı InterfaceTraining ad alanı başvuruyor, CustomersService sınıfı ve GetCustomersByCountry Web yöntemini hizmette tanımlanan. Bu, zaman uyumsuz Web hizmeti çağrısı döndürüldüğünde, çağrılması gereken OnWSRequestComplete adlı bir geri çağırma işlevini yanı sıra TextBox'dan alınan bir ülke değer geçirir. OnWSRequestComplete hizmetten döndürülen müşteri nesneler dizisi işler ve bunları sayfada gösterilen bir tabloya dönüştürür. Şekil 1'de çağrısından oluşturulan çıktı gösterilmektedir.


[![Web hizmeti bir zaman uyumsuz AJAX çağrısı yaparak elde edilen verilerle bağlama.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Şekil 1**: veri bağlama elde ettiğiniz bir Web hizmeti için bir zaman uyumsuz AJAX çağrısı yaparak.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript proxy'leri, Web Hizmetleri için tek yönlü çağrıları burada Web yöntemi çağrılmalıdır ancak proxy yanıt beklemesi gerekmez durumlarda de yapabilirsiniz. Örneğin, bir iş akışı gibi bir işlem başlatma, ancak bir dönüş değeri hizmetinden bekleyin değil bir Web hizmeti çağırmak isteyebilirsiniz. Tek yönlü bir çağrı bir hizmet için yapılması gereken yere durumlarda listeleme 13'te gösterilen geri çağırma işlevine sadece atlanabilir. Geri arama işlevi tanımlamış verileri döndürmek Web hizmeti proxy nesnesi beklemez.

## <a name="handling-errors"></a>Hataları işleme

Zaman uyumsuz geri çağırmalar için Web Hizmetleri gibi Web Hizmeti'nin veya verilen bir özel durum, ağ hataları farklı türde karşılaşırsınız. Neyse ki, JavaScript proxy nesneleri ScriptManager tarafından oluşturulan hatalar ve daha önce gösterilen başarılı geri arama yanı sıra hataları işlemek için tanımlanmış birden çok geri çağırmaları izin verin. Bir hata geri çağırma işlevini listeleme 14'te gösterildiği gibi Web yöntemi çağrısında standart geri çağırma işlevi hemen sonra tanımlanabilir.

**14 listesi. Bir hata geri çağırma işlevi tanımlama ve hataları görüntüleme.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Web hizmeti çağrıldığında oluşan hataların bir parametre olarak bir hatayı temsil eden bir nesne kabul eden çağrılacak OnWSRequestFailed() geri çağırma işlevi tetikler. Hata nesnesi yanı sıra olup olmadığını çağrısı zaman aşımına uğradı hatanın nedenini belirlemek için birkaç farklı işlevler sunar. 14 listeleme farklı hata işlevleri kullanarak bir örnek gösterilmektedir ve Şekil 2'işlevleri tarafından oluşturulan çıktı örneği gösterilmektedir.


[![ASP.NET AJAX hata işlevleri çağrılarak oluşturulan çıktı.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Şekil 2**: ASP.NET AJAX hata işlevleri çağrılarak oluşturulan çıktı.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Bir Web hizmetinden döndürülen XML verilerini işleme

Daha önce ResponseFormat özelliğinin yanı sıra ScriptMethod özniteliğini kullanarak bir Web yöntemine ham XML verileri nasıl döndürebilir gördünüz. ResponseFormat ResponseFormat.Xml için ayarlandığında, Web hizmetinden döndürülen veriler JSON yerine XML olarak serileştirilmiş. XML verileri işlemek için JavaScript veya XSLT kullanarak doğrudan istemciye geçirilecek gerektiğinde bu yararlı olabilir. Şu anda Internet Explorer 5 ya da daha yüksek ayrıştırma ve MSXML için yerleşik destek nedeniyle XML verileri filtreleme için en iyi istemci tarafı nesne modeli sağlar.

Bir Web hizmetinden XML verilerini alma, diğer veri türleri almaktan farklı değildir. Uygun işlevi çağırmak ve bir geri çağırma işlevini tanımlamak için JavaScript proxy'si çağırarak başlatın. Çağrısı döndüğünde sonra geri çağırma işlevi verileri işleyebilir.

15 listeleme Web XmlElement nesneyi döndürür GetRssFeed() adlı bir metot çağrısının bir örnek gösterilmektedir. GetRssFeed() RSS almak için akışı için URL'yi temsil eden tek bir parametre kabul eder.

**15 listesi. Bir Web hizmetinden döndürülen XML verileri ile çalışma.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Bu örnek, bir RSS akışına bir URL'sini geçirir ve OnWSRequestComplete() işlevinde döndürülen XML verileri işler. OnWSRequestComplete() ilk tarayıcı Internet Explorer'ı MSXML ayrıştırıcının kullanılabilir olup olmadığını bilmek olup olmadığını denetler. İse, bir XPath ifadesi tüm bulmak için kullanılan &lt;öğesi&gt; etiketlerin RSS akışı. Her bir öğe daha sonra üzerinden yinelenir ve ilişkili &lt;başlık&gt; ve &lt;bağlantı&gt; etiketleri bulunur ve her öğenin verileri görüntülemek için işlenen. Şekil 3, bir ASP.NET AJAX GetRssFeed() Web yöntemine yapılan bir JavaScript proxy'si aracılığıyla çağrı yapmasını oluşturulan çıktının bir örneği gösterilmektedir.

## <a name="handling-complex-types"></a>Karmaşık türler işleme

Kabul edildi veya bir Web hizmeti tarafından döndürülen karmaşık türler, otomatik olarak bir JavaScript proxy'si sunulur. Karmaşık iç içe geçmiş türler ancak, istemci tarafında doğrudan erişilebilir değildir, bu sürece GenerateScriptType öznitelik hizmete daha önce bahsedildiği gibi uygulanır. Neden karmaşık iç içe geçmiş bir tür üzerinde istemci tarafı kullanmak istiyorsunuz?

Bu soruyu cevaplamak için bir ASP.NET AJAX sayfasına müşteri verileri görüntüler ve son müşterinin adresini güncelleştirmek kullanıcılara varsayılır. Web hizmeti adres türünü (CustomerDetails sınıfı içinde tanımlanan bir karmaşık tür) istemciye gönderilen olduğunu belirtiyorsa sonra güncelleştirme işlemini daha iyi kod yeniden kullanımı için işlevleri ayırın bölünebilir.


[![Çıkış çağırma RSS veri döndüren bir Web hizmeti oluşturma.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Şekil 3**: çıkış çağırma RSS veri döndüren bir Web hizmeti oluşturma.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-web-services/_static/image9.png))


16 modeli ad alanında tanımlanan bir adresi nesne çağıran istemci tarafı koduna ilişkin bir örnek güncelleştirilen verilerle doldurur ve CustomerDetails nesnesinin adresi özelliğine atar gösterir. CustomerDetails nesne, daha sonra işlenmek üzere Web hizmetine geçirilir.

**16 listesi. Karmaşık iç içe geçmiş türler kullanma**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Sayfa yöntemleri oluşturma ve kullanma

Web Hizmetleri, bir ASP.NET AJAX sayfaları istemciler dahil birçok yeniden kullanılabilir hizmetleri kullanıma sunmak için mükemmel bir yol sağlar. Ancak, bir sayfa olmaz sürekli kullanılabilir veya diğer sayfalar tarafından paylaşılan verileri almak için gereken yere durumlar olabilir. Hizmet yalnızca tek bir sayfa tarafından kullanılmadığından bu durumda, verilere erişmek için sayfayı izin vermek için bir .asmx dosya düşünülecek gibi görünebilir.

ASP.NET AJAX Web hizmeti gibi çağrıları tek başına .asmx dosyalarını oluşturmadan yapmak için başka bir mekanizma sağlar. Bu, "Sayfa yöntemleri olarak" başvurulan bir yöntem kullanılarak gerçekleştirilir. Sayfa, doğrudan bir sayfası veya kod yanında dosyasında katıştırılmış WebMethod özniteliği uygulanmış statik (paylaşılan VB.NET) yöntemleri yöntemlerdir. WebMethod özniteliği uygulayarak, çalışma zamanında dinamik olarak oluşturulan PageMethods adlı özel bir JavaScript nesnesi kullanılarak çağrılabilir. PageMethods nesnesi JSON serileştirme/seri durumundan çıkarma işleminden ayrıntılarından korur bir proxy görevi görür. PageMethods nesne kullanmak için ScriptManager'ın EnablePageMethods özelliği true olarak ayarlamanız gerekir olduğunu unutmayın.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

17 listeleme ASP.NET kodu yanında sınıfında iki sayfa yöntemleri tanımlayan bir örnek gösterilmektedir. Bu yöntemler uygulamasında yer alan bir iş katmanı sınıftaki veri\_Web sitesinin kod klasörü.

**17 listesi. Sayfa yöntemleri tanımlama.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

ScriptManager sayfasında Web yöntemleri varlığını algıladığında daha önce bahsedilen PageMethods nesneye dinamik başvuru oluşturur. Web yöntemi çağırma yöntemi ve iletilmesi gereken herhangi bir gerekli parametresi veri adından önce gelen PageMethods sınıfı başvurarak gerçekleştirilir. 18 kod, daha önce gösterilen iki sayfa metotları çağırma örnekleri gösterir.

**18 listesi. PageMethods JavaScript nesne arama sayfası yöntemleri.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

PageMethods nesnesini kullanarak, bir JavaScript proxy nesnesi kullanmaya çok benzer. Önce tüm sayfa yöntemine geçirilen ve ardından zaman uyumsuz çağrı döndürdüğünde çağrılması gereken geri çağırma işlevi tanımlayan parametre verileri belirtin. Bir hata geri çağırma de belirtilebilir (hata işleme örneği için listeleme 14'e bakın).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender ve ASP.NET AJAX Araç Seti

ASP.NET AJAX Araç Seti (kullanılabilir [ http://ajax.asp.net ](http://ajax.asp.net)) Web hizmetlerine erişmek için kullanılan çeşitli denetimler sunar. Özellikle, adlı faydalı bir Denetim Araç Seti içeren `AutoCompleteExtender` Web hizmetlerini çağırır ve herhangi bir JavaScript kodu yazmadan sayfalarında verileri göstermek için kullanılabilir.

AutoCompleteExtender denetimi var olan bir metin kutusu işlevlerini genişletmek ve kullanıcıların daha fazla, aradığınız verileri kolayca bulmasına yardımcı olmak için kullanılabilir. Bir metin kutusuna yazarken denetimi Web hizmetini sorgulama için kullanılabilir ve metin kutusunun altındaki sonuçlar dinamik olarak gösterir. Şekil 4 destek uygulama için müşteri kimliklerini görüntülenecek AutoCompleteExtender denetimi kullanarak bir örnek gösterilmektedir. Kullanıcı TextBox'a farklı karakter türleri gibi farklı öğeler, bunların giriş sırasında göre aşağıda gösterilir. Kullanıcılar, ardından istenen müşteri kimliği seçebilirsiniz.

ASP.NET AJAX sayfa içinde AutoCompleteExtender kullanarak AjaxControlToolkit.dll derleme Web sitesinin bin klasörüne erişmeye eklenmesi gerekir. Araç Seti derleme eklendikten sonra içerdiği denetimler, uygulamadaki tüm sayfalar için kullanılabilir olacak şekilde, web.config dosyasında başvurmak isteyebilirsiniz. Bu Web.Config'deki içinde aşağıdaki etiketi ekleyerek yapılabilir &lt;denetimleri&gt; etiketi:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Yalnızca belirli bir sayfaya denetimi kullanmak için gerektiğinde web.config güncelleştiriliyor yerine sonraki gösterildiği bir sayfanın üstüne başvuru yönergesi ekleyerek başvuru:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![AutoCompleteExtender denetim kullanma.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Şekil 4**: AutoCompleteExtender denetimi kullanma.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-web-services/_static/image12.png))


ASP.NET AJAX Araç Seti kullanmak için Web sitesi yapılandırıldıktan sonra normal bir ASP.NET sunucu denetimi eklersiniz gibi AutoCompleteExtender denetim sayfasına çok eklenebilir. 19 kod, bir Web hizmeti çağırmak amacıyla denetimi kullanarak bir örnek gösterir.

**19 listesi. ASP.NET AJAX Araç Seti AutoCompleteExtender denetim kullanma.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender, sunucu denetimleri bulunan standart kimliği ve runat özelliklerini de dahil olmak üzere birçok farklı özelliğe sahiptir. Bunlara ek olarak, bunu önce Web hizmeti bir son kullanıcı türleri için verileri sorguladı karakterlerinin kaçının tutulacağını tanımlamanızı sağlar. Listeleme 19'gösterilen MinimumPrefixLength özelliği Hizmeti'nin TextBox'a bir karakter girildikten her zaman çağrılmasına neden olur. Bu her zaman bir karakter Web hizmeti kullanıcı türleri olduğundan, bu değer ayarlandığında metin kutusuna karakterlerle eşleşen değeri aramak için çağrılacak dikkatli olmanız gerekir. Web yöntemi hedef yanı sıra Web hizmetini çağırmak için sırasıyla ServicePath ve ServiceMethod özellikleri kullanılarak tanımlanır. Son olarak, hangi metin kutusundaki AutoCompleteExtender denetime bağlama TargetControlID özelliği tanımlar.

Çağrılan Web hizmeti daha önce bahsedildiği gibi uygulanan ScriptService özniteliği olmalıdır ve ' % s'hedef Web yöntemini prefixText ve sayısı adlı iki parametre kabul etmeniz gerekir. PrefixText parametre son kullanıcı tarafından yazılan karakter ve sayı parametresi döndürmek için ne kadar öğenin gösterir (varsayılan değer 10'dur). 20 listeleme GetCustomerIDs Web önceki listeleme 19'gösterilen AutoCompleteExtender denetimi tarafından çağrılan yöntemi bir örneği gösterilmektedir. Web yöntemi sırayla çağrıları veri verileri filtreleme ve eşleşen sonuçları döndüren işleyen yöntem katman, iş katmanı yöntemi çağırır. Veri katmanı yöntemi için kod listeleme 21'de gösterilir.

**20 listesi. AutoCompleteExtender denetiminden gönderilen verileri filtreleme.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**21 listesi. Sonuçlar üzerinde son kullanıcı girişi tabanlı filtreleme.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Sonuç

ASP.NET AJAX, çok sayıda istek ve yanıt iletileri işlemek için özel bir JavaScript kodu yazmadan Web hizmetlerini çağırmak için mükemmel destek sağlar. Bu makalede gördüğünüz JSON iletilerini işlemek ve ScriptManager denetimini kullanarak JavaScript proxy'leri tanımlama etkinleştirmek için .NET Web hizmetlerini AJAX etkinleştir. Ayrıca, nasıl JavaScript proxy'leri Web hizmetlerini çağırmak için kullanılan basit ve karmaşık türler işlemek ve hatalarla uğraşmak gördünüz. Son olarak, oluşturma ve Web hizmeti çağrıları yapma sürecini basitleştirmek için sayfa yöntemleri'nın nasıl kullanılabileceğini ve yazarken AutoCompleteExtender denetim Yardım son kullanıcılara nasıl sağlayabilirsiniz gördünüz. ASP.NET AJAX içinde kullanılabilir UpdatePanel kesinlikle, kolaylık sağlaması nedeniyle çok sayıda AJAX programcıları için tercih ettiğiniz denetim olsa da, JavaScript proxy'leri Web hizmetlerini çağırmak nasıl bilmeden birçok uygulamada yararlı olabilir.

## <a name="bio"></a>Biyografi

Dan Wahlin (Microsoft en değerli Professional ASP.NET ve XML Web Hizmetleri) olan arabirimi teknik eğitim sırasında bir .NET geliştirme Eğitmen ve mimari Danışman ([http://www.interfacett.com](http://www.interfacett.com)). Dan kurulan ASP.NET geliştiricilerinin Web sitesi için XML ([www.XMLforASP.NET](http://www.XMLforASP.NET)) üzerinde INETA konuşmacının kuruluşu olan ve çeşitli konferanslarda konuşur. Dan Professional Windows DNA (Wrox), ASP.NET yazarlarından: ipuçları, öğreticiler ve kod (kendi), ASP.NET 1.1 Insider çözümleri, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP yönlendirir ve ASP.NET geliştiricileri (kendi) için yazılan XML. He kodu, makaleler veya books değil yazarken, yazma ve müzik kaydetme ve golf ve kendi eşim ve çocuklar Basketbol yürütme Dan ölçeklenebilme.

Scott Cate 1997'den beri Microsoft Web teknolojileri ile çalışmakta olduğu ve myKB.com Yardımcısı ([www.myKB.com](http://www.myKB.com)) tabanlı Bilgi Bankası yazılım çözümlerinizi odaklı uygulamaları burada kendisinin ASP.NET yazma konusunda uzmanlaşmış. Scott temas kurulabileceğini e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog'da [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-asp-net-ajax-localization.md)
> [İleri](understanding-asp-net-ajax-debugging-capabilities.md)
