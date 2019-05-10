---
title: ASP.NET Core MVC için ASP.NET MVC ' geçiş
author: ardalis
description: Bir ASP.NET MVC projesi için ASP.NET Core MVC geçişini kullanmaya nasıl başlayacağınızı öğrenin.
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a9e2b41b933ed04a23515564892ed1694a4ac4f8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899729"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>ASP.NET Core MVC için ASP.NET MVC ' geçiş

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), ve [Scott Addie](https://scottaddie.com)

Bu makalede, bir ASP.NET MVC projesini geçişini kullanmaya başlamak gösterilmektedir [ASP.NET Core MVC](../mvc/overview.md). İşlem sırasında ASP.NET MVC değişmiş olan şeyleri çoğunu vurgulamaktadır. Birden çok adımlı bir işlemin, ASP.NET MVC ' geçişi ve bu makalede, ilk Kurulum, temel denetleyicileri ve görünümleri, statik içerik ve istemci tarafı bağımlılıkları yer almaktadır. Diğer makaleler geçirme yapılandırma ve birçok ASP.NET MVC projesinde bulunan bir kimlik kodu kapsar.

> [!NOTE]
> Sürüm numaraları örneklerdeki geçerli olmayabilir. Projelerinizi uygun şekilde güncelleştirmeniz gerekebilir.

## <a name="create-the-starter-aspnet-mvc-project"></a>Başlangıç ASP.NET MVC projesi oluşturma

Yükseltme göstermek için bir ASP.NET MVC uygulaması oluşturarak başlayacağız. Adlı oluşturun *WebApp1* ad sonraki adımda oluşturacağız ASP.NET Core projesi eşleşecek şekilde.

![Visual Studio yeni proje iletişim kutusu](mvc/_static/new-project.png)

![Yeni Web uygulaması iletişim kutusu: ASP.NET şablonları panelinde seçili MVC proje şablonu](mvc/_static/new-project-select-mvc-template.png)

*İsteğe bağlı:* Çözüm adını değiştirmek *WebApp1* için *Mvc5*. Visual Studio yeni çözüm adını görüntüler (*Mvc5*), kolaylaştırır bu projeyi bir sonraki projenizde söylemek.

## <a name="create-the-aspnet-core-project"></a>ASP.NET Core projesi oluşturma

Yeni bir *boş* önceki projeyle aynı ada sahip bir ASP.NET Core web uygulaması (*WebApp1*) iki proje alanlarında eşleşecek şekilde. Aynı ad alanına sahip iki proje arasında kod kopyalamak kolaylaştırır. Aynı adı kullanmak için önceki projeyi farklı bir dizine içinde bu proje oluşturmanız gerekir.

![Yeni Proje iletişim kutusu](mvc/_static/new_core.png)

![Yeni ASP.NET Web uygulaması iletişim kutusu: ASP.NET Core şablonları panelinde seçili boş proje şablonu](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *İsteğe bağlı:* Yeni bir ASP.NET Core uygulamasını kullanarak oluşturma *Web uygulaması* proje şablonu. Projeyi adlandırın *WebApp1*ve bir kimlik doğrulama seçeneği işaretleyin **bireysel kullanıcı hesapları**. Bu uygulamayı yeniden adlandır *FullAspNetCore*. Size zaman kazandırır proje dönüştürme oluşturuluyor. Şablon tarafından oluşturulan kodu sonuç görmek için veya dönüştürme projeye kodu kopyalamak göz atabilirsiniz. Şablon tarafından oluşturulan proje ile karşılaştırılacak bir dönüştürme adımında takılı kalarak olduğunda da yararlıdır.

## <a name="configure-the-site-to-use-mvc"></a>Siteyi MVC kullanacak şekilde yapılandırma

::: moniker range=">= aspnetcore-2.1"

* .NET Core hedeflenirken [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) varsayılan olarak başvurulur. Bu paket, MVC uygulamaları tarafından yaygın olarak kullanılan paketler paketleri içerir. .NET Framework'ü hedefleyen, paket başvuruları ayrı ayrı proje dosyasında listelenmelidir.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* .NET Core hedeflenirken [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) varsayılan olarak başvurulur. Bu paket, MVC uygulamaları tarafından yaygın olarak kullanılan paketler paketleri içerir. .NET Framework'ü hedefleyen, paket başvuruları ayrı ayrı proje dosyasında listelenmelidir.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* .NET Core veya .NET Framework hedefleme, MVC uygulamaları tarafından yaygın olarak kullanılan paketler paketlerini ayrı ayrı proje dosyasında listelenir.

::: moniker-end

`Microsoft.AspNetCore.Mvc` ASP.NET Core MVC çerçevedir. `Microsoft.AspNetCore.StaticFiles` statik dosya işleyicisidir. ASP.NET Core çalışma zamanı, modüler ve siz açıkça statik dosyaları işleme kabul etmek gerekir (bkz [statik dosyalar](xref:fundamentals/static-files)).

* Açık *Startup.cs* dosya ve kodu aşağıdaki ile eşleşecek şekilde değiştirin:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles` Genişletme yöntemi statik dosya işleyicisi ekler. Daha önce belirtildiği gibi ASP.NET çalışma zamanı modüler ve siz açıkça statik dosyaları işleme kabul etmek gerekir. `UseMvc` Uzantı yöntemi, yönlendirme ekler. Daha fazla bilgi için [uygulama başlatma](xref:fundamentals/startup) ve [yönlendirme](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Bir denetleyici ve Görünüm Ekle

Bu bölümde, bir en az bir denetleyici ve ASP.NET MVC denetleyicisi için yer tutucu olarak görev yapacak görünümü ve sonraki bölümde geçiş görünümü ekleyeceksiniz.

* Ekleme bir *denetleyicileri* klasör.

* Ekleme bir **denetleyici sınıfı** adlı *HomeController.cs* için *denetleyicileri* klasör.

![Yeni öğe iletişim kutusu Ekle](mvc/_static/add_mvc_ctl.png)

* Ekleme bir *görünümleri* klasör.

* Ekleme bir *görünümler/giriş* klasör.

* Ekleme bir **Razor Görünüm** adlı *Index.cshtml* için *görünümler/giriş* klasör.

![Yeni öğe iletişim kutusu Ekle](mvc/_static/view.png)

Proje yapısını aşağıda gösterilmiştir:

![Dosya ve klasörleri WebApp1 gösteren Çözüm Gezgini](mvc/_static/project-structure-controller-view.png)

Öğesinin içeriğini değiştirin *Views/Home/Index.cshtml* aşağıdaki dosya:

```html
<h1>Hello world!</h1>
```

Uygulamayı çalıştırın.

![Microsoft Edge'de açık Web uygulaması](mvc/_static/hello-world.png)

Bkz: [denetleyicileri](xref:mvc/controllers/actions) ve [görünümleri](xref:mvc/views/overview) daha fazla bilgi için.

En az bir çalışan ASP.NET Core projesi sahibiz, biz işlevselliği ASP.NET MVC projeden geçiş başlatabilirsiniz. Aşağıdaki taşımanız gerekir:

* istemci tarafı içeriği (CSS, yazı tipleri ve betikler)

* denetleyiciler

* görünümler

* modeller

* Paketleme

* filtreler

* Giren/çıkan günlüğü, kimlik (Bu, sonraki öğreticide gerçekleştirilir.)

## <a name="controllers-and-views"></a>Denetleyicileri ve görünümleri

* ASP.NET MVC yöntemlerinin her birini kopyalayın `HomeController` yeni `HomeController`. ASP.NET MVC'de yerleşik şablonun denetleyici eylem yöntemi dönüş türü olduğunu unutmayın [actionresult öğesini](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); ASP.NET Core MVC, eylem yöntemleri dönüş `IActionResult` yerine. `ActionResult` uygulayan `IActionResult`var. Bu nedenle, eylem yöntemleri dönüş türünü değiştirmenize gerek yoktur.

* Kopyalama *About.cshtml*, *Contact.cshtml*, ve *Index.cshtml* ASP.NET MVC projesi Razor görünüm dosyaları ASP.NET Core projesi.

* ASP.NET Core uygulaması çalıştırın ve her yöntem test edin. İşlenmiş görünümler yalnızca görünümü dosya içeriklerinde içerir, böylece biz Düzen dosyası ya da stilleri henüz geçişi henüz. Düzen oluşturulan dosya bağlantılarını olmaz `About` ve `Contact` görünümleri tarayıcıdan çağrılacak gerekir (Değiştir **4492** projenizde kullanılan bağlantı noktası numarası ile).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kişi sayfası](mvc/_static/contact-page.png)

Stil ve menü öğeleri eksikliği unutmayın. Sonraki bölümde, gidereceğiz.

## <a name="static-content"></a>Statik içerik

ASP.NET MVC önceki sürümlerinde, statik içerik web projesinin kökünden barındırılan ve sunucu tarafı dosyaları ile karıştırılmış,. ASP.NET Core, statik içerik içinde barındırılan *wwwroot* klasör. Eski ASP.NET MVC uygulamanıza statik içeriği kopyalamak istersiniz *wwwroot* ASP.NET Core proje klasöründe. Bu örnek dönüştürme:

* Kopyalama *favicon.ico* eski MVC projesini dosyasından *wwwroot* ASP.NET Core projesi klasöründe.

ASP.NET MVC eski proje kullandığı [önyükleme](https://getbootstrap.com/) önyükleme dosyaları, stil ve depoları *içerik* ve *betikleri* klasörleri. Eski ASP.NET MVC projesi oluşturulan şablonu Düzen dosyası içinde önyükleme başvuruyor (*Views/Shared/_Layout.cshtml*). Konumuna yüklenemedi *bootstrap.js* ve *bootstrap.css* ASP.NET MVC dosyalarından proje için *wwwroot* klasöründe yeni bir proje. Sonraki bölümde CDN'ler kullanarak Bootstrap için destek (ve diğer istemci tarafı kitaplıkları) bunun yerine, ekleyeceğiz.

## <a name="migrate-the-layout-file"></a>Düzen dosyası geçirme

* Kopyalama *_ViewStart.cshtml* eski ASP.NET MVC proje dosyasından *görünümleri* ASP.NET Core proje klasörüne *görünümleri* klasör. *_ViewStart.cshtml* dosya içinde ASP.NET Core MVC değişmemiştir.

* Oluşturma bir *görünümler/paylaşılan* klasör.

* *İsteğe bağlı:* Kopyalama *_viewımports.cshtml* gelen *FullAspNetCore* MVC projenin *görünümleri* ASP.NET Core proje klasörüne *görünümleri* klasör. Herhangi bir ad alanı bildiriminde kaldırmak *_viewımports.cshtml* dosya. *_Viewımports.cshtml* dosya ad alanları için tüm görünüm dosyaları sağlar ve getirdiği [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro). Etiket Yardımcıları yeni bir düzen dosyasında kullanılır. *_Viewımports.cshtml* dosya ASP.NET Core için yenidir.

* Kopyalama *_Layout.cshtml* eski ASP.NET MVC proje dosyasından *görünümler/paylaşılan* ASP.NET Core proje klasörüne *görünümler/paylaşılan* klasör.

Açık *_Layout.cshtml* dosya ve (tamamlanan kodu aşağıda gösterilmektedir) aşağıdaki değişiklikleri yapın:

* Değiştirin `@Styles.Render("~/Content/css")` ile bir `<link>` yüklenecek öğe *bootstrap.css* (aşağıya bakın).

* Kaldırma `@Scripts.Render("~/bundles/modernizr")`.

* Açıklama `@Html.Partial("_LoginPartial")` satır (satırla çevreleyen `@*...*@`). Daha fazla bilgi için [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core](xref:migration/identity)

* Değiştirin `@Scripts.Render("~/bundles/jquery")` ile bir `<script>` öğesi (aşağıya bakın).

* Değiştirin `@Scripts.Render("~/bundles/bootstrap")` ile bir `<script>` öğesi (aşağıya bakın).

Değiştirme biçimlendirme önyükleme CSS eklemek için:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

JQuery ve önyükleme JavaScript ekleme için değiştirme biçimlendirme:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

Güncelleştirilmiş *_Layout.cshtml* dosya aşağıda gösterilmektedir:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Site tarayıcıda görüntüleme. Bunu artık doğru yerde beklenen stilleri ile yüklemeniz gerekir.

* *İsteğe bağlı:* Yeni düzen dosyası kullanarak denemek isteyebilirsiniz. Bu proje için Düzen dosyasından kopyalayabilirsiniz *FullAspNetCore* proje. Yeni düzen dosyası kullanan [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve diğer iyileştirmeler yapılmıştır.

## <a name="configure-bundling-and-minification"></a>Paketleme ve küçültme yapılandırın

Paketleme ve küçültme yapılandırma hakkında daha fazla bilgi için bkz: [paketleme ve küçültme](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>HTTP 500 hataları çözün

Sorunun kaynağını hakkında hiçbir bilgi içeren bir HTTP 500 hata iletisini neden olabilecek çok sayıda sorunları vardır. Örneğin, varsa *Views/_ViewImports.cshtml* dosya içeriyorsa, projede mevcut bir ad alanı, bir HTTP 500 hata alırsınız. Varsayılan olarak, ASP.NET Core uygulamalarında `UseDeveloperExceptionPage` uzantısı eklenmiş `IApplicationBuilder` ve yapılandırma olduğunda yürütülen *geliştirme*. Bu aşağıdaki kodda ayrıntılı olarak verilmiştir:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core web uygulamasında işlenmeyen özel durumları HTTP 500 hata yanıtları dönüştürür. Normalde, hata ayrıntıları sunucu ile ilgili potansiyel olarak hassas bilgilerin açığa çıkmasını önlemek için bu yanıtları dahil değildir. Bkz: **Geliştirici özel durumu sayfasını kullanarak** içinde [hataları işlemek](../fundamentals/error-handling.md) daha fazla bilgi için.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
