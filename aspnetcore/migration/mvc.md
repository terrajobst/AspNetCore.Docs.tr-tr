---
title: "ASP.NET MVC ASP.NET Core MVC geçirme"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 88e5b7575930434e291a7aa4daef429306c653e0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>ASP.NET MVC ASP.NET Core MVC geçirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), ve [Scott Addie](https://scottaddie.com)

Bu makalede, ASP.NET MVC projesinde geçirme başlamak gösterilmiştir [ASP.NET Core MVC](../mvc/overview.md). İşlem sırasında çoğunu ASP.NET MVC değişen vurgular. ASP.NET MVC geçiş birden çok adım bir işlemdir ve bu makalede, ilk Kurulum, temel denetleyicileri ve görünümler, statik içerik ve istemci tarafı bağımlılıkları yer almaktadır. Diğer makaleler geçirme yapılandırması ve kimlik kodu birçok ASP.NET MVC projelerinde bulunamadı kapsar.

> [!NOTE]
> Sürüm numaraları örneklerdeki geçerli olmayabilir. Buna göre projelerinizi güncelleştirmeniz gerekebilir.

## <a name="create-the-starter-aspnet-mvc-project"></a>ASP.NET MVC projesinin starter oluşturma

Yükseltme göstermek için bir ASP.NET MVC uygulaması oluşturarak başlayacağız. Adlı oluşturun *WebApp1* ad alanı sonraki adımda oluşturuyoruz ASP.NET Core proje eşleşecek şekilde.

![Visual Studio yeni proje iletişim kutusu](mvc/_static/new-project.png)

![Yeni Web uygulaması iletişim kutusu: ASP.NET şablonları panelinde seçili MVC proje şablonu](mvc/_static/new-project-select-mvc-template.png)

*İsteğe bağlı:* çözümden adını değiştirmek *WebApp1* için *Mvc5*. Visual Studio yeni çözüm adı görüntülenir (*Mvc5*), hangi kolaylaştırır sonraki proje bu projeden söyleyin.

## <a name="create-the-aspnet-core-project"></a>ASP.NET Core projesi oluşturma

Yeni bir *boş* ASP.NET Core web uygulaması önceki projesi olarak aynı ada sahip (*WebApp1*) ad alanları iki proje ile eşleşecek şekilde. Aynı ad alanına sahip iki projeler arasında kodu kopyalamak kolaylaştırır. Aynı adı kullanmak üzere önceki proje daha farklı bir dizinde bu projeyi oluşturmak zorunda kalırsınız.

![Yeni Proje iletişim kutusu](mvc/_static/new_core.png)

![Yeni ASP.NET Web uygulaması iletişim kutusu: ASP.NET Core şablonları panelinde seçili boş proje şablonu](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *İsteğe bağlı:* kullanarak yeni bir ASP.NET Core uygulama oluştur *Web uygulaması* proje şablonu. Proje adı *WebApp1*, bir kimlik doğrulama seçeneğini seçip **tek tek kullanıcı hesaplarını**. Bu uygulama yeniden adlandırma *FullAspNetCore*. Bu proje zaman kazanabilirsiniz dönüştürmede oluşturuluyor. Sonuç görmek için veya dönüştürme projeye kodu kopyalamak için ' şablon oluşturulan kod bakabilirsiniz. Şablon tarafından oluşturulan proje ile karşılaştırmak için dönüştürme adım sıkışan gerektiğinde de yararlıdır.

## <a name="configure-the-site-to-use-mvc"></a>Siteyi MVC kullanacak şekilde yapılandırma

* Yükleme `Microsoft.AspNetCore.Mvc` ve `Microsoft.AspNetCore.StaticFiles` NuGet paketleri.

  `Microsoft.AspNetCore.Mvc`ASP.NET Core MVC çerçevedir. `Microsoft.AspNetCore.StaticFiles`statik dosya işleyici olur. ASP.NET çalışma zamanı modüler ve açıkça statik dosyaları işleme için kabul gerekir (bkz [statik dosyaları ile çalışma](../fundamentals/static-files.md)).

* Açık *.csproj* dosyası ('nde projeye sağ **Çözüm Gezgini** seçip **Düzenle WebApp1.csproj**) ve ekleme bir `PrepareForPublish` hedef:

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  `PrepareForPublish` Hedef istemci-tarafı kitaplıkları Bower aracılığıyla alınırken için gereklidir. Biz hakkında daha sonra konuşun.

* Açık *haline* dosya ve kodu aşağıdaki ile eşleşecek şekilde değiştirin:

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  `UseStaticFiles` Genişletme yöntemi statik dosya işleyici ekler. Daha önce belirtildiği gibi ASP.NET çalışma zamanı modüler ve açıkça statik dosyaları işleme için kabul gerekir. `UseMvc` Genişletme yöntemi ekler yönlendirme. Daha fazla bilgi için bkz: [uygulama başlangıç](../fundamentals/startup.md) ve [yönlendirme](../fundamentals/routing.md).

## <a name="add-a-controller-and-view"></a>Bir denetleyici ve Görünüm Ekle

Bu bölümde, bir en az denetleyici ve görünüm ASP.NET MVC denetleyicisi yer tutucular olarak hizmet verecek ve sonraki bölümde geçirmek görünümleri ekleyeceksiniz.

* Ekleme bir *denetleyicileri* klasör.

* Ekleme bir **MVC denetleyicisi sınıfı** adıyla *HomeController.cs* için *denetleyicileri* klasör.

![Yeni öğe iletişim ekleyin](mvc/_static/add_mvc_ctl.png)

* Ekleme bir *görünümleri* klasör.

* Ekleme bir *görünümler/giriş* klasör.

* Ekleme bir *Index.cshtml* MVC görünüm sayfası *görünümler/giriş* klasör.

![Yeni öğe iletişim ekleyin](mvc/_static/view.png)

Proje yapısı aşağıda gösterilmiştir:

![Dosya ve klasörleri WebApp1 gösteren Çözüm Gezgini](mvc/_static/project-structure-controller-view.png)

Değiştir *Views/Home/Index.cshtml* aşağıdaki dosyasıyla:

```html
<h1>Hello world!</h1>
```

Uygulamayı çalıştırın.

![Web uygulaması Microsoft Edge'de Aç](mvc/_static/hello-world.png)

Bkz: [denetleyicileri](../mvc/controllers/index.md) ve [görünümleri](../mvc/views/index.md) daha fazla bilgi için.

En az bir çalışma ASP.NET Core proje sahibiz, biz işlevselliği ASP.NET MVC projesinden geçiş başlatabilirsiniz. Aşağıdaki taşımanız gerekebilir:

* istemci-tarafı içeriği (CSS, yazı tipleri ve komut dosyaları)

* denetleyiciler

* görünümler

* modeller

* Paketleme

* filtreler

* Giriş/Çıkış günlüğü, kimlik (Bu sonraki öğreticide yapılır.)

## <a name="controllers-and-views"></a>Denetleyicileri ve görünümler

* Yöntemlerin her biri ASP.NET MVC kopyalama `HomeController` yeni `HomeController`. ASP.NET MVC uygulamasında yerleşik şablonun denetleyici eylem yönteminin dönüş türü olduğunu unutmayın [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); ASP.NET mvc'de çekirdek, dönüş eylem yöntemleri `IActionResult` yerine. `ActionResult`uygulayan `IActionResult`, bu yüzden, eylem yöntemleri dönüş türü değiştirmenize gerek yoktur.

* Kopya *About.cshtml*, *Contact.cshtml*, ve *Index.cshtml* ASP.NET Core proje için ASP.NET MVC projesindeki Razor görünüm dosyaları.

* ASP.NET Core uygulamayı çalıştırın ve her yöntemi sınayın. İşlenmiş görünüm yalnızca görünüm dosyalarının içeriği içerecek şekilde biz düzeni dosyasını veya stiller henüz, geçirilen henüz. Düzen oluşturulan dosyanın bağlantılarını olmayacaktır `About` ve `Contact` tarayıcıdan çağırma gerekecek şekilde görünümler (Değiştir **4492** projenizde kullanılan bağlantı noktası numarası ile).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kişi sayfası](mvc/_static/contact-page.png)

Stil ve menü öğelerini eksikliği unutmayın. Biz, sonraki bölümde düzeltmesi.

## <a name="static-content"></a>Statik içerik

ASP.NET MVC önceki sürümlerinde, statik içerik web projesi kökünden barındırılan ve sunucu tarafı dosyalarıyla intermixed. ASP.NET çekirdek statik içerik içinde barındırılan *wwwroot* klasör. Statik içerik eski ASP.NET MVC uygulamanıza kopyalamak istersiniz *wwwroot* ASP.NET Core projenizdeki klasöre. Bu örnek dönüştürmede:

* Kopya *favicon.ico* eski MVC proje dosyasından *wwwroot* ASP.NET Core proje klasöründe.

ASP.NET MVC eski proje kullanır [önyükleme](http://getbootstrap.com/) önyükleme dosyaları stil ve depoları *içerik* ve *betikleri* klasörler. Önyükleme düzeni dosyasında eski ASP.NET MVC projesinin oluşturulan, şablonu başvuruyor (*Views/Shared/_Layout.cshtml*). Kopyalamak *bootstrap.js* ve *bootstrap.css* ASP.NET MVC dosyalarından proje için *wwwroot* değil klasörünü yeni proje, ancak bu yaklaşımı kullanın ASP.NET Core istemci-tarafı bağımlılıkları yönetmek için geliştirilmiş mekanizması.

Yeni projede kullanarak önyükleme (ve diğer istemci-tarafı kitaplıklar) desteği ekleyeceğiz [Bower](https://bower.io/):

* Ekle bir [Bower](https://bower.io/) yapılandırma dosyası adlı *bower.json* proje kök (projeye sağ tıklayın ve ardından **Ekle > Yeni Öğe > Bower yapılandırma dosyası**). Ekleme [önyükleme](http://getbootstrap.com/) ve [jQuery](https://jquery.com/) dosyasına (aşağıda vurgulanan satırlar bakın).

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

Dosyayı kaydettikten sonra Bower otomatik olarak bağımlılıkları indirir *wwwroot/lib* klasör. Kullanabileceğiniz **arama Çözüm Gezgini** kutusunu varlıklar yolunu bulmak için:

![Çözüm Gezgini arama sonuçlarında gösterilen jquery varlıklar](mvc/_static/search.png)

Bkz: [istemci-tarafı paketlerle yönetmek Bower](../client-side/bower.md) daha fazla bilgi için.

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>Düzen dosyasını geçirme

* Kopya *_ViewStart.cshtml* eski ASP.NET MVC projesinin dosyadan *görünümleri* ASP.NET Core projenin klasörüne *görünümleri* klasör. *_ViewStart.cshtml* dosya ASP.NET Core MVC'de değişmemiştir.

* Oluşturma bir *görünümler/paylaşılan* klasör.

* *İsteğe bağlı:* kopya *_viewımports.cshtml* gelen *FullAspNetCore* MVC projesinin *görünümleri* ASP.NET Core projenin klasörüne*Görünümleri* klasör. Tüm ad alanı bildirimini kaldırın *_viewımports.cshtml* dosya. *_Viewımports.cshtml* dosya ad alanları için tüm görünüm dosyaları sağlar ve getirir [etiket Yardımcıları](../mvc/views/tag-helpers/index.md). Etiket Yardımcıları yeni düzen dosyasında kullanılır. *_Viewımports.cshtml* dosya ASP.NET Core için yenidir.

* Kopya *_Layout.cshtml* eski ASP.NET MVC projesinin dosyadan *görünümler/paylaşılan* ASP.NET Core projenin klasörüne *görünümler/paylaşılan* klasör.

Açık *_Layout.cshtml* dosya ve (tamamlanan kodu aşağıda gösterilmektedir) aşağıdaki değişiklikleri yapın:

   * Değiştir `@Styles.Render("~/Content/css")` ile bir `<link>` yüklemek için öğesi *bootstrap.css* (aşağıya bakın).

   * Kaldırma `@Scripts.Render("~/bundles/modernizr")`.

   * Out açıklama `@Html.Partial("_LoginPartial")` satır (satırıyla çevreleyen `@*...*@`). Sonraki öğreticide kendisine getireceğiz.

   * Değiştir `@Scripts.Render("~/bundles/jquery")` ile bir `<script>` öğesi (aşağıya bakın).

   * Değiştir `@Scripts.Render("~/bundles/bootstrap")` ile bir `<script>` öğesi (aşağıya bakın)...

Değiştirme CSS Bağlantısı:

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

Değiştirme komut dosyası etiketi:

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

Güncelleştirilmiş *_Layout.cshtml* dosya aşağıda gösterilmektedir:

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

Site tarayıcıda görüntüleyin. Artık doğru yerde beklenen stilleri ile yüklenmesi gerekir.

* *İsteğe bağlı:* yeni düzen dosyasını kullanmayı denemek isteyebilirsiniz. Bu proje için Düzen dosyasından kopyalayabilirsiniz *FullAspNetCore* projesi. Yeni düzen dosyasını kullanan [etiket Yardımcıları](../mvc/views/tag-helpers/index.md) ve diğer geliştirmeler sahiptir.

## <a name="configure-bundling--minification"></a>Paketleme yapılandırmak & küçültme

Paketleme ve küçültme yapılandırma hakkında daha fazla bilgi için bkz: [paketleme ve küçültme](../client-side/bundling-and-minification.md).

## <a name="solving-http-500-errors"></a>HTTP 500 hataları çözme

Sorunun kaynağını hakkında hiçbir bilgi içeren bir HTTP 500 hata iletisini neden olan birçok sorunları vardır. Örneğin, varsa *Views/_ViewImports.cshtml* dosya içeriyorsa, projede mevcut olmayan bir ad alanı, bir HTTP 500 hata iletisi alırsınız. Ayrıntılı hata iletisini almak için aşağıdaki kodu ekleyin:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

Bkz: **Geliştirici özel durum sayfasını kullanarak** içinde [işleme hatası](../fundamentals/error-handling.md) daha fazla bilgi için.

## <a name="additional-resources"></a>Ek Kaynaklar

* [İstemci-tarafı geliştirme](../client-side/index.md)

* [Etiket Yardımcıları](../mvc/views/tag-helpers/index.md)
