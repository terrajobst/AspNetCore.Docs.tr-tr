---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: ASP.NET AJAX Web hizmetlerini anlama | Microsoft Docs
author: scottcate
description: Web Hizmetleri, dağıtılmış sistemleri arasında veri değişimi için platformlar arası çözümü sağlamak .NET framework'ün ayrılmaz bir parçasıdır. Ancak Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 0b9f61f895fea1960ebd25780454b86d5c3ba1bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889348"
---
<a name="understanding-aspnet-ajax-web-services"></a>ASP.NET AJAX Web hizmetlerini anlama
====================
tarafından [Scott göstermek](https://github.com/scottcate)

[PDF indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Web Hizmetleri, dağıtılmış sistemleri arasında veri değişimi için platformlar arası çözümü sağlamak .NET framework'ün ayrılmaz bir parçasıdır. Web Hizmetleri, normalde farklı işletim sistemleri, nesne modelleri ve veri göndermek ve almak için programlama dilleri izin vermek için kullanılsa dinamik olarak bir ASP.NET AJAX sayfasına veri ekleme ya da bir arka uç sisteme sayfadan veri göndermek için de kullanılabilir. Tüm bu işlemleri geri gönderme başvurmadan yapılabilir.


## <a name="calling-web-services-with-aspnet-ajax"></a>ASP.NET AJAX ile arama Web Hizmetleri

Dan Wahlin

Web Hizmetleri, dağıtılmış sistemleri arasında veri değişimi için platformlar arası çözümü sağlamak .NET framework'ün ayrılmaz bir parçasıdır. Web Hizmetleri, normalde farklı işletim sistemleri, nesne modelleri ve veri göndermek ve almak için programlama dilleri izin vermek için kullanılsa dinamik olarak bir ASP.NET AJAX sayfasına veri ekleme ya da bir arka uç sisteme sayfadan veri göndermek için de kullanılabilir. Tüm bu işlemleri geri gönderme başvurmadan yapılabilir.

ASP.NET AJAX UpdatePanel denetim AJAX basit bir şekilde sağlarken tüm ASP.NET sayfası etkinleştirmek için dinamik olarak sunucu üzerindeki bir UpdatePanel kullanmadan verilere gerektiğinde zamanlar olabilir. Bu makalede, bunu oluşturarak ve Web Hizmetleri içinde ASP.NET AJAX sayfalar kullanma gerçekleştirmek nasıl görürsünüz.

Bu makalede, ASP.NET AJAX AutoCompleteExtender adlı Araç Seti Web hizmetinin etkinleştirilmiş denetiminde yanı sıra çekirdek ASP.NET AJAX uzantıları kullanılabilir işlevselliği hakkında odaklanır. Kapsanan konular AJAX etkinleştirilmiş Web Hizmetleri tanımlama, istemci proxy'leri oluşturma ve Web Hizmetleri JavaScript ile çağırma içerir. Ayrıca, Web hizmeti çağrıları doğrudan ASP.NET sayfası yöntemleri için nasıl yapılabilir görürsünüz.

## <a name="web-services-configuration"></a>Web Hizmetleri Yapılandırması

Visual Studio 2008 ile yeni bir Web sitesi projesi oluşturduğunuzda, web.config dosyasını Visual Studio'nun önceki sürümleri kullanıcılara tanınmayan eklemeleri sayısına sahip. Başkalarının gerekli HttpHandlers ve HttpModules tanımlayın sırada sayfalarında kullanılabilmesi için bu değişikliklerden bazıları ASP.NET AJAX denetimlere "asp" öneki eşleyin. Kod 1 yapılan değişiklikleri gösterir `<httpHandlers>` Web hizmeti çağrıları etkiler web.config öğesinde. HttpHandler .asmx çağrıları işlemek için kullanılan varsayılan kaldırılır ve System.Web.Extensions.dll derlemesinde bulunan ScriptHandlerFactory sınıfı ile değiştirilir. System.Web.Extensions.dll tüm ASP.NET AJAX tarafından kullanılan çekirdek işlevselliğini içerir.

**1 listesi. ASP.NET AJAX Web hizmeti işleyici yapılandırması**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Bu HttpHandler değiştirme, bir JavaScript Web hizmeti proxy'si kullanarak .NET Web Hizmetleri için ASP.NET AJAX sayfalarından yapılması JavaScript nesne gösterimi (JSON) çağrılarına izin vermek için yapılır. ASP.NET AJAX genellikle Web Hizmetleri ile ilişkili standart Basit Nesne Erişim Protokolü (SOAP) aramaları aksine Web hizmetlerine JSON iletileri gönderir. Bu, daha küçük istek ve yanıt iletileri genel ile sonuçlanır. ASP.NET AJAX JavaScript kitaplığı JSON nesneleriyle çalışmak için optimize edilmiştir beri verileri daha verimli istemci tarafı işleme için de sağlar. 2 ve 3 listeleme Göster JSON biçimine serileştirilmiş Web hizmeti isteği ve yanıt iletileri örnekleri listesi. Müşteri nesnelerinin bir dizisi ve bunların ilişkili özelliklerini listeleme 3 yanıt iletisinde geçirir listeleme 2'de gösterilen istek iletisi ülke parametresi "Belçika" değerini iletir.

**2 listesi. JSON olarak serileştirilmiş web hizmeti istek iletisi**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] işlem adı web hizmetinin URL'sini bir parçası olarak tanımlanan; Ayrıca, istek iletilerini her zaman aracılığıyla gönderilen değil. Web hizmetleri kullanan ScriptMethod özniteliği UseHttpGet parametresi aracılığıyla geçirilecek parametreler neden true olarak ayarlanmış bir sorgu dizesi parametreleri.*


**3 listesi. JSON olarak serileştirilmiş web hizmeti yanıtı iletisi**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Sonraki bölümde, Web Hizmetleri JSON istek iletilerini işleme ve basit ve karmaşık türleri ile yanıt yeteneğine oluşturma görürsünüz.

## <a name="creating-ajax-enabled-web-services"></a>AJAX etkinleştirilmiş Web hizmetleri oluşturma

ASP.NET AJAX framework Web hizmetlerini çağırır için birçok farklı yol sağlar. AutoCompleteExtender denetimi (ASP.NET AJAX araç setindeki kullanılabilir) veya JavaScript kullanabilirsiniz. Böylece istemci-komut dosyası kodu tarafından çağrılabilir ancak bir hizmet çağırmadan önce AJAX etkinleştirmek için bunu vardır.

ASP.NET Web Hizmetleri için yeni olsun veya olmasın, onu oluşturmak basit ve AJAX Etkinleştirme Hizmetleri bulabilirsiniz. .NET framework ASP.NET Web hizmetleri oluşturma ve ilk yayınlandığı 2002 beri desteklenen ve ASP.NET AJAX uzantıları .NET framework'ün varsayılan özellikler kümesi derlemeler ek AJAX işlevsellik sağlar. Visual Studio .NET 2008 Beta 2 .asmx Web hizmeti dosyaları oluşturmak için yerleşik desteğe sahiptir ve otomatik olarak sınıfları yanında ilişkili kodu System.Web.Services.WebService'in sınıfından türetilir. Sınıfına yöntemleri ekledikçe WebMethod özniteliği için bunları Web hizmeti tüketiciler tarafından çağrılacak sırayla uygulamanız gerekir.

4 listeleme WebMethod özniteliği GetCustomersByCountry() adlı bir yöntem uygulamanın bir örneği gösterir.

**Listing 4. Bir Web hizmeti WebMethod özniteliğini kullanma**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

GetCustomersByCountry() yöntemi ülke parametre kabul eder ve bir müşteri nesne dizisi döndürür. Yönteme geçirilen ülke değeri, veritabanından veri almak için bir veri katmanı sınıf verilerle müşteri nesne özelliklerini doldurun ve dizi döndürecek çağıran DLL'den iş katmanı sınıfına iletilir.

## <a name="using-the-scriptservice-attribute"></a>ScriptService özniteliğini kullanma

Web hizmetine standart SOAP iletileri gönderen istemciler çağrılacak GetCustomersByCountry() yöntemi özniteliğe izin WebMethod eklenirken, kutunun dışında ASP.NET AJAX uygulamalardan yapılması JSON çağrıları izin vermez. ASP.NET AJAX uzantının uygulamak size yapılması JSON çağrılarına izin vermek için `ScriptService` özniteliği için Web hizmeti sınıfı. Bu JSON kullanılarak biçimlendirilmiş yanıt iletilerini göndermek üzere bir Web hizmetini etkinleştirir ve JSON iletileri göndererek hizmet çağırmak istemci tarafı komut dosyası sağlar.

5 listeleme ScriptService özniteliği CustomersService adlı bir Web hizmeti sınıfı için uygulama örneği gösterir.

**5 listesi. AJAX özellikli bir Web hizmeti için ScriptService özniteliğini kullanma**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

ScriptService özniteliği AJAX komut dosyası kodundan çağrılmak gösteren bir işaretçi olarak görev yapar. Bu gerçekten arka planda oluşan JSON seri hale getirme veya seri durumdan çıkarma görevlerden herhangi birini işleyemez. (Web.config dosyasında yapılandırılmış) ScriptHandlerFactory ve diğer ilgili sınıflar JSON işleme toplu yapın.

## <a name="using-the-scriptmethod-attribute"></a>ScriptMethod özniteliğini kullanma

Bir .NET Web hizmetinde ASP.NET AJAX sayfaları tarafından kullanılması için bu tanımlanması gerekir yalnızca ASP.NET AJAX öznitelik ScriptService özniteliğidir. Ancak, ScriptMethod adlı başka bir öznitelik de doğrudan Web yöntemleri bir hizmette uygulanabilir. ScriptMethod tanımlar dahil olmak üzere üç özellik `UseHttpGet`, `ResponseFormat` ve `XmlSerializeString`. Bu özelliklerin değerlerini değiştirme Web yöntemi tarafından kabul edilen istek türü gereken bir Web yöntemini biçiminde ham XML verileri döndürmek gerektiğinde GET için değiştirilecek durumlarda yararlı olabilir bir `XmlDocument` veya `XmlElement` nesne veya veri olduğunda döndürülen bir  Hizmet, JSON yerine XML olarak her zaman serileştirilmelidir.

Web yöntemi kabul etmelidir özelliği kullanılabilir UseHttpGet POST istekleri aksine istekleri alın. Web yöntemi giriş parametreleriyle bir URL'yi kullanarak sorgu dizesi parametreleri dönüştürülen gönderdiği istekleri. Özellik varsayılan olarak false değerine ve yalnızca UseHttpGet ayarlanabilir `true` işlemleri zaman bilinen uyumlu ve zaman hassas verileri bir Web hizmetine aktarılmaz. 6 listeleme UseHttpGet özelliğiyle ScriptMethod özniteliğini kullanarak bir örnek gösterilmektedir.

**6 listesi. ScriptMethod özniteliği UseHttpGet özelliğiyle kullanma.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

HttpGetEcho Web listeleme 6'da gösterilen yöntemi çağrıldığında gönderilen üstbilgileri örneği gösterilen sonraki:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Bir hizmet JSON yerine döndürülecek XML yanıtlarını ihtiyacınız olduğunda Web HTTP GET isteklerini kabul edecek şekilde yöntemlerine izin yanı sıra, ScriptMethod özniteliği de kullanılabilir. Örneğin, bir Web hizmeti uzak bir siteden bir RSS akışı almak ve bir XmlDocument veya XmlElement nesne olarak döndürür. XML'sini işleme veri ardından istemcide ortaya çıkabilir.

7 listeleme XML verilerini bir Web yönteminden döndürülen olduğunu belirtmek için ResponseFormat özelliğini kullanarak bir örnek gösterilmektedir.

**Listing 7. ScriptMethod özniteliği ResponseFormat özelliğiyle kullanma.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

ResponseFormat özelliği de XmlSerializeString özelliği ile birlikte kullanılabilir. XmlSerializeString özelliği dönüş tüm Web yönteminden döndürülen dizeleri dışında türlerine XML olarak serileştirilmiş anlamı false varsayılan değerine sahip olduğunda `ResponseFormat` özelliği ayarlanmış `ResponseFormat.Xml`. Zaman `XmlSerializeString` ayarlanır `true`, Web yöntemi tarafından döndürülen tüm türleri dize türleri dahil olmak üzere XML olarak serileştirilmiş. ResponseFormat özellik değeri varsa `ResponseFormat.Json` XmlSerializeString özelliği göz ardı edilir.

8 listeleme XML olarak serileştirilmiş dizeleri zorlamak için XmlSerializeString özelliğini kullanarak bir örnek gösterilmektedir.

**8 listesi. XmlSerializeString özelliğiyle ScriptMethod özniteliğini kullanma**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

GetXmlString Web listeleme 8'de gösterilen yöntemi çağırma döndürülen değer sonraki gösterilir:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Varsayılan JSON biçimi istek ve yanıt iletileri toplam boyutunu en aza indirir ve ASP.NET AJAX istemciler tarafından bir çapraz tarayıcı şekilde daha kolay kullanılır, ancak ResponseFormat ve XmlSerializeString özellikleri olabilir ne zaman göstermesi istemci uygulamalar gibi Internet Explorer 5 veya daha yüksek bir Web yönteminden döndürülen için XML verileri bekler.

## <a name="working-with-complex-types"></a>Karmaşık türleri ile çalışma

5 listeleyen bir Web hizmeti müşteriden adlı bir karmaşık türü döndüren bir örnek gösterilmiştir. Müşteri sınıf birkaç farklı basit türler FirstName ve LastName gibi özellikleri olarak dahili olarak tanımlar. Bir AJAX etkinleştirilmiş Web yönteminin dönüş türüne otomatik olarak serileştirilir JSON içinde istemci-tarafı gönderilmeden önce veya karmaşık türler giriş parametresi olarak kullanılabilir. Ancak, iç içe geçmiş karmaşık türler (tanımlananlar dahili içinde başka bir tür) istemciye tek başına nesneler varsayılan olarak kullanılabilir duruma getirilmez.

Burada bir Web hizmeti tarafından kullanılan iç içe geçmiş bir karmaşık tür ayrıca bir istemci sayfasında kullanılmalıdır durumlarda, ASP.NET AJAX GenerateScriptType özniteliği Web hizmetine eklenebilir. Örneğin, adresi ve cinsiyetiniz özelliklerini listeleme 9'da gösterilen CustomerDetails sınıfı içerir, ***iç içe geçmiş karmaşık türler temsil eder.***

**Listing 9. Burada gösterilen CustomerDetails sınıfı iki iç içe geçmiş karmaşık türler içerir.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

İç içe geçmiş türler olduğundan listeleme 9'da gösterilen CustomerDetails sınıfı içinde tanımlanan adres ve cinsiyetiniz nesneleri otomatik olarak istemci tarafı JavaScript aracılığıyla üzerinde kullanılabilir yapılması olmaz (adresi bir sınıf, cinsiyetiniz ise numaralandırma). Burada bir Web hizmeti içinde kullanılan bir iç içe geçmiş tür istemci tarafında kullanılabilir olmalıdır durumlarda, daha önce bahsedilen GenerateScriptType özniteliği kullanılabilir (listeleme 10'a bakın). Bu öznitelik durumlarda farklı iç içe geçmiş karmaşık türler hizmetten döndürülen burada birden çok kez eklenebilir. Doğrudan Web hizmeti sınıfı veya belirli Web yöntemleri yukarıda uygulanabilir.

