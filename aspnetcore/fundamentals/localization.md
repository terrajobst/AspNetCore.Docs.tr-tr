---
title: ASP.NET Core Genelleştirme ve yerelleştirme
author: rick-anderson
description: ASP.NET Core farklı diller ve kültürlere içerik yerelleştirilmesi için nasıl hizmet ve ara yazılım sağladığını öğrenin.
ms.author: riande
ms.date: 01/14/2017
uid: fundamentals/localization
ms.openlocfilehash: 6dfbeae201a3586dfea6620917083130c4985b22
ms.sourcegitcommit: dc96d76f6b231de59586fcbb989a7fb5106d26a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703805"
---
# <a name="globalization-and-localization-in-aspnet-core"></a>ASP.NET Core Genelleştirme ve yerelleştirme

By [Rick Anderson](https://twitter.com/RickAndMSFT), [Hemien Bowden](https://twitter.com/damien_bod), [Bart calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), ve [fiham bin ateya](https://twitter.com/hishambinateya)

Bu belge ASP.NET Core 3,0 için güncelleştirilene kadar, [ASP.NET Core 3,0 ' de bkz. yeni yerelleştirme](http://hishambinateya.com/what-is-new-in-localization-in-asp.net-core-3.0)için bkz.

ASP.NET Core bir çoklu dil web sitesi oluşturmak, sitenizin daha geniş bir hedef kitleye ulaşmasını sağlar. ASP.NET Core, farklı diller ve kültürlere yerelleştirme için hizmet ve ara yazılım sağlar.

Uluslararası duruma getirme, [Genelleştirme](/dotnet/api/system.globalization) ve [Yerelleştirme](/dotnet/standard/globalization-localization/localization)içerir. Genelleştirme, farklı kültürleri destekleyen uygulamalar tasarlama işlemidir. Genelleştirme, belirli coğrafi alanlarla ilgili olarak tanımlanmış bir dil betikleri kümesinin giriş, görüntüleme ve çıkışı için destek ekler.

Yerelleştirme, belirli bir kültür/yerel ayara, Yerelleştirilebilirlik için zaten işlenmiş olan bir Genelleştirilmiş uygulamasını uyarlatma işlemidir. Daha fazla bilgi için bu belgenin sonundaki **Genelleştirme ve yerelleştirme koşullarına** bakın.

Uygulama yerelleştirmesi şunları içerir:

1. Uygulamanın içeriğini yerelleştirilebilir yapın

2. Destekettiğiniz diller ve kültürler için yerelleştirilmiş kaynaklar sağlayın

3. Her istek için dil/kültür seçmek üzere bir strateji uygulayın

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="make-the-apps-content-localizable"></a>Uygulamanın içeriğini yerelleştirilebilir yapın

ASP.NET Core tanıtılan `IStringLocalizer` ve `IStringLocalizer<T>`, yerelleştirilmiş uygulamalar geliştirilirken üretkenliği artırmak için tasarlanmıştır. `IStringLocalizer`, çalışma zamanında kültüre özgü kaynaklar sağlamak için [ResourceManager](/dotnet/api/system.resources.resourcemanager) ve [ResourceReader](/dotnet/api/system.resources.resourcereader) kullanır. Basit arabirimde bir Dizin Oluşturucu ve yerelleştirilmiş dizeleri döndürmek için bir `IEnumerable` vardır. `IStringLocalizer`, varsayılan dil dizelerini bir kaynak dosyasında depolamanızı gerektirmez. Yerelleştirmeye yönelik bir uygulama geliştirebilir ve geliştirmede erken kaynak dosyaları oluşturmaya gerek yoktur. Aşağıdaki kod, yerelleştirme için "başlık hakkında" dizesinin nasıl sarılacağı gösterilmektedir.

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

Yukarıdaki kodda `IStringLocalizer<T>` uygulamasının [bağımlılığı ekleme](dependency-injection.md)işleminden gelir. Yerelleştirilmiş "başlık hakkında" değeri bulunmazsa, Dizin Oluşturucu anahtarı döndürülür, diğer bir deyişle "başlık hakkında" dizesidir. Uygulama geliştirmeye odaklanabilmeniz için varsayılan dil değişmez dizelerini uygulamada bırakabilir ve Localizer ' da kaydırabilirsiniz. Uygulamanızı varsayılan diliniz ile geliştirin ve öncelikle varsayılan bir kaynak dosyası oluşturmadan yerelleştirme adımına hazırlayın. Alternatif olarak, geleneksel yaklaşımı kullanabilir ve varsayılan dil dizesini almak için bir anahtar sağlayabilirsiniz. Birçok geliştirici için, varsayılan Language *. resx* dosyası olmayan yeni iş akışı ve yalnızca dize değişmez değerlerini sarmalama, bir uygulamayı yerelleştirme yükünü azaltabilir. Diğer geliştiriciler geleneksel iş akışını tercih eder, daha uzun dize sabit değerleri ile çalışmayı kolaylaştırır ve yerelleştirilmiş dizelerin güncelleştirilmesini kolaylaştırır.

HTML içeren kaynaklar için `IHtmlLocalizer<T>` uygulamasını kullanın. `IHtmlLocalizer` HTML, kaynak dizesinde biçimlendirilen ve kaynak dizenin kendisini kodlayan bağımsız değişkenleri kodluyor. Aşağıda vurgulanan örnekte yalnızca `name` parametresinin değeri HTML kodlamalı olur.

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Not:** Genellikle HTML değil yalnızca metni yerelleştirmek istersiniz.

En düşük düzeyde, [bağımlılık ekleme](dependency-injection.md)`IStringLocalizerFactory` ' ı alabilir:

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Yukarıdaki kod, iki fabrika oluşturma yönteminin her birini gösterir.

Yerelleştirilmiş dizelerinizi denetleyiciye, alana veya yalnızca bir kapsayıcıya göre bölümleyebilirsiniz. Örnek uygulamada, paylaşılan kaynaklar için `SharedResource` adlı bir kukla sınıf kullanılır.

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

Bazı geliştiriciler, genel veya paylaşılan dizeler içermesi için `Startup` sınıfını kullanır. Aşağıdaki örnekte, `InfoController` ve `SharedResource` yerelleştiriciler kullanılır:

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Yerelleştirmeyi görüntüle

@No__t-0 hizmeti bir [Görünüm](xref:mvc/views/overview)için yerelleştirilmiş dizeler sağlar. @No__t-0 sınıfı bu arabirimi uygular ve kaynak konumunu görünüm dosyası yolundan bulur. Aşağıdaki kod, `IViewLocalizer` ' ın varsayılan uygulamasının nasıl kullanılacağını gösterir:

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

@No__t-0 ' ın varsayılan uygulanması, kaynak dosyayı görünümün dosya adına göre bulur. Genel paylaşılan kaynak dosyası kullanma seçeneği yoktur. `ViewLocalizer` `IHtmlLocalizer` kullanarak yorumdur 'ı uygular, bu nedenle Razor, yerelleştirilmiş dizeyi HTML olarak kodlayamıyor. Kaynak dizelerini parametreleştirebilirsiniz ve `IViewLocalizer`, parametreleri kaynak dize değil, HTML olarak kodlayabilir. Aşağıdaki Razor işaretlemesini göz önünde bulundurun:

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Bir Fransızca kaynak dosyası şunları içerebilir:

| Anahtar | Value |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

İşlenmiş görünüm, kaynak dosyasındaki HTML işaretlemesini içerir.

**Not:** Genellikle HTML değil yalnızca metni yerelleştirmek istersiniz.

Paylaşılan bir kaynak dosyasını bir görünümde kullanmak için @no__t Ekle-0:

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Dataaçıklamaların yerelleştirilmesi

Dataaçıklamalarda hata iletileri `IStringLocalizer<T>` ile yerelleştirilir. @No__t-0 seçeneğini kullanarak `RegisterViewModel` ' deki hata iletileri aşağıdaki yollardan birinde depolanabilir:

* *Resources/Viewmodeller. account. RegisterViewModel. fr. resx*
* *Kaynaklar/Viewmodeller/hesap/RegisterViewModel. fr. resx*

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

ASP.NET Core MVC 1.1.0 ve üzeri, doğrulama olmayan öznitelikler yereldir. ASP.NET Core MVC 1,0, doğrulama olmayan öznitelikler için yerelleştirilmiş **dizeleri aramaz.**

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a>Birden çok sınıf için bir kaynak dizesi kullanma

Aşağıdaki kod, birden çok sınıfa sahip doğrulama öznitelikleri için bir kaynak dizesinin nasıl kullanılacağını gösterir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

Yukarıdaki kodda, `SharedResource`, doğrulama iletilerinizin depolandığı resx öğesine karşılık gelen sınıftır. Bu yaklaşımda, veri açıklamaları her sınıf için kaynak yerine yalnızca `SharedResource` kullanır.

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Destekettiğiniz diller ve kültürler için yerelleştirilmiş kaynaklar sağlayın

### <a name="supportedcultures-and-supporteduicultures"></a>Supportedkültürleri ve SupportedUICultures

ASP.NET Core iki kültür değeri belirtmenize izin verir, `SupportedCultures` ve `SupportedUICultures`. @No__t-1 için [CultureInfo](/dotnet/api/system.globalization.cultureinfo) nesnesi, tarih, saat, sayı ve para birimi biçimlendirme gibi kültüre bağımlı işlevlerin sonuçlarını belirler. `SupportedCultures` Ayrıca metnin, büyük/küçük harf kurallarının ve dize karşılaştırmalarının sıralama sırasını da belirler. Sunucunun kültürü nasıl aldığı hakkında daha fazla bilgi için bkz [. CultureInfo. CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) . @No__t-0, hangi dizelerin ( *. resx* dosyalarından) [ResourceManager](/dotnet/api/system.resources.resourcemanager)tarafından arandığını belirler. @No__t-0, yalnızca `CurrentUICulture` tarafından belirlenen kültüre özgü dizeleri arar. .NET 'teki her iş parçacığında `CurrentCulture` ve `CurrentUICulture` nesneleri vardır. ASP.NET Core kültüre bağımlı işlevleri işlerken bu değerleri inceler. Örneğin, geçerli iş parçacığının kültürü "en-US" (Ingilizce, Birleşik Devletler) olarak ayarlandıysa, `DateTime.Now.ToLongDateString()` "Perşembe, 18 Şubat 2016" değerini görüntüler, ancak `CurrentCulture` ' i "ES-ES" (Ispanyolca, Ispanya) olarak ayarlandıysa çıkış "Jueves, 18 de febrero de 2016" olacaktır.

## <a name="resource-files"></a>Kaynak dosyalar

Kaynak dosyası, koddan yerelleştirilebilir dizeleri ayırmak için kullanışlı bir mekanizmadır. Varsayılan olmayan dil için çevrilmiş dizeler yalıtılmış *. resx* kaynak dosyalarıdır. Örneğin, çevrilmiş dizeleri içeren *Welcome. es. resx* adlı İspanyolca kaynak dosyası oluşturmak isteyebilirsiniz. "es", Ispanyolca için dil kodudur. Bu kaynak dosyasını Visual Studio 'da oluşturmak için:

1. **Çözüm Gezgini**, kaynak dosyasını içerecek klasöre sağ tıklayın >  > **Yeni öğe** **ekleyin**.

    ![İç içe bağlam menüsü: Çözüm Gezgini, kaynaklar için bir bağlamsal menü açıktır. İkinci bağlamsal menü, yeni öğe komutunun vurgulandığı ekleme için açıktır.](localization/_static/newi.png)

2. **Yüklü şablonları ara** kutusuna "kaynak" yazın ve dosyayı adlandırın.

    ![Yeni öğe Ekle iletişim kutusu](localization/_static/res.png)

3. **Ad** sütununa anahtar değerini (yerel dize) ve **değer** sütununda çevrilmiş dizeyi girin.

    ![Ad sütunundaki Merhaba. es. resx dosyası (Ispanyolca için hoş geldiniz kaynak dosyası) ve değer sütununda Hola (Ispanyolca) adlı kelime](localization/_static/hola.png)

    Visual Studio, *Welcome. es. resx* dosyasını gösterir.

    ![Hoş geldiniz Ispanyolca (es) kaynak dosyasını gösteren Çözüm Gezgini](localization/_static/se.png)

## <a name="resource-file-naming"></a>Kaynak dosyası adlandırma

Kaynaklar, sınıfının tam tür adı için derleme adı eksi olarak adlandırılır. Örneğin, ana derlemesi sınıf için `LocalizationWebsite.Web.dll` olan bir projedeki bir Fransızca kaynak `LocalizationWebsite.Web.Startup`, *Startup. fr. resx*olarak adlandırılır. @No__t-0 sınıfı için bir kaynak, *Controllers. HomeController. fr. resx*olarak adlandırılır. Hedeflenen sınıfınızın ad alanı, derleme adı ile aynı değilse, tam tür adına ihtiyacınız olur. Örneğin, örnek projede `ExtraNamespace.Tools` türü için bir kaynak *ExtraNamespace. Tools. fr. resx*olarak adlandırılır.

Örnek projede `ConfigureServices` yöntemi, `ResourcesPath` ' i "resources" olarak ayarlar. bu nedenle, ana denetleyicinin Fransızca kaynak dosyasının proje göreli yolu *kaynaklar/denetleyiciler. HomeController. fr. resx*olur. Alternatif olarak, kaynak dosyalarını düzenlemek için klasörleri de kullanabilirsiniz. Ana denetleyici için yol *kaynaklar/denetleyiciler/HomeController. fr. resx*olacaktır. @No__t-0 seçeneğini kullanmazsanız, *. resx* dosyası proje temel dizinine gidecek. @No__t-0 ' a ait kaynak dosyası *denetleyicileri. HomeController. fr. resx*olarak adlandırılır. Nokta veya yol adlandırma kuralını kullanma seçeneği, kaynak dosyalarınızı nasıl düzenlemek istediğinize bağlıdır.

| Kaynak adı | Nokta veya yol adlandırma |
| ------------   | ------------- |
| Kaynaklar/denetleyiciler. HomeController. fr. resx | Nokta  |
| Kaynaklar/denetleyiciler/HomeController. fr. resx  | `Path` |
|    |     |

Razor görünümlerinde `@inject IViewLocalizer` kullanan kaynak dosyaları benzer bir düzene uyar. Bir görünüm için kaynak dosyası, nokta adlandırması veya yol adlandırması kullanılarak adlandırılabilir. Razor görünümü kaynak dosyaları, ilişkili görünüm dosyalarının yolunu taklit. @No__t-0 ' ı "resources" olarak belirlediğimiz varsayılarak, *Görünümler/Home/about. cshtml* görünümüyle ilişkili Fransızca kaynak dosyası aşağıdakilerden biri olabilir:

* Kaynaklar/görünümler/giriş/about. fr. resx

* Kaynaklar/görünümler. Home. about. fr. resx

@No__t-0 seçeneğini kullanmazsanız, bir görünümün *. resx* dosyası görünümü ile aynı klasörde bulunur.

### <a name="rootnamespaceattribute"></a>RootNamespaceAttribute 

[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) özniteliği, bir derlemenin kök ad alanı derleme adından farklı olduğunda bir derlemenin kök ad alanını sağlar. 

Bir derlemenin kök ad alanı, derleme adından farklıysa:

* Yerelleştirme varsayılan olarak çalışmaz.
* Yerelleştirme, derleme içinde kaynakların aranacağı yol nedeniyle başarısız olur. `RootNamespace`, yürütme işlemi için kullanılamayan bir derleme zamanı değeridir. 

@No__t-0 `AssemblyName` ' den farklıysa, *AssemblyInfo.cs* (parametre değerleri gerçek değerlerle değiştirilmiştir) içine aşağıdakini ekleyin:

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

Yukarıdaki kod resx dosyalarının başarıyla çözümlenmesine izin vermez.

## <a name="culture-fallback-behavior"></a>Kültür geri dönüş davranışı

Bir kaynak aranırken, yerelleştirme "kültür geri dönüş" bölümünde ilgilenir. İstenen kültürden başlayarak, bulunamazsa bu kültürün üst kültürüne geri döner. Bir kenara de, [CultureInfo. Parent](/dotnet/api/system.globalization.cultureinfo.parent) özelliği üst kültürü temsil eder. Bu genellikle (her zaman değil), National signifier 'in ISO 'dan kaldırılması anlamına gelir. Örneğin, Meksika 'da konuşulan Ispanyolca diyalekt "es-MX" dir. Tüm ülkelere özgü olmayan "es" &mdash;Ispanyolca üst öğesi vardır.

Sitenizin "fr-CA" kültürünü kullanarak "hoş geldiniz" kaynağı için bir istek aldığını düşünün. Yerelleştirme sistemi aşağıdaki kaynakları sırayla arar ve ilk eşleşmeyi seçer:

* *Welcome.fr-CA. resx*
* *Welcome. fr. resx*
* *Welcome. resx* (`NeutralResourcesLanguage` "fr-CA" ise)

Örnek olarak, ". fr" kültür göstergesini kaldırırsanız ve kültürü Fransızca olarak ayarlarsanız, varsayılan kaynak dosyası okunurdur ve dizeler yerelleştirilir. Kaynak Yöneticisi, istenen kültürü hiçbir şey karşılamıyorsa, için bir varsayılan veya geri dönüş kaynağı belirler. Yalnızca istenen kültür için bir kaynak eksik olduğunda anahtarı döndürmek istiyorsanız varsayılan bir kaynak dosyanız olmamalıdır.

### <a name="generate-resource-files-with-visual-studio"></a>Visual Studio ile kaynak dosyaları oluşturma

Dosya adında bir kültür olmadan Visual Studio 'da bir kaynak dosyası oluşturursanız (örneğin, *Welcome. resx*), Visual Studio her bir dize için bir özelliği olan bir C# sınıf oluşturur. ASP.NET Core, genellikle istediğiniz gibi değildir. Genellikle Default *. resx* kaynak dosyanız (kültür adı olmayan bir *. resx* dosyası) yoktur. *. Resx* dosyasını bir kültür adı (örneğin, *Welcome. fr. resx*) ile oluşturmanızı öneririz. Kültür adı ile bir *. resx* dosyası oluşturduğunuzda, Visual Studio sınıf dosyası oluşturmaz. Birçok geliştiricinin varsayılan dil kaynak dosyası oluşturmayacağız.

### <a name="add-other-cultures"></a>Diğer kültürleri Ekle

Her dil ve kültür bileşimi (varsayılan dil dışında), benzersiz bir kaynak dosyası gerektirir. ISO dili kodlarının dosya adının parçası olduğu yeni kaynak dosyaları oluşturarak farklı kültürler ve yerel ayarlar için kaynak dosyaları oluşturun (örneğin, **en-US**, **fr-CA**ve **en-GB**). Bu ISO kodları, *Welcome.es-MX. resx* (Ispanyolca/Meksika) içinde olduğu gibi dosya adı ve *. resx* dosya uzantısı arasına yerleştirilir.

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Her istek için dil/kültür seçmek üzere bir strateji uygulayın

### <a name="configure-localization"></a>Yerelleştirmeyi yapılandırma

Yerelleştirme `Startup.ConfigureServices` yönteminde yapılandırılır:

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* `AddLocalization`, yerelleştirme hizmetlerini hizmetler kapsayıcısına ekler. Yukarıdaki kod, kaynakların yolunu da "resources" olarak ayarlar.

* `AddViewLocalization` yerelleştirilmiş görünüm dosyaları için destek ekler. Bu örnek görünümde yerelleştirme, görünüm dosyası sonekini temel alır. Örneğin, *Index. fr. cshtml* dosyasındaki "fr".

* `AddDataAnnotationsLocalization`, yerelleştirilmiş `DataAnnotations` doğrulama iletileri için `IStringLocalizer` soyutlamalar aracılığıyla destek ekler.

### <a name="localization-middleware"></a>Yerelleştirme ara yazılımı

Bir istekteki geçerli kültür, yerelleştirme [Ara](xref:fundamentals/middleware/index)ortamında ayarlanır. Yerelleştirme ara yazılımı `Startup.Configure` yönteminde etkinleştirilir. Yerelleştirme ara yazılımı, istek kültürünü denetlemeyebilir (örneğin, `app.UseMvcWithDefaultRoute()`) herhangi bir ara yazılım önce yapılandırılmalıdır.

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]

`UseRequestLocalization` `RequestLocalizationOptions` nesnesini başlatır. Her istekte, `RequestLocalizationOptions` ' deki `RequestCultureProvider` listesi numaralandırılır ve istek kültürünü başarıyla belirleyebilmesi için ilk sağlayıcı kullanılır. Varsayılan sağlayıcılar `RequestLocalizationOptions` sınıfından gelir:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

Varsayılan liste, en çok belirli olan en az özel. Makalenin ilerleyen kısımlarında, sırayı nasıl değiştirekullanabileceğinizi ve hatta özel bir kültür sağlayıcısı nasıl ekleyebileceğiniz hakkında bilgi edineceksiniz. Sağlayıcıların hiçbiri istek kültürünü belirleyeemiyorsa, `DefaultRequestCulture` kullanılır.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Bazı uygulamalar [kültür ve UI kültürünü](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)ayarlamak için bir sorgu dizesi kullanır. Tanımlama bilgisi veya Accept-Language üst bilgisi yaklaşımını kullanan uygulamalar için, URL 'ye bir sorgu dizesi eklemek hata ayıklama ve test kodu için yararlıdır. Varsayılan olarak `QueryStringRequestCultureProvider`, `RequestCultureProvider` listesinde ilk yerelleştirme sağlayıcısı olarak kaydedilir. @No__t-0 ve `ui-culture` sorgu dizesi parametrelerini geçirirsiniz. Aşağıdaki örnek, belirli kültürü (dil ve bölge) Ispanyolca/Meksika olarak ayarlar:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Yalnızca iki (`culture` veya `ui-culture`) birini geçirirseniz, sorgu dizesi sağlayıcısı, her iki değeri de geçirdiğiniz birini kullanarak ayarlar. Örneğin, yalnızca kültür ayarlandığında `Culture` ve `UICulture` ayarlanır:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Üretim uygulamaları genellikle ASP.NET Core kültür tanımlama bilgisiyle kültürü ayarlamaya yönelik bir mekanizma sağlar. Bir tanımlama bilgisi oluşturmak için `MakeCookieValue` yöntemini kullanın.

@No__t-0 `DefaultCookieName`, kullanıcının tercih ettiği kültür bilgilerini izlemek için kullanılan varsayılan tanımlama bilgisi adını döndürür. Varsayılan tanımlama bilgisi adı `.AspNetCore.Culture` ' dır.

Tanımlama bilgisi biçimi `c=%LANGCODE%|uic=%LANGCODE%` ' dır, burada `c` `Culture` ' dir ve `uic` `UICulture` ' dir; örneğin:

    c=en-UK|uic=en-US

Kültür bilgisi ve UI kültürünün yalnızca birini belirtirseniz, belirtilen kültür hem kültür bilgileri hem de UI kültürü için kullanılacaktır.

### <a name="the-accept-language-http-header"></a>Accept-Language HTTP üstbilgisi

[Accept-Language üst bilgisi](https://www.w3.org/International/questions/qa-accept-lang-locales) tarayıcıların çoğu tarayıcıda ayarlanabilir ve başlangıçta kullanıcının dilini belirtmeye yöneliktir. Bu ayar, tarayıcının temel alınan işletim sisteminden gönderme veya devralma olarak ayarlandığını gösterir. Bir tarayıcıdan Accept-Language HTTP üst bilgisi, kullanıcının tercih ettiği dili algılamaya yönelik uygun olmayan bir yoldur (bkz. [bir tarayıcıda dil tercihlerini ayarlama](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Bir üretim uygulaması, kullanıcının kültür seçimini özelleştirmenin bir yolunu içermelidir.

### <a name="set-the-accept-language-http-header-in-ie"></a>IE 'de Accept-Language HTTP üstbilgisini ayarlama

1. Dişli simgesinden **Internet seçenekleri**' ne dokunun.

2. **Diller**' e dokunun.

    ![Internet seçenekleri](localization/_static/lang.png)

3. **Dil tercihlerini ayarla**' ya dokunun.

4. **Dil ekle**' ye dokunun.

5. Dilini ekleyin.

6. Dile dokunun ve ardından **Yukarı taşı**' ya dokunun.

### <a name="use-a-custom-provider"></a>Özel bir sağlayıcı kullan

Müşterilerinizin kendi dil ve kültürünü veritabanlarınızı veritabanlarına depolamasına izin vermek istediğinizi varsayalım. Kullanıcı için bu değerleri aramak üzere bir sağlayıcı yazabilirsiniz. Aşağıdaki kod, özel bir sağlayıcının nasıl ekleneceğini göstermektedir:

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

Yerelleştirme sağlayıcıları eklemek veya kaldırmak için `RequestLocalizationOptions` kullanın.

### <a name="set-the-culture-programmatically"></a>Kültürü program aracılığıyla ayarlama

[GitHub](https://github.com/aspnet/entropy) 'daki Bu örnek **Yerelleştirme. starterweb** projesi, `Culture` ayarlamak için Kullanıcı arabirimi içerir. *Views/Shared/_SelectLanguagePartial. cshtml* dosyası desteklenen kültürler listesinden kültürü seçmenizi sağlar:

[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial. cshtml* dosyası, düzen dosyasının `footer` bölümüne eklenir, bu nedenle tüm görünümlerde kullanılabilir hale gelir:

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

@No__t-0 yöntemi kültür tanımlama bilgisini ayarlar.

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

Bu proje için örnek koda *_Selectlanguagepartial. cshtml* ekleyemezsiniz. [GitHub](https://github.com/aspnet/entropy) 'daki **Yerelleştirme. starterweb** projesi, [bağımlılık ekleme](dependency-injection.md) kapsayıcısı aracılığıyla `RequestLocalizationOptions` ' y i Razor kısmi 'e Flow koduna sahiptir.

## <a name="globalization-and-localization-terms"></a>Genelleştirme ve yerelleştirme koşulları

Uygulamanızı yerelleştirme işlemi, modern yazılım geliştirmede yaygın olarak kullanılan ilgili karakter kümelerinin temel olarak anlaşılmasını ve bunlarla ilişkili sorunların anlaşılmasını gerektirir. Tüm bilgisayarlar metni sayı (kodlar) olarak depolayabilse de, farklı sistemler farklı sayılar kullanarak aynı metni depolar. Yerelleştirme süreci, belirli bir kültür/yerel ayar için uygulama kullanıcı arabirimini (UI) çevirmeye başvurur.

[Yerelleştirilebilirlik](/dotnet/standard/globalization-localization/localizability-review) , bir Genelleştirilmiş uygulamasının yerelleştirme için hazırlandığının doğrulanması için bir ara işlemdir.

Kültür adı için [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) biçimi `<languagecode2>-<country/regioncode2>` ' dir; burada `<languagecode2>` dil kodudur ve `<country/regioncode2>` alt kültür kodudur. Örneğin, Ispanyolca için `es-CL` (Şili), Ingilizce (Birleşik Devletler) için `en-US` ve Ingilizce (Avustralya) için `en-AU`. [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) , bir dille ILIŞKILI bir ISO 639 2-Letter küçük harfli kültür kodu ve bir ülke veya bölgeyle ILIŞKILI bir ISO 3166 2 harfli büyük harf alt kültür kodu birleşimidir. Bkz. [dil kültürü adı](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Uluslararası duruma getirme genellikle "I18N" olarak kısaltılır. Kısaltma ilk ve son harfleri ve aralarındaki harflerin sayısını alır, bu nedenle 18 ilk "I" ve son "N" arasındaki harflerin sayısını temsil eder. Aynı Genelleştirme (G11N) ve yerelleştirme (L10N) için de geçerlidir.

Larındaki

* Genelleştirme (G11N): Uygulama yapma işlemi farklı dilleri ve bölgeleri destekler.
* Yerelleştirme (L10N): Bir uygulamayı belirli bir dil ve bölge için özelleştirme işlemi.
* Uluslararası duruma getirme (I18N): Genelleştirme ve yerelleştirme konularını açıklar.
* Ayarı Bu bir dildir ve isteğe bağlı olarak bir bölgedir.
* Nötr kültür: Belirtilen dile sahip, ancak bölge olmayan bir kültür. (örneğin, "en", "es")
* Belirli kültür: Belirtilen dile ve bölgeye sahip bir kültür. (örneğin, "en-US", "en-GB", "es-CL")
* Üst kültür: Belirli bir kültürü içeren nötr kültür. (örneğin, "tr", "en-US" ve "en-GB" öğesinin üst kültürüdür)
* Ayarlar Yerel ayar kültür ile aynıdır.

[!INCLUDE[](~/includes/currency.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* Makalesinde kullanılan [Yerelleştirme. StarterWeb projesi](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) .
* [.NET uygulamalarını genelleştirmek ve yerelleştirme](/dotnet/standard/globalization-localization/index)
* [. Resx dosyalarındaki kaynaklar](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Microsoft çok dilli uygulama araç seti](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Yerelleştirme & genel türler](https://github.com/hishamco/hishambinateya.com/blob/master/Posts/localization-and-generics.md)
