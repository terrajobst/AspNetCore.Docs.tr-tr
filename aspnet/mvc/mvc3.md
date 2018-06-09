---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (Nisan 2011 içerir Araçları güncelleştirmesi) ASP.NET MVC 3 tanınmış tasarım desenini kullanarak ölçeklenebilir, standartlara dayalı web uygulamaları oluşturmaya yönelik bir çerçevedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034743"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(Nisan 2011 içerir Araçları güncelleştirmesi)*
> 
> ASP.NET MVC 3 tanınmış tasarım desenleri ve ASP.NET ve .NET Framework gücünü kullanarak ölçeklenebilir, standartlara dayalı web uygulamaları oluşturmaya yönelik bir çerçevedir.
> 
> ASP.NET MVC kullanmaya bugün başlamak için 2, yan yana yükler!
> 
> Karşıdan [burada yükleyici](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Üst özellikleri

- Tümleşik yapı İskelesi sistem NuGet üzerinden Genişletilebilir
- HTML 5 etkin proje şablonları
- Yeni Razor görüntüleme altyapısı dahil olmak üzere açıklayıcı görünümleri
- Bağımlılık ekleme ve genel eylem filtreleri ile güçlü kancaları
- Örtük JavaScript'i, jQuery doğrulama ve JSON bağlama ile zengin JavaScript desteği
- *Tam özellik listesi okuma [aşağıda](#overview)*

## <a name="top-links"></a>Üst bağlantılar

ASP.NET MVC 3 yenilikler nelerdir?

- Phil Haack: [ASP.NET MVC 3 piyasaya Sürüldü](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express ve Orchard yayımlanan - Microsoft Ocak Web sürümde bağlamı](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix sürümü Duyurusu](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

Yükleme ve Yardım

- ASP.NET MVC 3 yüklemesi kullanılarak [Web Platformu Yükleyicisi'ni (önerilir)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- ASP.NET MVC 3 yüklemesi kullanılarak [yükleyici çalıştırılabilir](https://go.microsoft.com/fwlink/?LinkID=208140)
- Yükleme [Visual Studio 11 Geliştirici önizlemesi için ASP.NET MVC 3](https://go.microsoft.com/fwlink/?LinkID=208140)
- Okuma [ASP.NET MVC 3 öğretici giriş](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Yardım alın ve ASP.NET MVC 3'te ele [forumları](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 genel bakış

ASP.NET MVC 3, her ikisi de kodunuzu basitleştiren ve derin genişletilebilirlik izin harika özellikler ekleme, ASP.NET MVC 1 ve 2 oluşturur. Bu konu aşağıdaki bölümlere düzenlenmiş bu sürümde, dahil edilen yeni özelliklerin çoğunu bir bakış sağlar:

- [Genişletilebilir yapı İskelesi MvcScaffold Tümleştirmesi ile](#BM_MvcScaffolding)
- [HTML 5 etkin proje şablonları](#BM_HTML5)
- [Razor görüntüleme altyapısı](#BM_TheRazorViewEngine)
- [Birden çok görünüm altyapıları için destek](#BM_Support_for_Multiple_View_Engines)
- [Denetleyici geliştirmeleri](#BM_Controller_Improvements)
- [JavaScript ve Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Model doğrulama geliştirmeleri](#BM_Model_Validation_Improvements)
- [Bağımlılık ekleme geliştirmeleri](#BM_Dependency_Injection_Improvements)
- [Diğer yeni özellikler](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Genişletilebilir yapı İskelesi MvcScaffold Tümleştirmesi ile

Yeni İskele sistem almak ve framework için tamamen yeni başladıysanız üretken kullanmaya başlamak için deneyimli ise ortak geliştirme görevleri otomatik hale getirmek ve gerçekleştirmekte olduğunuz zaten biliyor daha kolay hale getirir.

Bu yeni NuGet tarafından desteklenen *iskele* adlı paketi **MvcScaffolding**. "İskele" çok sayıda yazılım teknolojilere "hızlı bir şekilde cihazı yazılımınızı temel bir özetini oluşturma düzenleyin ve özelleştirme" anlamına için kullanılan terim. ASP.NET MVC için oluşturuyoruz yapı iskelesi paket çeşitli senaryolarda büyük ölçüde yararlıdır:

- **İlk kez ASP.NET MVC öğrenme varsa**, düzenlemek ve sizin ihtiyaçlarınıza göre uyum bazı yararlı çalışan kodu almak için hızlı bir şekilde verdiğinden. Boş bir sayfanın arayan ve hiçbir fikir nerede başlatılacağını sahip trauma kaydeder!
- **ASP.NET MVC iyi bildiğiniz ve bazı yeni eklenti teknolojisi artık keşfetme** bir nesne ilişkisel Eşleyici, bir görünüm altyapısı, bir sınama kitaplığı gibi vb. Oluşturucusu bu teknoloji, ayrıca bir yapı iskelesi paketi için oluşturmuş olabileceğiniz olduğundan.
- **Çalışmanızı benzer sınıfları veya bazı dosyaları tekrar tekrar oluşturmayı içerir,**, test armatürleri, dağıtım betikleri veya başka bir istediğiniz çıktı özel scaffolders oluşturabilirsiniz. Takımınızdaki herkes, özel scaffolders de kullanabilirsiniz.

MvcScaffolding diğer özellikleri içerir:

- C# ve VB projelerinin desteği
- Razor ve ASPX motorları görüntülemek için destek
- ASP.NET MVC alanlarına iskele kurma ve özel görünüm düzenleri/yöneticileri kullanarak destekler
- T4 şablonları düzenleyerek çıkış kolayca özelleştirebilirsiniz
- Özel PowerShell mantık ve özel T4 şablonları kullanarak tamamen yeni scaffolders ekleyebilirsiniz. Bu (ve izin verdiğiniz bunları herhangi bir özel parametre) otomatik olarak Konsolu sekme tamamlama listede görüntülenir.
- Farklı teknolojiler için ek scaffolders içeren NuGet paketlerini alabilirsiniz (örn., yoktur bir, kavram bir LINQ-SQL için şimdi) karıştırmak ve bunları birlikte eşleştirmesi

ASP.NET MVC 3 araçları güncelleştirme harika Visual Studio bu yapı iskelesi sistem gibi destekler:

- Denetleyici iletişim artık tam otomatik iskelesini oluşturma, okuma, güncelleştirme ve silme denetleyici eylemleri ve karşılık gelen görünümleri destekler ekleyin. Varsayılan olarak, bu veri erişim EF Code First kullanarak kodu iskelesini kurar.
- Denetleyici iletişim destekler eklemek *Genişletilebilir iskelesini kurar* NuGet paketleri gibi *MvcScaffolding*. Bu, özel iskelesini kurar, böylece inclined varsa iskelesini kurar NHibernate veya hatta JET gibi diğer veri erişim teknolojileri için ODBCDirect ile oluşturmanıza izin iletişim halinde takma sağlar!

ASP.NET MVC 3'te yapı İskelesi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- Steve Sanderson'ın da dahil olmak üzere serisi, gönderin: 

    1. [Giriş: ASP.NET MVC 3 projenizi MvcScaffolding paket İskele](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standart kullanımı: tipik kullanım durumları ve seçenekleri](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Bir-çok ilişkileri](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Yapı iskelesi eylemleri ve birim testleri](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [T4 şablonları geçersiz kılma](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Bu post: özel scaffolders oluşturma](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Kendi PDC 2010 oturumundan Scott Hanselman'ın post [Microsoft "Adlandırılmamış paket, arama Web Love" ile bir Blog oluşturma](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 proje şablonları

Yeni Proje iletişim kutusu proje şablonları onay kutusunu etkinleştir HTML 5 sürümlerini içerir. Bu şablonlar, alt düzey tarayıcılarda HTML 5 ve CSS 3 için Uyumluluk desteği sağlamak için Modernizr 1.7 yararlanın.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor görüntüleme altyapısı

ASP.NET MVC 3, aşağıdaki avantajları sunar Razor adlı yeni bir görünüm altyapısı ile birlikte gelir:

- Razor sözdizimi temiz ve kısa, tuş vuruşlarınızı en az sayıda istemektir.
- C# ve Visual Basic gibi mevcut diller bağlı olduğu, kısmen öğrenmek Razor kolaydır.
- Visual Studio Razor sözdizimi için IntelliSense ve kod renklendirme içerir.
- Razor görünümleri uygulamayı çalıştırın veya bir web sunucusunu başlatmaya gerek olmadan test birim olabilir.

Bazı yeni Razor özellikler şunları içerir:

- `@model` görünüme geçirilen türünü belirtmek için söz dizimi.
- `@* *@` Açıklama sözdizimi.
- Varsayılanları belirtme olanağı (gibi `layoutpage`) tüm site için bir kez.
- `Html.Raw` HTML kodlaması olmadan metin görüntüleme yöntemi.
- Birden çok görünüm arasında kod paylaşımını desteği (*\_viewstart.cshtml* veya  *\_viewstart.vbhtml* dosyaları).

Razor ayrıca aşağıdaki gibi yeni HTML Yardımcıları içerir:

- `Chart`. Grafik denetimi ASP.NET 4'te aynı özellikleri sunan bir grafik oluşturur.
- `WebGrid`. Disk belleği ve sıralama işlevselliği ile tam veri kılavuzu işler.
- `Crypto`. Güvenlik ve parolaların karma algoritmaları düzgün bir şekilde oluşturmak için karma kullanır.
- `WebImage`. Bir görüntü oluşturur.
- `WebMail`. Bir e-posta iletisi gönderir.

Razor hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Razor Tanıtımı Scott Guthrie'nın blog gönderisi](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie'nın blog gönderisine Tanıtımı @model anahtar sözcüğü](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie'nın blog gönderisine ile tanışın Razor düzenleri](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API hızlı başvuru](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Birden çok görünüm altyapıları için destek

**Görünüm Ekle** ASP.NET MVC 3 iletişim kutusunda, çalışmak istediğiniz görünüm altyapısı seçmenize olanak sağlar ve **yeni proje** iletişim kutusu bir proje için varsayılan görünüm altyapısı belirtmenize olanak sağlar. Web Forms görünüm altyapısı (ASPX), Razor ya da bir açık kaynak görünüm altyapısı gibi seçebilirsiniz [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), veya [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Denetleyici geliştirmeleri

### <a name="global-action-filters"></a>Genel eylem filtreleri

Bazen bir eylem yöntemi çalıştırılmadan önce veya bir eylem yöntemi çalıştıktan sonra mantığı yapmak istiyorsunuz. Bunu desteklemek için eylem filtrelerini ASP.NET MVC 2 sağlanan. Eylem filtreleri eylem öncesi ve sonrası eylem davranışı belirli denetleyici eylem yöntemleri eklemek için bir bildirime sağlayan özel öznitelikleridir. Ancak, bazı durumlarda, tüm eylem yöntemleri için geçerlidir ön eylem veya eylem sonrası davranışını belirtmek isteyebilirsiniz. MVC 3 genel filtreler ekleyerek belirtmenize olanak sağlar `GlobalFilters` koleksiyonu. Genel eylem filtreleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [MVC 3 Önizleme Scott Guthrie'nın blogunda](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [ASP.NET MVC'de filtreleme](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Yeni "Görünüm paketi" özelliği

MVC 2 denetleyicileri desteği bir `ViewData` veri geç bağlama sözlüğü API kullanarak bir görünüm şablonu iletmek olanak tanıyan özellik. MVC 3'te biraz daha basit sözdizimi ile de kullanabilirsiniz `ViewBag` aynı amaca gerçekleştirmek için özellik. Örneğin, yazma yerine `ViewData["Message"]="text"`, yazabileceğiniz `ViewBag.Message="text"`. Kullanmak için hiçbir kesin türü belirtilmiş sınıflarını tanımla gerekmez `ViewBag` özelliği. Dinamik bir özellik olduğundan, bunun yerine, yalnızca alabilirsiniz veya özelliklerini ayarlama ve bunları çalışma zamanında dinamik olarak çözer. Dahili olarak, `ViewBag` özellikleri ad/değer çiftleri olarak depolanır `ViewData` sözlük. (Not: MVC 3, çoğu yayın öncesi sürümlerini içinde `ViewBag` özelliği adlandırılması `ViewModel` özelliğini.)

### <a name="new-actionresult-types"></a>Yeni "ActionResult" türleri

Aşağıdaki `ActionResult` türleri ve karşılık gelen yardımcı yöntemler yeni veya geliştirilmiş MVC 3'te:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). İstemciye bir 404 HTTP durum kodu döndürür.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Geçici bir yeniden yönlendirme (HTTP 302 durum kodu) veya bir Boolean parametresiyle bağlı olarak kalıcı bir yeniden yönlendirme (301 HTTP durum kodu) döndürür. Bu değişiklik, birlikte [denetleyicisi](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) sınıfı şimdi yeniden yönlendirmeleri kalıcı olarak gerçekleştirmek için üç yöntem vardır: `RedirectPermanent`, `RedirectToRoutePermanent`, ve `RedirectToActionPermanent`. Bu yöntemler bir örneğini döndürmek `RedirectResult` ile `Permanent` özelliğini `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Bir kullanıcı tarafından belirtilen HTTP durum kodu döndürür.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript ve Ajax geliştirmeleri

Varsayılan olarak, MVC 3'te Ajax ve doğrulama Yardımcıları bir örtük JavaScript yaklaşımı kullanın. Örtük JavaScript'i satır içi JavaScript HTML'e injecting önler. Bu daha küçük ve daha az HTML yığın yapar ve takas veya JavaScript kitaplıklarını özelleştirmek kolaylaştırır. MVC 3'te doğrulama Yardımcıları de `jQueryValidate` varsayılan eklentisi. MVC 2 davranışı istiyorsanız, örtük JavaScript kullanarak devre dışı bırakabilirsiniz bir *web.config* dosya ayarı. JavaScript ve Ajax iyileştirmeleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Wikipedia sitesinde örtük JavaScript için temel giriş](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson'ın örtük JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson'ın örtük JavaScript doğrulama Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Razor ve örtük JavaScript ile MVC 3 uygulaması oluşturma](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (ASP.NET sitesindeki öğretici)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Varsayılan olarak etkin istemci tarafı doğrulama

MVC önceki sürümlerde açıkça çağırmanız gerekir. `Html.EnableClientValidation` istemci tarafı doğrulamasını etkinleştirmek için bir görünüm yönteminden. İstemci tarafı doğrulama varsayılan olarak etkin olduğundan MVC 3'te bu artık gerekli değildir. (, Bu ayarı kullanarak devre dışı bırakabilirsiniz *web.config* dosyası.)

Sırayla çalışması istemci tarafı doğrulama için hala uygun jQuery ve jQuery doğrulama kitaplıklarını sitenizdeki başvuru gerekir. Bu kitaplıklar, kendi sunucu üzerinde barındırmak veya bunları CDN'ler Microsoft veya Google gibi içerik teslim ağı (CDN) başvurun.

### <a name="remote-validator"></a>Uzak Doğrulayıcı

ASP.NET MVC 3 destekleyen yeni [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) jQuery doğrulama Tak bileşenini yararlanmak sağlayan sınıf kullanıcının uzak Doğrulayıcı desteği. Bu otomatik olarak sunucu tarafı sunucuda yalnızca yapılabilir doğrulama mantığını gerçekleştirmek için tanımladığınız özel bir yöntemi çağırmak istemci tarafı doğrulama kitaplığını sağlar.

Aşağıdaki örnekte, `Remote` özniteliği belirtir istemci doğrulama adlı bir eylem çağıracak `UserNameAvailable` üzerinde `UsersController` doğrulamakta sınıfı `UserName` alan.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Aşağıdaki örnek, ilgili denetleyicisi gösterir.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Nasıl kullanılacağı hakkında daha fazla bilgi için `Remote` özniteliği için bkz: [nasıl yapılır: ASP.NET MVC uygulama uzak doğrulama](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) MSDN Kitaplığı'nda.

### <a name="json-binding-support"></a>JSON bağlama desteği

ASP.NET MVC 3 JSON olarak kodlanmış veri almak ve model, eylem-yöntem parametrelerini bağlamak eylem yöntemlerini etkinleştirir yerleşik JSON bağlama destek içerir. Bu özellik, istemci şablonları ve veri bağlama ilgili senaryolarda kullanışlıdır. (İstemci şablonlar, biçimlendirme ve istemci üzerinde yürütme şablonları kullanarak tek bir veri öğesi veya veri öğeleri kümesi görüntülemek etkinleştirir.) MVC 3 JSON veri gönderip eylem yöntemleri ile sunucu üzerindeki istemci şablonları kolayca bağlanmanızı sağlar. JSON bağlama desteği hakkında daha fazla bilgi için bkz: **JavaScript ve AJAX iyileştirmeleri** bölümünü [Scott Guthrie'nın MVC 3 Önizleme blog gönderisi](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Model doğrulama geliştirmeleri

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations" meta veri öznitelikleri

ASP.NET MVC 3 destekleyen `DataAnnotations` meta veri öznitelikleri gibi `DisplayAttribute`.

### <a name="validationattribute-class"></a>"ValidationAttribute" sınıfı

`ValidationAttribute` Sınıfı yeni desteklemek için .NET Framework 4'de Geliştirilmiş `IsValid` aşırı hangi nesne doğrulandığı gibi geçerli doğrulama bağlamını hakkında daha fazla bilgi sağlar. Bu, burada başka bir model özelliğine göre geçerli değeri doğrulamak için daha zengin senaryolara olanak sağlar. Örneğin, yeni `CompareAttribute` özniteliği bir modelin iki özelliklerinin değerlerini karşılaştırmanıza olanak sağlar. Aşağıdaki örnekte, `ComparePassword` özelliği eşleşmelidir `Password` geçerli olması için alan.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Doğrulama arabirimleri

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) arabirimi model düzeyi doğrulama gerçekleştirmenize olanak sağlar ve doğrulama durumuna genel modelinin veya modeli içindeki iki özellikleri arasında özel hata iletileri sağlamak sağlar . MVC 3 şimdi hatalarından alır `IValidatableObject` arabirim alanları yerleşik HTML form Yardımcıları kullanarak bir görünüm içindeki model bağlama ve otomatik olarak bayrakları veya vurgular etkilenen olduğunda.

[Iclientvalidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) arabirimi çalışma zamanında bir doğrulayıcının istemci doğrulamayı olup olmadığını bulmak ASP.NET MVC sağlar. Bu arabirim, böylece doğrulama çerçeveleri çeşitli ile tümleştirilebilir tasarlanmıştır.

Doğrulama arabirimleri hakkında daha fazla bilgi için bkz: **Model doğrulama geliştirmeleri** bölümünü [Scott Guthrie'nın MVC 3 Önizleme blog gönderisi](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Ancak, "IValidateObject" başvurusunu günlüğündeki "IValidatableObject" olması gerektiğini unutmayın.)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Bağımlılık ekleme geliştirmeleri

ASP.NET MVC 3, bağımlılık ekleme (dı) uygulamak için ve bağımlılık ekleme veya denetimi tersine çevirme (IOC) kapsayıcı ile tümleştirmek için daha iyi destek sağlar. Aşağıdaki alanlarda dı için destek eklenmiştir:

- (Kaydetme ve denetleyici üreteçlerini, denetleyicileri injecting injecting) denetleyicileri.
- Görünümler (kaydetme ve görünüm altyapısı görünüm sayfalarına bağımlılıkları injecting injecting).
- (Bulma ve filtreleri injecting) eylem filtreleri.
- (Kaydetme ve injecting) model bağlayıcıları.
- Model doğrulama sağlayıcılarının (kaydetme ve injecting).
- Model meta veri sağlayıcıları (kaydetme ve injecting).
- Değer sağlayıcıları (kaydetme ve injecting).

MVC 3'ü destekler [genel hizmet bulucu](https://github.com/unitycontainer/commonservicelocator) kitaplığı ve kitaplığın destekleyen herhangi bir dı kapsayıcısına `IServiceLocator` arabirimi. Ayrıca yeni bir destekler `IDependencyResolver` dı çerçeveleri tümleştirmek kolaylaştırır arabirimi.

MVC 3'te dı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Hizmet konumu blog gönderilerine Brad Wilson'ın dizi](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Diğer yeni özellikler

### <a name="nuget-integration"></a>NuGet tümleştirme

ASP.NET MVC 3 otomatik olarak yükler ve onun kurulumunun bir parçası NuGet sağlar. NuGet bulmak, yükleme ve .NET kitaplıkları ve Araçlar projelerinizde kullanma kolaylaştıran ücretsiz açık kaynak paketi yöneticisidir. (ASP.NET Web Forms ve ASP.NET MVC dahil) tüm Visual Studio Proje türleri ile çalışır.

NuGet paketini kendi kitaplıkları ve çevrimiçi Galerisi'nde kaydetmek için açık kaynak projeleri (örneğin, projeler Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks ve Elmah gibi) sürdürmek geliştiriciler sağlar. Bu kitaplıklar birini paket bulmak ve üzerinde çalıştıkları projelerinde yüklemek için kullanmak isteyen .NET geliştiricileri için sonra kolaydır.

NuGet güncelleştirilebilir; bu nedenle ASP.NET 3 araçları güncelleştirme ile JavaScript kitaplıklarını önceden yüklenmiş NuGet paketleri, proje şablonları içerir. Entity Framework Code First da bir NuGet paketi olarak ön yüklenir.

NuGet hakkında daha fazla bilgi için bkz: [NuGet belgelerine](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Kısmi sayfa çıktı önbelleği

ASP.NET MVC 1 sürümünden bu yana tam sayfa yanıtların çıktı önbelleği desteklenen. MVC 3 ayrıca kısmi sayfa çıktı önbelleği, kolayca önbellek bölgeler veya bir yanıt parçalarını sağlayan destekler. Önbelleğe alma hakkında daha fazla bilgi için bkz: **kısmi sayfa çıktı önbelleği** bölümünü [Scott Guthrie'nın blog gönderisi MVC 3 Sürüm Adayı konusunda](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) ve **alt eylem çıktı önbelleği** bölümünü [MVC 3 Sürüm Notları](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>İstek doğrulama üzerinde ayrıntılı denetim

ASP.NET MVC otomatik olarak XSS ve HTML yerleştirme saldırılarına karşı korumaya yardımcı olur yerleşik istek doğrulama sahiptir. Ancak, açıkça devre dışı bırakma isteği doğrulama, bazen istediğiniz gelmesi gibi HTML (örneğin, blog girdileri veya CMS içerik) içerik sonrası kullanıcıların izin vermek istiyor. Artık ekleyebilirsiniz bir [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) özniteliği modellerine veya model bağlama sırasında özelliğe temelinde istek doğrulamanın devre dışı bırakmak için modelleri görüntüleyin. İstek doğrulama hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- **Örtük JavaScript ve doğrulama** bölümüne [Scott Guthrie'nın blog gönderisi MVC 3 Sürüm Adayı konusunda](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Genişletilebilir "Yeni Proje" iletişim kutusu

ASP.NET MVC 3'te proje şablonları, görünüm altyapıları ekleyebilir ve birim testi projesi çerçeveler **yeni proje** iletişim kutusu.

### <a name="template-scaffolding-improvements"></a>Şablonu iskele kurmayı geliştirmeleri

ASP.NET MVC 3 yapı iskelesi şablonları modellerinde birincil anahtar özellikleri ve bunları uygun şekilde daha MVC eski sürümlerinde işleme daha iyi iş yapın. (Örneğin, yapı iskelesi şablonları artık birincil anahtarı düzenlenebilir form alanı olarak iskele kurulmuş değil, emin olun.)

Varsayılan olarak, oluşturma ve düzenleme iskelesini kurar şimdi kullanmak `Html.EditorFor` yerine Yardımcısı `Html.TextBoxFor` Yardımcısı. Bu formun veri modelinde bulunan meta verileri desteği artırır ek açıklama öznitelikleri **Görünüm Ekle** iletişim kutusu görünümü oluşturur.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>"Html.LabelFor" ve "Html.LabelForModel" için yeni aşırı yüklemeleri

Yeni yöntemi aşırı yüklemeleri için eklenmiştir `LabelFor` ve `LabelForModel` yardımcı yöntemler. Yeni aşırı etiketi geçersiz kılmak etkinleştirin.

### <a name="sessionless-controller-support"></a>Oturumsuz denetleyici desteği

Oturum durumunu kullanmak için bir denetleyici sınıfı istediğinizi ve öyleyse belirtebilirsiniz ASP.NET MVC 3'te mi oturum durumu olmalıdır okuma/yazma veya salt okunur. Oturumsuz denetleyicisi desteği hakkında daha fazla bilgi için bkz: [MVC 3 Sürüm Notları](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Yeni "AdditionalMetadataAttribute" sınıfı

Kullanabileceğiniz [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) özniteliğini doldurmak için `ModelMetadata.AdditionalValues` model özelliğine sözlüğü. Örneğin, bir görünüm modeli yalnızca yönetici görüntülenmesi gereken bir özellik varsa, aşağıdaki örnekte gösterildiği gibi bu özellik açıklama ekleyebilirsiniz:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Bir ürün görünüm modeli işlendiğinde bu meta veriler herhangi bir görüntü veya Düzenleyicisi şablonu için kullanılabilir hale getirilir. Bu meta veri bilgilerini yorumlamak için size bağlıdır.

### <a name="accountcontroller-improvements"></a>AccountController geliştirmeleri

AccountController Internet proje şablonu olarak büyük ölçüde geliştirilmiştir.

### <a name="new-intranet-project-template"></a>Yeni Intranet proje şablonu

Yeni bir Intranet proje şablonu dahil olduğu Windows kimlik doğrulamasını etkinleştirir ve AccountController kaldırır.