**10 listesi. İstemciye kullanılabilir olması gerektiğini iç içe geçmiş türler tanımlamak için GenerateScriptService özniteliğini kullanarak.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Uygulayarak `GenerateScriptType` özniteliği Web hizmeti, adres ve cinsiyetiniz türleri için otomatik olarak oluşturulacak kullanılabilir istemci-tarafı ASP.NET AJAX JavaScript kodu tarafından. Otomatik olarak oluşturulan ve bir Web hizmeti GenerateScriptType özniteliği ekleyerek istemciye gönderilen JavaScript örneği listeleme 11'de gösterilir. İç içe geçmiş karmaşık türler makalenin sonraki bölümlerinde kullanma görürsünüz.

**11 listesi. İç içe geçmiş karmaşık türler bir ASP.NET AJAX sayfasına kullanılabilir.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Web hizmetleri oluşturma ve ASP.NET AJAX sayfalara erişilebilir gördüğünüze göre oluşturma ve JavaScript proxy'leri kullanabilir, böylece veri alınan ya da Web hizmetlerine gönderilen bir göz atalım.

## <a name="creating-javascript-proxies"></a>JavaScript proxy'leri oluşturma

Standart bir Web hizmeti (.NET veya başka bir platform) genellikle çağırma SOAP isteği ve yanıt iletileri gönderme karmaşıklık ayrıntılarından korur bir proxy nesnesi oluşturma içerir. ASP.NET AJAX Web hizmeti çağrıları ile JavaScript proxy'leri oluşturulur ve seri hale getirme ve seri JSON iletileri hakkında endişelenmeden Hizmetleri kolayca çağırmak için kullanılır. JavaScript proxy'leri, ASP.NET AJAX ScriptManager denetimi kullanarak otomatik olarak oluşturulabilir.

