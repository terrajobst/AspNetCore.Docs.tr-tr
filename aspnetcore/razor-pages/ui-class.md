---
title: ASP.NET Core Sınıf Kitaplığı'nda yeniden kullanılabilir Razor UI
author: Rick-Anderson
description: Yeniden kullanılabilir Razor kısmi görünümler, ASP.NET Core sınıf kitaplığında kullanarak kullanıcı Arabirimi oluşturma açıklanır.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 1a20b136203921e6a147058eb34da6dcbb7863e7
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2020
ms.locfileid: "76971989"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>ASP.NET Core 'de Razor Sınıf Kitaplığı projesini kullanarak yeniden kullanılabilir kullanıcı arabirimi oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir. RCL paketlenir ve yeniden kullanılabilir. Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar. Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Razor UI içeren bir sınıf kitaplığı oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio 'dan **Yeni bir proje oluştur**' u seçin.
* **Razor sınıfı kitaplığı** > **İleri ' yi**seçin.
* Kitaplığı adlandırın (örneğin, "RazorClassLib"), > **Oluştur**. Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.
* Görünümleri desteketmeniz gerekiyorsa **destek sayfaları ve görünümleri '** ni seçin. Varsayılan olarak yalnızca Razor Pages desteklenir. Seçin **oluşturma**.

Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler. **Destek sayfaları ve görünümleri** seçeneği sayfaları ve görünümleri destekler.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından çalıştırmak `dotnet new razorclasslib`. Örneğin:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler. Sayfalar ve görünümler için destek sağlamak üzere `--support-pages-and-views` seçeneğini (`dotnet new razorclasslib --support-pages-and-views`) geçirin.

Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new). Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.

---

Razor dosyaları için RCL ekleyin.

ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör. `~/Areas/Pages`yerine `~/Pages` içeriği kullanıma sunan bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.

## <a name="reference-rcl-content"></a>RCL içeriğine başvur

RCL tarafından başvurulabilir:

* NuGet paketi. Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName} .csproj*. Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).

## <a name="override-views-partial-views-and-pages"></a>Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl

Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir. Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.

Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.

Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Yeni bir konum belirtmek için işaretleme güncelleştirin. Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.

### <a name="rcl-pages-layout"></a>RCL sayfa düzeni

RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:

