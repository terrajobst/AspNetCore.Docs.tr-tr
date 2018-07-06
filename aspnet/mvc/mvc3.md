---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (Nisan 2011 içerir araçları güncelleştirme) ASP.NET MVC 3, tanınmış tasarım desenini kullanarak ölçeklenebilir, standartlara dayanan web uygulamaları oluşturmaya yönelik bir çerçevedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: ed3bf81943ac2ffa143cf169c87b3012269d9e77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375797"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(Nisan 2011 içerir araçları güncelleştirme)*
> 
> ASP.NET MVC 3, tanınmış tasarım desenleri ve ASP.NET ve .NET Framework gücünü kullanarak ölçeklenebilir, standartlara dayanan web uygulamaları oluşturmaya yönelik bir çerçevedir.
> 
> ASP.NET MVC bugün kullanmaya başlamak için 2 ile yan yana yükler!
> 
> İndirme [yükleyiciyi buradan](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>En iyi Özellikler

- NuGet aracılığıyla Genişletilebilir tümleşik yapı İskelesi sistem
- HTML 5 etkin proje şablonları
- Yeni Razor görünüm altyapısı dahil olmak üzere ifadesel görünümleri
- Bağımlılık ekleme ve genel eylem filtreleri ile güçlü kancaları
- Örtük JavaScript ve jQuery doğrulaması JSON bağlama ile zengin JavaScript desteği
- *Tam özellik listesini okuma [aşağıda](#overview)*

## <a name="top-links"></a>En önemli

ASP.NET MVC 3 sürümündeki yenilikler

- Phil Haack: [ASP.NET MVC 3 piyasaya Sürüldü](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express ve Orchard yayımlanan - Microsoft Ocak Web sürümde bağlamı](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix yayın Duyurusu](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

Yükleme ve Yardım

- ASP.NET MVC 3 kullanılarak [Web Platformu Yükleyicisi'ni (önerilir)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- ASP.NET MVC 3 kullanılarak [yürütülebilir yükleyicisi](https://go.microsoft.com/fwlink/?LinkID=208140)
- Yükleme [Visual Studio 11 Developer Preview için ASP.NET MVC 3](https://go.microsoft.com/fwlink/?LinkID=208140)
- Okuma [ASP.NET MVC 3 öğreticiye giriş](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Yardım alın ve ASP.NET MVC 3'te tartışın [forumları](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 genel bakış

ASP.NET MVC 3, ASP.NET MVC 1 ve 2, her ikisi de kodunuzu basitleştirin ve daha ayrıntılı genişletilebilirlik sağlayan harika özellikler eklemek oluşturur. Bu konu aşağıdaki bölümlere düzenlenen bu sürümde, dahil edilen yeni özelliklerin çoğunu bir genel bakış sağlar:

- [Genişletilebilir yapı İskelesi MvcScaffold tümleştirme](#BM_MvcScaffolding)
- [HTML 5 etkin proje şablonları](#BM_HTML5)
- [Razor görünüm altyapısı](#BM_TheRazorViewEngine)
- [Birden çok görünüm altyapıları için destek](#BM_Support_for_Multiple_View_Engines)
- [Denetleyici geliştirmeleri](#BM_Controller_Improvements)
- [JavaScript ve Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Model doğrulama geliştirmeleri](#BM_Model_Validation_Improvements)
- [Bağımlılık ekleme iyileştirmeleri](#BM_Dependency_Injection_Improvements)
- [Diğer yeni özellikler](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Genişletilebilir yapı İskelesi MvcScaffold tümleştirme

Yeni yapı İskelesi sistem toplayın ve framework, tamamen yeni, üretken bir şekilde kullanmaya başlamak için ve deneyimli değilseniz yaygın geliştirme görevlerini otomatikleştirin ve ne yaptığınızı zaten biliyor kolaylaştırır.

Bu yeni NuGet tarafından desteklenen *yapı iskelesi* adlı paketi **MvcScaffolding**. Terimi "İskele kurma özelliği" çok sayıda yazılım teknolojilere "hızlı bir şekilde gerçekleştirebileceğiniz yazılımınızın temel bir anahat oluşturma ardından düzenleme ve özelleştirme" anlamına gelir için kullanılır. ASP.NET MVC için oluşturuyoruz yapı iskelesi paketi büyük ölçüde çeşitli senaryolarda yararlıdır:

- **ASP.NET MVC ilk kez öğreniyoruz varsa**, Düzenle ve ihtiyaçlarınıza göre uyarlama bazı yararlı çalışan kodu almak için hızlı bir yolunu sunar. Boş bir sayfa arama ve hiçbir fikriniz nereden başlayacağınızı sahip trauma kaydeder!
- **ASP.NET MVC iyi biliyor ve bazı yeni bir eklenti teknoloji artık keşfettiğiniz** gibi bir nesne ilişkisel eşleyicidir, bir görünüm altyapısı, bir sınama kitaplığı Oluşturucusu bu teknoloji, ayrıca bir yapı iskelesi paket için oluşturmuş olabilir çünkü vb.
- **Benzer sınıflar veya tür dosyaları tekrar tekrar oluşturma iş içerir,**, test armatürleri, dağıtım betikleri veya istediğiniz başka çıktı özel platformları oluşturabilirsiniz. Takımınızdaki herkesin, özel platformları de kullanabilirsiniz.

MvcScaffolding diğer özellikleri şunlardır:

- C# ve VB projeleri için destek
- Razor ve ASPX altyapıları görüntülemek için destek
- ASP.NET MVC alanlarına yapı iskelesini oluşturmak ve özel görünüm düzenleri/yöneticileri kullanarak destekler
- T4 şablonları düzenleyerek çıkış kolayca özelleştirebilirsiniz
- Tamamen yeni platformları özel PowerShell mantık ile özel T4 şablonları ekleyebilirsiniz. Bu (ve izin verdiğiniz bunları herhangi bir özel parametre) otomatik olarak Konsolu sekme tamamlama listesinde görünür.
- Farklı teknolojiler için ek platformları içeren NuGet paketleri elde edebilirsiniz (örneğin, var. bir kavram kanıtı bir LINQ to SQL için artık) ve karıştırın ve bunları birlikte eşleştirmesi

ASP.NET MVC 3 araçları güncelleştirme, bu yapı iskelesi sistem için harika bir Visual Studio desteği gibi içerir:

- Denetleyici iletişim oluşturma, okuma, güncelleştirme ve silme denetleyici eylemlerini ve karşılık gelen görünümlerinin tam otomatik bir yapı iskelesi artık destekliyor ekleyin. Varsayılan olarak, bu EF Code First kullanarak veri erişim kodu iskele oluşturulduğunu.
- Ekleme denetleyicisi iletişim kutusunu destekler *Genişletilebilir iskelesini kurar* NuGet paketleri gibi *MvcScaffolding*. Bu nedenle yazmaya, NHibernate veya hatta JET gibi diğer veri erişim teknolojileri için iskelesini kurar ODBCDirect ile oluşturmak izin iletişim özel iskelesini kurar, takma sağlar!

ASP.NET MVC 3'te iskele kurma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- Steve Sanderson'ın da dahil olmak üzere seri gönderin: 

    1. [Giriş: ASP.NET MVC 3 projenizle MvcScaffolding paket iskelesini](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standart Kullanım: tipik kullanım durumları ve seçenekleri](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Bire çok ilişkileri](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Yapı iskelesi Eylemler ve birim testleri](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [T4 şablonları geçersiz kılma](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Bu gönderinin: özel platformları oluşturma](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman'ın post kendi PDC 2010 oturumundan ["Adsız paket, arama Web Love" Microsoft ile bir Blog oluşturma](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 proje şablonları

Yeni Proje iletişim kutusu bir onay kutusu etkinleştirme HTML 5 sürümleri proje şablonları içerir. Bu şablonlar, HTML 5 ve CSS 3 alt düzey tarayıcılarda uyumluluk desteği sağlamak için Modernizr 1.7 yararlanın.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor görünüm altyapısı

ASP.NET MVC 3, aşağıdaki avantajları sağlamaktadır Razor adlı yeni bir görünüm altyapısı ile birlikte gelir:

- Razor sözdizimi temiz ve kısa, tuş vuruşlarınızı en az sayıda gerektirmektir.
- Razor, C# ve Visual Basic gibi mevcut dil bağlı olduğu, kısmen öğrenmek kolay bir işlemdir.
- Visual Studio, Razor sözdizimi için IntelliSense ve kod renklendirme içerir.
- Razor görünümleri, uygulamayı çalıştırın veya bir web sunucusunu başlatmak gerek kalmadan test birimi olabilir.

Bazı yeni Razor özellikleri şunlardır:

- `@model` görünüme geçirilen türü belirtmek için sözdizimi.
- `@* *@` Açıklama söz dizimi.
- Varsayılanları belirtme olanağı (gibi `layoutpage`) tüm bir site için bir kez.
- `Html.Raw` Yöntemi HTML kodlaması olmadan metin görüntülemek için bu.
- Birden çok görünüm arasında kod paylaşma desteği (*\_viewstart.cshtml* veya  *\_viewstart.vbhtml* dosyaları).

Razor, aynı zamanda aşağıdaki gibi yeni HTML Yardımcıları içerir:

- `Chart`. ASP.NET 4'te grafiği denetimi ile aynı özellikleri sunan bir grafik oluşturur.
- `WebGrid`. Veri Kılavuzu, sayfalama ve sıralama işlevselliği ile tam işler.
- `Crypto`. Karma algoritmaları düzgün bir şekilde oluşturmak için kullandığı salted ve parolaların karma.
- `WebImage`. Bir görüntü oluşturur.
- `WebMail`. Bir e-posta iletisi gönderir.

Razor hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Scott Guthrie'nin blog postası Razor ile tanışın](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie'nin blog gönderisi giriş @model anahtar sözcüğü](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie'nin blog gönderisini giriş Razor düzeni](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API hızlı başvurusu](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Birden çok görünüm altyapıları için destek

**Görünüm Ekle** iletişim kutusundaki ASP.NET MVC 3, çalışmak istediğiniz görünüm altyapısı seçmenize olanak sağlar ve **yeni proje** iletişim kutusunda bir proje için varsayılan görünüm altyapısı belirtmenize olanak sağlar. Web Forms görünüm Altyapısı'nı (ASPX), Razor veya bir açık kaynak görünüm altyapısı gibi seçebilirsiniz [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), veya [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Denetleyici geliştirmeleri

### <a name="global-action-filters"></a>Genel eylem filtreleri

Bazen mantıksal bir eylem yöntemi çalıştıktan önce veya bir eylem yöntemi çalıştıktan sonra gerçekleştirmek istediğiniz. Bunu desteklemek için ASP.NET MVC 2'eylem filtrelerini sağladı. Eylem filtreleri eylem öncesi ve sonrası eylem davranışı için belirli bir denetleyici eylem yöntemleri eklemek için bir bildirime sağlayan özel öznitelikleridir. Ancak, bazı durumlarda tüm eylem yöntemlerine uygulanır ön eylem veya eylem sonrası davranışını belirtmek isteyebilirsiniz. MVC 3 genel filtreler ekleyerek belirtmenize olanak tanır `GlobalFilters` koleksiyonu. Genel eylem filtreleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Scott Guthrie'nin blogunu MVC 3 önizlemede](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [ASP.NET MVC'de filtreleme](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Yeni "Görünüm paketini" özelliği

MVC 2 denetleyicileri desteği bir `ViewData` geç bağlanan sözlüğü API kullanarak bir görünüm şablonu veri iletmek olanak tanıyan özellik. MVC 3'te, biraz daha basit söz dizimi ile de kullanabilirsiniz `ViewBag` aynı amaca gerçekleştirmek için özellik. Örneğin, yazmak yerine `ViewData["Message"]="text"`, yazabileceğiniz `ViewBag.Message="text"`. Kullanmak istediğiniz kesin türü belirtilmiş sınıflarını tanımlamak gerek kalmayacak `ViewBag` özelliği. Dinamik bir özelliği olduğundan, bunun yerine, yalnızca alabilirsiniz veya özelliklerini ayarlama ve bunları çalışma zamanında dinamik olarak çözer. Dahili olarak `ViewBag` özellikleri içinde ad/değer çiftleri olarak depolanır `ViewData` sözlüğü. (Not: MVC 3, birçok yayın öncesi sürümlerinde `ViewBag` özelliği adlandırılmıştı `ViewModel` özellik.)

### <a name="new-actionresult-types"></a>Yeni "ActionResult" türleri

Aşağıdaki `ActionResult` türleri ve karşılık gelen yardımcı yöntemler yeni veya iyileştirilmiş MVC 3'te:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). İstemciye bir 404 HTTP durum kodu döndürür.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Geçici bir yeniden yönlendirme (302 HTTP durum kodu) veya bağlı bir Boolean parametre olarak kalıcı bir yeniden yönlendirme (301 HTTP durum kodu) döndürür. Bu değişiklikle birlikte [denetleyicisi](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) sınıfı artık yeniden yönlendirmeleri kalıcı olarak gerçekleştirmek için üç yöntem vardır: `RedirectPermanent`, `RedirectToRoutePermanent`, ve `RedirectToActionPermanent`. Bu yöntemler bir örneğini döndürür `RedirectResult` ile `Permanent` özelliğini `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Bir kullanıcı tarafından belirtilen HTTP durum kodu döndürür.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript ve Ajax geliştirmeleri

Varsayılan olarak, Ajax ve doğrulama Yardımcıları MVC 3'te bir örtük JavaScript yaklaşımı kullanın. Örtük JavaScript'i HTML'e satır içi JavaScript ekleme önler. Bu HTML daha küçük ve daha az dağınık hale getirir ve takas veya JavaScript kitaplıklarını özelleştirmek kolaylaştırır. MVC 3'te doğrulama Yardımcıları de `jQueryValidate` varsayılan eklenti. MVC 2 davranışı istiyorsanız, örtük JavaScript'i kullanarak devre dışı bırakabilirsiniz bir *web.config* dosyası ayarı. JavaScript ve Ajax iyileştirmeleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Örtük JavaScript'i Wikipedia sitesinde temel giriş](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson'ın örtük JavaScript sonrası](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson'ın örtük JavaScript doğrulama sonrası](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Razor ve Unobtrusive JavaScript ile MVC 3 uygulaması oluşturma](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (ASP.NET sitesindeki öğretici)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Varsayılan olarak etkin istemci tarafı doğrulama

MVC önceki sürümlerinde, açıkça çağırmak ihtiyacınız `Html.EnableClientValidation` istemci tarafı doğrulamasını etkinleştirmek için bir görünümden yöntemi. Varsayılan olarak istemci tarafı doğrulamanın etkin olup MVC 3'te artık gerekli olmasıdır. (, Bu ayarı kullanarak devre dışı bırakabilirsiniz *web.config* dosyası.)

Sırayla çalışması istemci tarafı doğrulama için yine de uygun jQuery ve jQuery doğrulama kitaplıkları sitenizde başvuru gerekir. Bu kitaplıklar kendi sunucunuzda ana bilgisayar veya bunları CDN'ler Microsoft veya Google gibi bir içerik teslim ağı (CDN) başvuru.

### <a name="remote-validator"></a>Uzak Doğrulayıcısını

ASP.NET MVC 3 destekler yeni [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) Tak açma jQuery doğrulaması avantajlarından yararlanmanızı sağlar sınıfını kullanıcının uzak doğrulayıcısını desteği. Bu, otomatik olarak sunucu tarafı sunucuda yalnızca yapılabilir doğrulama mantığını gerçekleştirmesi için tanımladığınız özel bir yöntem çağırmak istemci tarafı doğrulama kitaplığını sağlar.

Aşağıdaki örnekte, `Remote` özniteliği belirtir istemci doğrulama adlı bir eylem çağırır `UserNameAvailable` üzerinde `UsersController` doğrulayabilmek sınıfı `UserName` alan.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Aşağıdaki örnek, karşılık gelen denetleyicisi gösterilir.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Nasıl kullanılacağı hakkında daha fazla bilgi için `Remote` özniteliği için bkz: [nasıl yapılır: ASP.NET mvc'de uygulama uzak doğrulama](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) MSDN Kitaplığı'nda.

### <a name="json-binding-support"></a>JSON bağlama desteği

ASP.NET MVC 3, JSON olarak kodlanmış veri almak ve model, eylem yöntemi parametrelerine bağlamak eylem yöntemleri sağlayan yerleşik JSON bağlama desteği içerir. Bu özellik, istemci şablonları ve veri bağlama ile ilgili senaryolarda yararlıdır. (İstemci şablonlar, biçimlendirme ve istemci üzerinde yürütülen şablonları kullanarak bir tek veri öğesi veya veri öğeleri kümesi görüntülemek etkinleştirir.) MVC 3, JSON verilerini gönderip eylem yöntemleri ile sunucuda istemci şablonları kolayca bağlamanızı sağlar. JSON bağlama desteği hakkında daha fazla bilgi için bkz. **JavaScript ve AJAX geliştirmeleri** bölümünü [Scott Guthrie'nin MVC 3 Preview blog gönderisi](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Model doğrulama geliştirmeleri

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations" meta veri öznitelikleri

ASP.NET MVC 3 destekler `DataAnnotations` meta veri öznitelikleri gibi `DisplayAttribute`.

### <a name="validationattribute-class"></a>"ValidationAttribute" sınıfı

`ValidationAttribute` Sınıfı yeni bir desteklemek için .NET Framework 4 ile geliştirilmiş `IsValid` hangi nesne Doğrulanmakta olan gibi geçerli doğrulama bağlamı hakkında daha fazla bilgi sağlayan bir aşırı yükleme. Bu, burada başka bir model özelliğine göre geçerli değeri doğrulamak için daha zengin senaryolarını olanaklı kılar. Örneğin, yeni `CompareAttribute` özniteliği bir modelin iki özelliklerin değerlerini karşılaştırmak olanak tanır. Aşağıdaki örnekte, `ComparePassword` özelliği eşleşmelidir `Password` geçerli olması için alan.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Doğrulama arabirimleri

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) arabirimi model düzeyinde doğrulama yapmanıza olanak tanır ve doğrulama durumuna genel modeli ya da model içindeki iki özellik arasında özel hata iletileri vermenizi sağlar . MVC 3 hatalarından artık alır `IValidatableObject` arabirim alanları yerleşik HTML form Yardımcıları kullanarak bir görünüm içindeki model bağlama ve otomatik olarak bayrakları veya önemli etkilenen olduğunda.

[Iclientvalidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) ASP.NET MVC, çalışma zamanında bir doğrulayıcının istemci doğrulama desteği olup olmadığını keşfetmek arabirim sağlar. Bu arabirim, çeşitli doğrulama çerçeveleri ile tümleştirilebilir, böylece tasarlanmıştır.

Doğrulama arabirimleri hakkında daha fazla bilgi için bkz. **Model doğrulama geliştirmeleri** bölümünü [Scott Guthrie'nin MVC 3 Preview blog gönderisi](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Ancak, "IValidatableObject" başvuru "IValidateObject" blogunda olması gerektiğini unutmayın.)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Bağımlılık ekleme iyileştirmeleri

ASP.NET MVC 3, bağımlılık ekleme (dı) uygulama ve bağımlılık ekleme veya denetimi tersine çevirme (IOC) kapsayıcı ile tümleştirmek için daha iyi destek sağlar. Aşağıdaki alanlarda DI için destek eklenmiştir:

- Denetleyicileri (kaydetme ve ekleme denetleyicileri, denetleyici üreteçlerini ekleme).
- Görünümler (kaydetme ve görünüm altyapısı görünüm sayfalarına bağımlılıkları ekleme, ekleme).
- Eylem filtreleri (bulma ve ekleme filtreleri).
- Model bağlayıcıları (kaydetme ve ekleme).
- Model doğrulama sağlayıcılarının (kaydetme ve ekleme).
- Model meta veri sağlayıcıları (kaydetme ve ekleme).
- Değer sağlayıcıları (kaydetme ve ekleme).

MVC 3'ü destekler [genel hizmet bulucu](https://github.com/unitycontainer/commonservicelocator) kitaplığı ve kitaplığın destekleyen her DI kapsayıcı `IServiceLocator` arabirimi. Ayrıca yeni bir destekler `IDependencyResolver` arabirimi DI çerçeveleri tümleştirmeyi kolaylaştırır.

MVC 3'te DI hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Brad Wilson'ın dizi blog gönderilerine hizmet konumu](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Diğer yeni özellikler

### <a name="nuget-integration"></a>NuGet tümleştirme

ASP.NET MVC 3, otomatik olarak yükler ve NuGet kendi kurulumunun bir parçası etkinleştirir. NuGet, bulmak, yüklemek ve projelerinizde .NET kitaplıkları ve araçları kullanmak kolay bir ücretsiz açık kaynaklı paket yöneticisidir. (ASP.NET Web Forms ve ASP.NET MVC dahil) tüm Visual Studio Proje türleri ile çalışır.

NuGet paketini kendi kitaplıkları ve bunları çevrimiçi bir galeride kaydetmek için açık kaynak projeler (örneğin, proje Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks ve Elmah gibi) bakımını yapan geliştiricilerin sağlar. Ardından bu kitaplıklar birini paketi bulmak ve üzerinde çalıştıkları projelerinde yüklemek için kullanmak isteyen .NET geliştiricilerinin kolaydır.

NuGet aracılığıyla güncelleştirilebilir şekilde ASP.NET 3 araçları güncelleştirme ile JavaScript kitaplıklarını önceden yüklenmiş NuGet paketlerini, proje şablonları içerir. Entity Framework Code First da NuGet paketi olarak önceden yüklenir.

NuGet hakkında daha fazla bilgi için bkz. [NuGet belgeleri](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Kısmi sayfa çıktı önbelleği

ASP.NET MVC 1 sürümünden itibaren tam sayfada yanıtların çıktı önbelleği destek içerir. MVC 3 da kısmi sayfa çıktı önbelleği, önbellek bölgeleri kolayca veya bir yanıt parçalarını sağlayan destekler. Önbelleğe alma hakkında daha fazla bilgi için bkz. **kısmi sayfa çıktı önbelleği** bölümünü [Scott Guthrie'nin blog postası MVC 3 Sürüm Adayı üzerinde](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) ve **alt eylem çıktı önbelleği** bölümünü [MVC 3 Sürüm Notları](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>İstek doğrulamanın üzerinde ayrıntılı denetim

ASP.NET MVC, otomatik olarak XSS ve HTML ekleme saldırılarına karşı korumaya yardımcı olan yerleşik istek doğrulamayı sahiptir. Ancak, açıkça devre dışı bırakma isteği doğrulama, bazen istediğiniz IF gibi kullanıcıların HTML (örneğin, blog girişleri veya CMS içerik) içerik sonrası istersiniz. Artık ekleyebilirsiniz bir [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) özniteliği modellere veya model bağlama sırasında özelliğe temelinde istek doğrulama devre dışı bırakmak için modelleri görüntüleyin. İstek doğrulama hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- **Örtük JavaScript'i ve doğrulama** konusundaki [Scott Guthrie'nin blog postası MVC 3 Sürüm Adayı üzerinde](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [MVC 3 sürüm notları](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Genişletilebilir "Yeni Proje" iletişim kutusu

ASP.NET MVC 3'te, proje şablonları, görünüm altyapısının ekleyebilirsiniz ve birim testi projesi çerçeveler için **yeni proje** iletişim kutusu.

### <a name="template-scaffolding-improvements"></a>Şablon yapı İskelesi geliştirmeleri

ASP.NET MVC 3 yapı iskelesi şablonları modelleri birincil anahtar özellikleri tanımlamak ve bunları uygun şekilde daha MVC önceki sürümlerinde işleme konusunda daha iyi iş yapın. (Örneğin, yapı iskelesi şablonları artık birincil anahtarı bir düzenlenebilir bir forma alan olarak başladınız değil emin olun.)

Varsayılan olarak, oluşturma ve düzenleme iskelesini kurar kullanmak `Html.EditorFor` Yardımcısı yerine `Html.TextBoxFor` Yardımcısı. Bu meta veri biçiminde veri modeli için desteği geliştirir ek açıklama öznitelikleri **Görünüm Ekle** iletişim kutusu, bir görünüm oluşturur.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>"Html.LabelFor" ve "Html.LabelForModel" için yeni aşırı yüklemeler

Yeni bir yöntem aşırı yüklemeleri için eklenmiştir `LabelFor` ve `LabelForModel` yardımcı yöntemler. Yeni aşırı etiket metni geçersiz kılmak etkinleştirin.

### <a name="sessionless-controller-support"></a>Oturumsuz denetleyici desteği

Oturum durumunu kullanmak için bir denetleyici sınıfı istediğiniz işlenmeyeceğini ve işlenecekse belirtebilirsiniz ASP.NET MVC 3'te mi oturum durumu olmalıdır okuma/yazma veya salt okunur. Oturumsuz denetleyicisi desteği hakkında daha fazla bilgi için bkz. [MVC 3 Sürüm Notları](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Yeni "AdditionalMetadataAttribute" sınıfı

Kullanabileceğiniz [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) doldurmak için öznitelik `ModelMetadata.AdditionalValues` model özelliğine sözlüğü. Örneğin, bir görünüm modeli yalnızca yönetici görüntülenmesi gereken bir özellik varsa, aşağıdaki örnekte gösterildiği gibi bu özellik açıklama ekleyebilirsiniz:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Bir ürün görünümü model yeniden işlendiğinde bu meta veriler herhangi bir görüntü veya Düzenleyicisi şablon için kullanılabilir hale getirilir. Bu meta veri bilgilerini yorumlamak için size bağlıdır.

### <a name="accountcontroller-improvements"></a>AccountController geliştirmeleri

AccountController Internet proje şablonunda büyük ölçüde geliştirilmiştir.

### <a name="new-intranet-project-template"></a>Yeni Intranet projesi şablonu

Yeni Intranet projesi şablonun eklendiğinden, Windows kimlik doğrulamasını etkinleştirir ve AccountController kaldırır.