Web hizmetlerini çağıran bir JavaScript proxy'si oluşturuluyor ScriptManager'ın Services özelliği kullanılarak gerçekleştirilir. Bu özellik, bir ASP.NET AJAX sayfası göndermek veya geri gönderme işlemleri gerek kalmadan veri almak için zaman uyumsuz olarak çağırabilirsiniz bir veya daha fazla hizmet tanımlamanıza olanak sağlar. ASP.NET AJAX'ı kullanarak bir hizmet tanımlama `ServiceReference` denetim ve Web hizmeti URL'si denetimin atama `Path` özelliği. 12 listeleme CustomersService.asmx adlı bir hizmette başvuran bir örnek gösterilmektedir.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**12 listesi. Bir ASP.NET AJAX sayfa içinde kullanılan bir Web hizmeti tanımlama.**

ScriptManager denetimi ile CustomersService.asmx bir başvuru ekleyerek, dinamik olarak oluşturulan ve sayfa tarafından başvurulan bir JavaScript proxy'si neden olur. Proxy kullanarak katıştırılmış &lt;betik&gt; etiketi ve CustomersService.asmx dosyasını çağıran ve onu sonuna /js ekleyerek tarafından dinamik olarak yüklendi. Aşağıdaki örnek, hata ayıklama web.config dosyasında devre dışı bırakıldığında nasıl JavaScript proxy'si sayfaya katıştırılmış gösterir:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Oluşturulan gerçek JavaScript proxy kodu görmek isterseniz, Internet Explorer'ın adres kutusuna istenen .NET Web hizmetinin URL'sini yazın ve /js bunu sona ekleyin.*