* *RazorUIClassLib/sayfaları*
* *RazorUIClassLib/sayfalar/paylaşılan*

Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*. `<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>Statik varlıklar içeren bir RCL oluşturma

RCL, RCL 'nin RCL veya tüketen uygulaması tarafından başvurulabilen, yardımcı statik varlıklar gerektirebilir. ASP.NET Core, tüketen bir uygulama tarafından kullanılabilen statik varlıkları içeren RCLs oluşturulmasına izin verir.

Yardımcı varlıkları RCL 'nin bir parçası olarak dahil etmek için, sınıf kitaplığında bir *Wwwroot* klasörü oluşturun ve gerekli dosyaları bu klasöre ekleyin.

RCL 'yi paketleyerek, *Wwwroot* klasöründeki tüm yardımcı varlıklar pakete otomatik olarak eklenir.

### <a name="exclude-static-assets"></a>Statik varlıkları hariç tut

Statik varlıkları dışlamak için, istenen dışlama yolunu proje dosyasındaki `$(DefaultItemExcludes)` özellik grubuna ekleyin. Girişleri noktalı virgülle ayırın (`;`).

Aşağıdaki örnekte, *Wwwroot* klasöründeki *lib. css* stil sayfası statik bir varlık olarak değerlendirilmez ve yayımlanan RCL 'ye dahil değildir:

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a>TypeScript tümleştirmesi

TypeScript dosyalarını RCL 'ye eklemek için:

1. TypeScript dosyalarını ( *. TS*) *Wwwroot* klasörünün dışına yerleştirin. Örneğin, dosyaları bir *istemci* klasörüne yerleştirin.

1. *Wwwroot* klasörü için TypeScript derleme çıkışını yapılandırın. Proje dosyasındaki bir `PropertyGroup` içindeki `TypescriptOutDir` özelliğini ayarlayın:

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. Proje dosyasındaki bir `PropertyGroup` içine aşağıdaki hedefi ekleyerek TypeScript hedefini `ResolveCurrentProjectStaticWebAssets` hedefinin bir bağımlılığı olarak ekleyin:

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a>Başvurulan bir RCL 'den içerik tüketme

RCL 'nin *Wwwroot* klasörüne eklenen dosyalar, ön ek `_content/{LIBRARY NAME}/`RCL veya tüketen uygulamaya sunulur. Örneğin, *Razor. Class. lib* adlı bir kitaplık `_content/Razor.Class.Lib/`statik içerik yolu ile sonuçlanır. Bir NuGet paketi üretilirken ve derleme adı paket KIMLIĞIYLE aynı değilse, `{LIBRARY NAME}`için paket KIMLIĞINI kullanın.

Tüketen uygulama, kitaplık tarafından `<script>`, `<style>`, `<img>`ve diğer HTML etiketleriyle sunulan statik varlıklara başvurur. Tüketim uygulaması `Startup.Configure`içinde [statik dosya desteğinin](xref:fundamentals/static-files) etkinleştirilmiş olması gerekir:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

Yapı çıktısından (`dotnet run`) tüketen uygulamayı çalıştırırken, varsayılan olarak, statik Web varlıkları geliştirme ortamında etkinleştirilmiştir. Yapı çıktısından çalışırken diğer ortamlardaki varlıkları desteklemek için, *program.cs*' deki konak oluşturucusu 'nda `UseStaticWebAssets` çağırın:

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

Yayımlanan çıktıdan bir uygulama çalıştırılırken çağrı `UseStaticWebAssets` gerekmez (`dotnet publish`).

### <a name="multi-project-development-flow"></a>Çoklu projeli geliştirme akışı

Kullanan uygulama şu şekilde çalışır:

* RCL içindeki varlıklar özgün klasörlerinde kalır. Varlıklar, tüketim uygulamasına taşınmaz.
* RCL 'nin *Wwwroot* klasörü içindeki tüm değişiklikler, RCL yeniden oluşturulduktan ve tüketen uygulamayı yeniden oluşturmadan önce tüketen uygulamaya yansıtılır.

RCL yapılandırıldığında, statik Web varlık konumlarını açıklayan bir bildirim oluşturulur. Tüketen uygulama, başvurulan proje ve paketlerden varlıkları kullanmak için çalışma zamanında bildirimi okur. Bir RCL 'ye yeni bir varlık eklendiğinde, bir uygulamanın yeni varlığa erişebilmesi için bildirim güncellemek üzere RCL 'nin yeniden oluşturulması gerekir.

### <a name="publish"></a>Yayımla

Uygulama yayımlandığında, tüm başvurulan projeler ve paketlerin yardımcı varlıkları `_content/{LIBRARY NAME}/`altında Yayınlanan uygulamanın *Wwwroot* klasörüne kopyalanır.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir. RCL paketlenir ve yeniden kullanılabilir. Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar. Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Razor UI içeren bir sınıf kitaplığı oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* (Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**. Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* **Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.

RCL aşağıdaki proje dosyasına sahiptir:

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından çalıştırmak `dotnet new razorclasslib`. Örneğin:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new). Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.

---

Razor dosyaları için RCL ekleyin.

ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör. `~/Areas/Pages`yerine `~/Pages` içeriği kullanıma sunan bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.

## <a name="reference-rcl-content"></a>RCL içeriğine başvur

RCL tarafından başvurulabilir:

* NuGet paketi. Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName} .csproj*. Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>İzlenecek yol: bir Razor Pages projesinden bir RCL projesi oluşturma ve kullanma

İndirebileceğiniz [tam proje](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ve yerine oluşturma test edin. Örnek indirme, ek kod ve test etmek proje kolaylaştırmak bağlantılar içerir. Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) yorumlarınızı üzerinde adım adım yönergeler ve örnekleri indirme ile.

### <a name="test-the-download-app"></a>İndirme uygulamayı test etme

Tamamlanmış uygulamayı karşıdan henüz ve bunun yerine izlenecek projesi oluşturacak, atlamak [sonraki bölümde](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Açık *.sln* dosyasını Visual Studio'da. Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Bir komut isteminden *CLI* dizin RCL oluşturun ve web uygulaması.

```dotnetcli
dotnet build
```

Taşı *WebApp1* dizin ve uygulamayı çalıştırın:

```dotnetcli
dotnet run
```

---

Bölümündeki yönergeleri [Test WebApp1](#test-webapp1)

## <a name="create-an-rcl"></a>RCL oluşturma

Bu bölümde bir RCL oluşturulur. Razor dosyaları için RCL eklenir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

RCL projesi oluşturun:

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* > **Tamam**, uygulamayı **RazorUIClassLib** olarak adlandırın.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* **Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.
* Razor kısmi Görünüm adlı bir dosya ekleyin *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından şu komutu çalıştırın:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Yukarıdaki komutlar:

* RCL `RazorUIClassLib` oluşturur.
* Bir Razor il_eti sayfası oluşturur ve için RCL ekler. `-np` Parametresi olmadan sayfa oluşturur bir `PageModel`.
* Oluşturur bir [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) dosya ve için RCL ekler.

*_ViewStart.cshtml* dosyası (sonraki bölümde eklenir) Razor sayfaları proje düzenini kullanmak için gereklidir.

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Razor dosyaları ve klasörleri projeye ekleyin.

* Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`). Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya. Örneğin:

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  Daha fazla bilgi için *_viewımports.cshtml*, bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)

* Derleyici hata doğrulamak için sınıf kitaplığı derleme:

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

Derleme çıktısını içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* derlenmiş Razor içeriği.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Bir Razor sayfaları projeden Razor kullanıcı Arabirimi kitaplığını kullanma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Razor sayfaları web uygulaması oluşturun:

* **Çözüm Gezgini**> çözüme sağ tıklayıp **Yeni >proje** **Ekle** ' ye tıklayın.
* Seçin **ASP.NET Core Web uygulaması**.
* Uygulamayı adlandırma **WebApp1**.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* **Web uygulaması** > **Tamam ' ı**seçin.

* Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **başlangıç projesi olarak ayarla**.
* **Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **Proje bağımlılıkları**> **derleme bağımlılıkları** ' nı seçin.
* Denetleme **RazorUIClassLib** bağımlılık olarak **WebApp1**.
* **Çözüm Gezgini**, **WebApp1** ' a sağ tıklayıp > **başvuru** **Ekle** ' yi seçin.
* **Başvuru Yöneticisi** Iletişim kutusunda **RazorUIClassLib** > **Tamam**' ı işaretleyin.

Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Razor Pages uygulamasını ve RCL 'yi içeren bir Razor Pages Web uygulaması ve çözüm dosyası oluşturun:

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Web uygulaması derleyebilir ve:

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a>Test WebApp1

Razor UI sınıfı kitaplığının kullanımda olduğunu doğrulamak için `/MyFeature/Page1` gidin.

## <a name="override-views-partial-views-and-pages"></a>Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl

Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir. Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.

Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.

Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Yeni bir konum belirtmek için işaretleme güncelleştirin. Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.

### <a name="rcl-pages-layout"></a>RCL sayfa düzeni

RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:

* *RazorUIClassLib/sayfaları*
* *RazorUIClassLib/sayfalar/paylaşılan*

Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*. `<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