Hata ayıklama sürümü JavaScript proxy'si sayfasındaki katıştırılmış web.config dosyasında hata ayıklama etkinse, sonraki gösterilmektedir:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

ScriptManager tarafından oluşturulan JavaScript proxy'si de doğrudan sayfasına katıştırılmış yerine kullanarak başvurulan &lt;betik&gt; etiketinin src özniteliği. Bu, (varsayılan değer false) true olarak ServiceReference denetimin Inlinescript özelliği ayarlanarak yapılabilir. Bir proxy sunucusuna yapılan ağ çağrılarının sayısını azaltmak istediğiniz zaman ve birden çok sayfa arasında paylaşılmayan bu yararlı olabilir. Varsayılan değer false olarak proxy ASP.NET AJAX uygulamanın birden çok sayfada tarafından kullanıldığı durumlarda önerilir Inlinescript proxy betiği true olarak ayarlandığında tarayıcı tarafından önbelleğe olmaz. Inlinescript özelliğini kullanarak bir örnek sonraki gösterilir:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>JavaScript proxy'leri kullanma

Bir Web hizmeti ScriptManager denetimi kullanarak bir ASP.NET AJAX sayfası tarafından başvurulan sonra Web hizmetine çağrı yapılabilmesi için ve döndürülen verileri geri arama işlevleri kullanılarak işlenebilir. Bir Web hizmeti (varsa), ad alanı başvuran, sınıf adını ve Web yöntemi adı tarafından çağrılır. Web hizmetine geçirilen herhangi bir parametre döndürülen verileri işleyen bir geri çağırma işlevini birlikte tanımlanabilir.

GetCustomersByCountry() adlı bir Web yöntemini çağırmak için bir JavaScript proxy'si kullanma örneği listeleme 13'te gösterilir. Bir son kullanıcı sayfasında bir düğmesini tıklattığında GetCustomersByCountry() işlevi çağrılır.

**13 listesi. Bir Web hizmeti ile bir JavaScript proxy'si çağrılıyor.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Bu çağrı InterfaceTraining ad alanına başvuruyor, hizmette tanımlanan CustomersService sınıfı ve GetCustomersByCountry Web yöntemi. Zaman uyumsuz Web hizmeti çağrısı döndürüldüğünde, çağrılması gereken OnWSRequestComplete adlı bir geri çağırma işlevini yanı sıra bir metin alınan bir ülke değer geçirir. OnWSRequestComplete hizmetten döndürülen müşteri nesneler dizisi işler ve bunları sayfada görüntülenen bir tabloya dönüştürür. Çağrısından oluşturulan çıktı Şekil 1'de gösterilir.


[![Bir Web hizmeti için bir zaman uyumsuz AJAX çağrısı yaparak alınan veriler bağlama.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Şekil 1**: veri bağlama elde bir Web hizmeti için bir zaman uyumsuz AJAX çağrısı yaparak.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript proxy'leri, Web yöntemi çağrılmalıdır ancak proxy için bir yanıt beklemesi döndürmemelidir durumlarda tek yönlü çağrıları Web Hizmetleri için de yapabilirsiniz. Örneğin, bir iş akışı gibi bir işlemi başlatmak, ancak hizmetinden dönüş değeri için bekleme için bir Web hizmeti çağrısı isteyebilirsiniz. Burada bir hizmet için yapılması tek yönlü bir çağrı gerekir durumlarda listeleme 13'te gösterilen geri çağırma işlevi yalnızca atlanabilir. Geri çağırma işlevi tanımlamış verileri döndürmek Web hizmeti için proxy nesnesi beklemez.

## <a name="handling-errors"></a>Hataları işleme

Web Hizmetleri zaman uyumsuz geri aramalar hataları aşağı, Web Hizmeti'nin kullanılabilir durumda veya döndürülen bir özel durum olan ağ gibi farklı türlerde karşılaşabilirsiniz. Neyse ki, JavaScript proxy nesneleri ScriptManager tarafından oluşturulan hataları ve daha önce gösterilen başarı geri çağırma yanı sıra hataları işlemek için tanımlanmış birden çok geri aramalar izin verin. Bir hata geri çağırma işlevini listeleme 14'te gösterildiği gibi standart geri çağırma işlevi çağrısında Web yönteminin hemen sonra tanımlanabilir.

**14 listesi. Bir hata geri çağırma işlevini tanımlama ve hataları görüntüleme.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Web hizmeti çağrıldığında oluşabilecek hatalar, bir parametre olarak hatasını temsil eden bir nesne kabul çağrılacak OnWSRequestFailed() geri çağırma işlevi tetikler. Hata nesnesi yanı sıra desteklemediğini çağrı zaman aşımına uğradı hatanın nedenini belirlemek için birkaç farklı işlevler sunar. 14 listeleme farklı hata işlevler kullanılarak bir örneği gösterir ve Şekil 2 işlevleri tarafından oluşturulan çıktı örneği gösterir.


[![ASP.NET AJAX hata işlevleri çağırma oluşturulan çıktı.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Şekil 2**: ASP.NET AJAX hata işlevleri çağırma oluşturulan çıktı.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Bir Web hizmetinden döndürülen XML verileri işleme

Daha önce ResponseFormat özelliğinin yanı sıra ScriptMethod özniteliğini kullanarak bir Web yöntemini ham XML verilerini nasıl döndürebilirsiniz gördünüz. ResponseFormat ResponseFormat.Xml için ayarlandığında, Web hizmetinden döndürülen veriler JSON yerine XML olarak serileştirilir. XML verilerini JavaScript veya XSLT kullanarak işlemek için doğrudan istemciye iletilecek gerektiğinde bu yararlı olabilir. Şu anda Internet Explorer 5 veya daha yüksek ayrıştırma ve onun MSXML için yerleşik destek nedeniyle XML verileri filtreleme için en iyi istemci-tarafı nesne modeli sağlar.

Bir Web hizmetinden XML verilerini alma diğer veri türleri almaktan farklı değildir. Uygun bir işlevi çağırmak ve bir geri çağırma işlevini tanımlamak için JavaScript proxy'si çağırarak başlatın. Çağrı döndüğünde sonra geri çağırma işlevi verilerde işleyebilir.

15 listeleyen bir XmlElement nesnesi döndüren GetRssFeed() adlı bir Web yöntemini çağırarak bir örnek gösterilmektedir. GetRssFeed() RSS almak için akışı için URL'yi temsil eden bir parametre kabul eder.

**15 listesi. Bir Web hizmetinden döndürülen XML verileri ile çalışma.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Bu örnek, bir URL bir RSS akışına geçirir ve OnWSRequestComplete() işlevinde döndürülen XML verileri işler. OnWSRequestComplete() ilk tarayıcı MSXML ayrıştırıcısı kullanılabilir olup olmadığını öğrenmek için Internet Explorer olup olmadığını denetler. İse, bir XPath ifadesi tüm bulmak için kullanılan &lt;öğesi&gt; RSS akışı içinde etiketler. Her öğe sonra aracılığıyla yinelendiğinde ve ilişkili &lt;başlık&gt; ve &lt;bağlantı&gt; etiketleri bulunur ve her öğenin verileri görüntülemek için işlenen. Şekil 3'te bir ASP.NET AJAX GetRssFeed() Web yöntemi için JavaScript proxy'si aracılığıyla çağrı yapmasını oluşturulan çıktı örneği gösterilmektedir.

## <a name="handling-complex-types"></a>Karmaşık türler işleme

Kabul edilen veya bir Web hizmeti tarafından döndürülen karmaşık türler otomatik olarak bir JavaScript proxy'si aracılığıyla sunulur. Ancak, GenerateScriptType özniteliği hizmetine daha önce bahsedildiği gibi uygulanan sürece iç içe geçmiş karmaşık türler istemci tarafında doğrudan erişilebilir değildir. Neden istemci tarafında iç içe geçmiş bir karmaşık tür kullanmak istiyor?

Bu soruyu yanıtlamak için bir ASP.NET AJAX sayfası müşteri verileri görüntüler ve Müşteri'nin adresini güncelleştirmek son kullanıcıların sağlar varsayalım. Web hizmeti adres türünü (CustomerDetails sınıfı içinde tanımlanan bir karmaşık tür) istemciye gönderilip gönderilmediğini belirtiyorsa güncelleştirme işlemini daha iyi kod yeniden kullanım için ayrı işlevler halinde ayrılabilir.


[![RSS verileri döndüren bir Web hizmeti çağırma oluşturma çıktı.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Şekil 3**: çıktı RSS veri döndüren bir Web hizmeti çağırma oluşturma.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-web-services/_static/image9.png))


16 listeleme Model ad alanında tanımlı bir adres nesne çağırır istemci-tarafı kod örneğini güncelleştirilmiş verilerle doldurur ve CustomerDetails nesnenin adresi özelliğine atar gösterir. CustomerDetails nesnesi daha sonra işlenmek üzere Web hizmetine geçirilir.

**16 listesi. İç içe geçmiş karmaşık türler kullanma**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Sayfa yöntemleri oluşturma ve kullanma

Web hizmetleri yeniden kullanışlı Hizmetleri istemcileri ASP.NET AJAX sayfalar dahil olmak üzere çeşitli kullanıma sunmak için mükemmel bir yol sağlar. Ancak, burada olmaz hiç kullanılan veya diğer sayfalar tarafından paylaşılan verileri almak için bir sayfa gereken durumlar olabilir. Hizmet yalnızca tek bir sayfayla tarafından kullanıldığından, bu durumda verilere erişmek sayfanın izin vermek için bir .asmx dosyasında yapma gereğinden fazla gibi görünebilir.

ASP.NET AJAX tek başına .asmx dosyalarını oluşturmadan Web hizmeti gibi çağrıları yapma için başka bir mekanizma sağlar. Bu işlem için "sayfası yöntemleri" olarak adlandırılan bir yöntemi kullanarak gerçekleştirilir. Sayfa yöntemleri uygulanmış WebMethod özniteliği olan doğrudan bir sayfa veya kod yanında dosyasında katıştırılmış statik (paylaşılan içinde VB.NET) yöntemleridir. WebMethod özniteliği uygulayarak bunlar çalışma zamanında dinamik olarak oluşturulan PageMethods adlı özel bir JavaScript nesne kullanılarak çağrılabilir. PageMethods nesnesi JSON serileştirme/seri durumdan çıkarma işleminden ayrıntılarından korur bir proxy olarak görev yapar. PageMethods nesnesinin kullanabilmesi için ScriptManager'ın EnablePageMethods özelliği true olarak ayarlamanız gerekir olduğunu unutmayın.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

17 listeleme ASP.NET kodu yanında sınıfında iki sayfa yöntemleri tanımlayan bir örnek gösterilmektedir. Bu yöntemler uygulamada bulunan bir iş katmanı sınıftan verileri\_Web sitesinin kod klasör.

**17 listesi. Sayfa yöntemleri tanımlama.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

ScriptManager sayfasında Web yöntemleri varlığını algıladığında daha önce bahsedilen PageMethods nesnesine dinamik bir başvuru oluşturur. Web yöntemi çağırma yöntemi ve iletilmesi gereken tüm gerekli parametre veri adından PageMethods sınıfı başvurarak gerçekleştirilir. 18 kod daha önce gösterilen iki sayfa yöntemleri çağırma örnekleri gösterir.

**18 listesi. PageMethods JavaScript nesne arama sayfası yöntemleri.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

PageMethods nesnesini kullanarak, bir JavaScript proxy nesnesi kullanarak çok benzer. Önce tüm sayfa yönteme geçirilen ve zaman uyumsuz çağrısı döndürüldüğünde çağrılmalıdır geri çağırma işlevi tanımlayan parametresi verileri belirtin. Bir hata geri çağırma de belirtilebilir (hatalarını işleme örneği için listeleme 14'e bakın).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender ve ASP.NET AJAX Araç Seti

ASP.NET AJAX Araç Seti (kullanılabilir [ http://ajax.asp.net ](http://ajax.asp.net)) Web hizmetlerine erişmek için kullanılan birkaç denetim sunar. Özellikle, araç seti adlı faydalı bir denetim içerir `AutoCompleteExtender` Web hizmetlerini çağırır ve herhangi bir JavaScript kod yazmadan sayfalarında verileri göstermek için kullanılabilir.

AutoCompleteExtender denetim textbox mevcut işlevselliğini genişletmek ve kolayca olarak veri bulmalarına daha fazla yardım için kullanılabilir. Bir metin kutusuna yazarken denetim bir Web hizmeti sorgulamak için kullanılır ve sonuçları metin kutusu altında dinamik olarak gösterir. Şekil 4 müşteri kimlikleri için destek uygulamayı görüntülemek için AutoCompleteExtender denetimini kullanarak bir örnek gösterilmektedir. Kullanıcı, metin kutusuna farklı karakter türleri gibi farklı öğeler üzerinde kendi giriş göre aşağıda gösterilir. Kullanıcılar daha sonra istenen müşteri kimliği seçebilirsiniz.

Bir ASP.NET AJAX sayfa içinde AutoCompleteExtender kullanarak AjaxControlToolkit.dll derleme Web sitesinin bin klasörüne eklenmesini gerektirir. Araç Seti derleme eklendiğinde içerdiği denetimleri bir uygulamadaki tüm sayfalar için kullanılabilir olacak şekilde, web.config dosyasında başvuru istersiniz. Bu web.config'ın içinde aşağıdaki etiketi ekleyerek yapılabilir &lt;denetimleri&gt; etiketi:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Durumlarda burada yalnızca belirli bir sayfaya denetimi kullanmak için gerekir, bu web.config güncelleştirme yerine sonraki gösterildiği gibi bir sayfanın üstüne başvuru yönergesi ekleyerek başvurabilir:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![AutoCompleteExtender denetim kullanma.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Şekil 4**: AutoCompleteExtender denetimini kullanma.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-web-services/_static/image12.png))


Web sitesi, ASP.NET AJAX Araç Seti kullanmak üzere yapılandırıldıktan sonra normal ASP.NET sunucu denetimi eklediğiniz gibi bir AutoCompleteExtender denetimi sayfaya çok eklenebilir. 19 listeleyen bir Web hizmetini çağırmak için denetimi kullanma örneği gösterir.

**19 listesi. ASP.NET AJAX Araç Seti AutoCompleteExtender denetim kullanma.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender sunucu denetimleri üzerinde bulunan standart kimliği ve runat özellikler de dahil olmak üzere birçok farklı özelliğe sahiptir. Bunlara ek olarak, Web hizmeti önce son kullanıcı türleri için verileri sorgulanan kaç karakterin tanımlamanızı sağlar. Listeleme 19'gösterilen MinimumPrefixLength özelliği Hizmeti'nin bir karakterin metin kutusuna yazılan her zaman çağrılmasına neden olur. Her bir karakteri Web hizmeti kullanıcı türleri olduğundan bu değer ayarlandığında metin kutusuna karakterlerle eşleşen değerleri aramak için çağrılacak dikkatli olun istersiniz. Web yöntemi hedef yanı sıra çağırmak için Web hizmeti ServicePath ve ServiceMethod özellikleri sırasıyla kullanılarak tanımlanır. Son olarak, hangi textbox kanca AutoCompleteExtender denetimi TargetControlID özelliği tanımlar.

Çağrılan Web hizmeti daha önce bahsedildiği gibi uygulanan ScriptService özniteliğe sahip olması gerekir ve hedef Web yöntemi prefixText ve sayısı adlı iki parametre kabul etmeniz gerekir. Son kullanıcı tarafından yazılan karakterler prefixText parametresini temsil eder ve sayı parametresi döndürmek için kaç tane öğelerini temsil eder (varsayılan değer 10'dur). 20 listeleme GetCustomerIDs Web önceki listeleme 19'gösterilen AutoCompleteExtender denetimi tarafından çağrılan yöntemi bir örneği gösterilmektedir. Web yöntemi sırayla çağrıları veri verileri filtreleme ve eşleşen sonuçları döndüren işleme yöntemi katman, iş katmanı yöntemini çağırır. Veri katmanı yöntemi için kod listeleme 21'de gösterilir.

**20 listesi. AutoCompleteExtender denetiminden gönderilen verileri filtreleme.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**21 listesi. Son kullanıcı girişi sonuçları filtreleme.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Sonuç

ASP.NET AJAX çok fazla istek ve yanıt iletileri işlemek için özel JavaScript kod yazmadan Web Hizmetleri çağırmak için mükemmel destek sağlar. Bu makalede gördüğünüz JSON iletilerini işlemek ve ScriptManager denetimi kullanarak JavaScript proxy'leri tanımlamak nasıl etkinleştirmek için AJAX etkinleştir .NET Web hizmetlerini. Ayrıca, nasıl JavaScript proxy'leri Web Hizmetleri çağırmak için kullanılan basit ve karmaşık türleri işlemek ve hatalarıyla ilgilidir gördünüz. Son olarak, sayfa yöntemleri oluşturma ve Web hizmeti çağrıları yapma işlemini basitleştirmek için nasıl kullanılabileceği ve nasıl bunlar yazarken AutoCompleteExtender denetim son kullanıcılara Yardım sağlayabilir gördünüz. ASP.NET AJAX ' kullanılabilir UpdatePanel kesinlikle kendi kolaylık olması nedeniyle birçok AJAX programcıları için seçim denetim olsa da, Web Hizmetleri JavaScript proxy'leri çağırmak nasıl bilerek birçok uygulamada yararlı olabilir.

## <a name="bio"></a>Biyografisi

Dan Wahlin (Microsoft en değerli profesyonel ASP.NET ve XML Web Hizmetleri için) olan arabirimi teknik eğitim sırasında bir .NET geliştirme Eğitmen ve mimari Danışman ([http://www.interfacett.com](http://www.interfacett.com)). Dan kurulan ASP.NET geliştiricilerinin Web sitesi için XML ([www.XMLforASP.NET](http://www.XMLforASP.NET)), üzerinde INETA Konuşmacı kuruluşu olması ve birkaç konferans konuşur. Dan birlikte Professional Windows DNA (Wrox), ASP.NET yazılan: ipuçları, öğreticiler ve kod (kendi), ASP.NET 1.1 Insider çözümleri, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP yönlendirir ve ASP.NET geliştiricilerinin (kendi) için yazılan XML. Kendisine kod, makaleler veya books değil yazarken, yazma ve müzik kaydetme ve golf ve kendi wife ve çocuklar Basketbol çalma Dan özgürlüğüne.

Tan göstermek Microsoft Web teknolojileri ile bu yana 1997 çalışma ve myKB.com Başkanı ise ([www.myKB.com](http://www.myKB.com)) kendisine ASP.NET yazılırken burada uzmanlaşmış tabanlı Bilgi Bankası yazılım çözümlerini odaklanmış uygulamaları. Tan temas kurulabileceğini doğrula e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog adresindeki [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-asp-net-ajax-localization.md)
> [sonraki](understanding-asp-net-ajax-debugging-capabilities.md)
