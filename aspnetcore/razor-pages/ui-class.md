---
title: ASP.NET Core ile sınıf kitaplıklarında yeniden kullanılabilir Razor Kullanıcı arabirimi
author: Rick-Anderson
description: ASP.NET Core bir sınıf kitaplığında kısmi görünümler kullanarak yeniden kullanılabilir Razor Kullanıcı arabirimi oluşturmayı açıklar.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: dcd24f7dafd198f88cdf84d1ab67c84f45428a95
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179328"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>ASP.NET Core 'de Razor Sınıf Kitaplığı projesini kullanarak yeniden kullanılabilir kullanıcı arabirimi oluşturma

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir. RCL paketlenebilir ve yeniden kullanılabilir. Uygulamalar RCL 'yi içerebilir ve içerdiği görünümleri ve sayfaları geçersiz kılabilir. Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Razor Kullanıcı arabirimi içeren bir sınıf kitaplığı oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması**' nı seçin.
* Kitaplığı adlandırın (örneğin, "RazorClassLib") > **Tamam**. Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.
* **ASP.NET Core 3,0** veya sonraki bir sürümü seçildiğini doğrulayın.
* **Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.

Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler. Visual Studio 'daki bir şablon seçeneği, sayfalar ve görünümler için şablon desteği sağlar.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından `dotnet new razorclasslib` ' ı çalıştırın. Örnek:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler. Sayfalar ve görünümler için destek sağlamak üzere `--support-pages-and-views` seçeneğini (`dotnet new razorclasslib --support-pages-and-views`) geçirin.

Daha fazla bilgi için bkz. [DotNet New](/dotnet/core/tools/dotnet-new). Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.

---

RCL 'ye Razor dosyaları ekleyin.

ASP.NET Core şablonları RCL içeriğinin *Areas* klasöründe olduğunu varsayar. @No__t-2 yerine `~/Pages` ' de içerik açığa çıkaran bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.

## <a name="reference-rcl-content"></a>RCL içeriğine başvur

RCL 'ye şu şekilde başvurulabilir:

* NuGet paketi. Bkz. [NuGet paketleri oluşturma](/nuget/create-packages/creating-a-package) ve [DotNet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [bir NuGet paketi oluşturma ve yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}. csproj*. Bkz. [DotNet-başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).

## <a name="override-views-partial-views-and-pages"></a>Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl

Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir. Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.

Örnek indirme içinde *WebApp1/Areas/MyFeature2* öğesini *WebApp1/Areas/myfeature* olarak yeniden adlandırın ve test önceliğini belirtin.

*RazorUIClassLib/Areas/myfeature/Pages/Shared/_Message. cshtml* kısmi görünümünü *WebApp1/Areas/Myfeature/Pages/Shared/_Message. cshtml*'ye kopyalayın. Biçimlendirmeyi yeni konumu belirtecek şekilde güncelleştirin. Uygulamanın kısmi sürümünün kullanılmakta olduğunu doğrulamak için uygulamayı derleyin ve çalıştırın.

### <a name="rcl-pages-layout"></a>RCL sayfaları düzeni

RCL içeriğine, Web uygulamasının *Sayfalar* klasörünün bir parçası olmasına rağmen başvurmak için, aşağıdaki dosya yapısıyla RCL projesini oluşturun:

* *RazorUIClassLib/sayfalar*
* *RazorUIClassLib/sayfalar/paylaşılan*

*RazorUIClassLib/Pages/Shared* iki kısmi dosya Içerir: *_header. cshtml* ve *_footer. cshtml*. @No__t-0 etiketleri *_Layout. cshtml* dosyasına eklenebilir:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>Statik varlıklar içeren bir RCL oluşturma

RCL, RCL 'nin tüketen uygulaması tarafından başvurulabilen, yardımcı statik varlıklar gerektirebilir. ASP.NET Core, tüketen bir uygulama tarafından kullanılabilen statik varlıkları içeren RCLs oluşturulmasına izin verir.

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

1. *Wwwroot* klasörü için TypeScript derleme çıkışını yapılandırın. Proje dosyasında bir `PropertyGroup` içinde `TypescriptOutDir` özelliğini ayarlayın:

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. Proje dosyasında bir `PropertyGroup` içine aşağıdaki hedefi ekleyerek TypeScript hedefini `ResolveCurrentProjectStaticWebAssets` hedefinin bir bağımlılığı olarak ekleyin:

   ```xml
  <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
    CompileTypeScript;
    $(ResolveCurrentProjectStaticWebAssetsInputs)
  </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a>Başvurulan bir RCL 'den içerik tüketme

RCL 'nin *Wwwroot* klasörüne eklenen dosyalar, `_content/{LIBRARY NAME}/` öneki altında tüketen uygulamaya sunulur. Örneğin, *Razor. Class. lib* adlı bir kitaplık `_content/Razor.Class.Lib/` ' de statik içerik yolu ile sonuçlanır.

Kullanan uygulama, kitaplık tarafından `<script>`, `<style>`, `<img>` ve diğer HTML etiketleriyle sunulan statik varlıklara başvurur. Tüketim uygulaması `Startup.Configure` ' de [statik dosya desteğinin](xref:fundamentals/static-files) etkin olması gerekir:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

Yapı çıktısından tüketen uygulamayı çalıştırırken (`dotnet run`), statik Web varlıkları geliştirme ortamında varsayılan olarak etkindir. Derleme çıktılarından çalışırken diğer ortamlardaki varlıkları desteklemek için, *program.cs*' deki konak oluşturucusu 'nda `UseStaticWebAssets` ' ı çağırın:

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

(@No__t-1) yayımlanmış çıkışdan bir uygulama çalıştırılırken, @no__t çağrısı yapılması gerekmez.

### <a name="multi-project-development-flow"></a>Çoklu projeli geliştirme akışı

Kullanan uygulama şu şekilde çalışır:

* RCL içindeki varlıklar özgün klasörlerinde kalır. Varlıklar, tüketim uygulamasına taşınmaz.
* RCL 'nin *Wwwroot* klasörü içindeki tüm değişiklikler, RCL yeniden oluşturulduktan ve tüketen uygulamayı yeniden oluşturmadan önce tüketen uygulamaya yansıtılır.

RCL yapılandırıldığında, statik Web varlık konumlarını açıklayan bir bildirim oluşturulur. Tüketen uygulama, başvurulan proje ve paketlerden varlıkları kullanmak için çalışma zamanında bildirimi okur. Bir RCL 'ye yeni bir varlık eklendiğinde, bir uygulamanın yeni varlığa erişebilmesi için bildirim güncellemek üzere RCL 'nin yeniden oluşturulması gerekir.

### <a name="publish"></a>Yayımlama

Uygulama yayımlandığında, tüm başvurulan projeler ve paketlerin yardımcı varlıkları `_content/{LIBRARY NAME}/` altında yayımlanan uygulamanın *Wwwroot* klasörüne kopyalanır.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir. RCL paketlenebilir ve yeniden kullanılabilir. Uygulamalar RCL 'yi içerebilir ve içerdiği görünümleri ve sayfaları geçersiz kılabilir. Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Razor Kullanıcı arabirimi içeren bir sınıf kitaplığı oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması**' nı seçin.
* Kitaplığı adlandırın (örneğin, "RazorClassLib") > **Tamam**. Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.
* **ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.
* **Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.

RCL aşağıdaki proje dosyasına sahiptir:

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından `dotnet new razorclasslib` ' ı çalıştırın. Örnek:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Daha fazla bilgi için bkz. [DotNet New](/dotnet/core/tools/dotnet-new). Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.

---

RCL 'ye Razor dosyaları ekleyin.

ASP.NET Core şablonları RCL içeriğinin *Areas* klasöründe olduğunu varsayar. @No__t-2 yerine `~/Pages` ' de içerik açığa çıkaran bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.

## <a name="reference-rcl-content"></a>RCL içeriğine başvur

RCL 'ye şu şekilde başvurulabilir:

* NuGet paketi. Bkz. [NuGet paketleri oluşturma](/nuget/create-packages/creating-a-package) ve [DotNet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [bir NuGet paketi oluşturma ve yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}. csproj*. Bkz. [DotNet-başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>İzlenecek yol: bir Razor Pages projesinden bir RCL projesi oluşturma ve kullanma

[Tüm projeyi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) indirebilir ve oluşturmak yerine test edebilirsiniz. Örnek indirme, projenin test olmasını kolaylaştıran ek kod ve bağlantılar içerir. Yükleme örnekleri ve adım adım yönergeler hakkındaki açıklamalarınızla [Bu GitHub sorunuyla](https://github.com/aspnet/AspNetCore.Docs/issues/6098) ilgili geri bildirimde bulunun.

### <a name="test-the-download-app"></a>İndirme uygulamasını test etme

Tamamlanmış uygulamayı indirmediyseniz ve gözden geçirme projesi oluşturmak istiyorsanız, [sonraki bölüme](#create-an-rcl)atlayın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio 'da *. sln* dosyasını açın. Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

*CLI* dizinindeki bir komut isteminden RCL ve Web uygulamasını oluşturun.

```dotnetcli
dotnet build
```

*WebApp1* dizinine gidin ve uygulamayı çalıştırın:

```dotnetcli
dotnet run
```

---

[Test WebApp1](#test-webapp1) içindeki yönergeleri izleyin

## <a name="create-an-rcl"></a>RCL oluşturma

Bu bölümde bir RCL oluşturulur. Razor dosyaları RCL 'ye eklenir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

RCL projesini oluşturun:

* Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması**' nı seçin.
* @No__t **-1 '** i uygulama **RazorUIClassLib** olarak adlandırın.
* **ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.
* **Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.
* *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message. cshtml*adlı bir Razor kısmi görünüm dosyası ekleyin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından aşağıdakileri çalıştırın:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Önceki komutlar:

* @No__t-0 RCL oluşturur.
* Bir Razor _Ileti sayfası oluşturur ve RCL 'ye ekler. @No__t-0 parametresi, `PageModel` olmadan sayfayı oluşturur.
* Bir [_Viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) dosyası oluşturur ve RCL 'ye ekler.

*_Viewstart. cshtml* dosyası, Razor Pages projenin (bir sonraki bölüme eklenen) düzeninin yerleşimini kullanmak için gereklidir.

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Projeye Razor dosyaları ve klasörleri ekleme

* *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message. cshtml* içindeki biçimlendirmeyi aşağıdaki kodla değiştirin:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* *RazorUIClassLib/Areas/MyFeature/Pages/Sayfa1. cshtml* içindeki biçimlendirmeyi aşağıdaki kodla değiştirin:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  kısmi görünümü kullanmak için `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` gereklidir (`<partial name="_Message" />`). @No__t-0 yönergesini dahil etmek yerine, *_Viewwimports. cshtml* dosyası ekleyebilirsiniz. Örnek:

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  *_Viewwimports. cshtml*hakkında daha fazla bilgi için bkz. [paylaşılan yönergeleri içeri aktarma](xref:mvc/views/layout#importing-shared-directives)

* Derleyici hatası olmadığını doğrulamak için sınıf kitaplığı oluşturun:

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

Derleme çıkışı *RazorUIClassLib. dll* ve *RazorUIClassLib. views. dll*içerir. *RazorUIClassLib. views. dll* derlenen Razor içeriğini içerir.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Razor Pages projesinden Razor Kullanıcı arabirimi kitaplığını kullanma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Razor Pages Web uygulaması oluşturun:

* **Çözüm Gezgini**>**yeni proje** **eklemek** > çözüme sağ tıklayın.
* **ASP.NET Core Web uygulaması**' nı seçin.
* Uygulamayı **WebApp1**olarak adlandırın.
* **ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.
* **Web uygulaması** @no__t seçin-1 **Tamam**.

* **Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **Başlangıç projesi olarak ayarla**' yı seçin.
* **Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **derleme bağımlılıkları** > **Proje bağımlılıkları**' nı seçin.
* **WebApp1**'in bağımlılığı olarak **RazorUIClassLib** denetleyin.
* **Çözüm Gezgini**, **WebApp1** ' a sağ tıklayıp > **başvurusu** **Ekle** ' yi seçin.
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

Web uygulamasını derleyin ve çalıştırın:

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a>Test WebApp1

Razor UI sınıfı kitaplığının kullanımda olduğunu doğrulamak için `/MyFeature/Page1` ' a gidin.

## <a name="override-views-partial-views-and-pages"></a>Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl

Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir. Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.

Örnek indirme içinde *WebApp1/Areas/MyFeature2* öğesini *WebApp1/Areas/myfeature* olarak yeniden adlandırın ve test önceliğini belirtin.

*RazorUIClassLib/Areas/myfeature/Pages/Shared/_Message. cshtml* kısmi görünümünü *WebApp1/Areas/Myfeature/Pages/Shared/_Message. cshtml*'ye kopyalayın. Biçimlendirmeyi yeni konumu belirtecek şekilde güncelleştirin. Uygulamanın kısmi sürümünün kullanılmakta olduğunu doğrulamak için uygulamayı derleyin ve çalıştırın.

### <a name="rcl-pages-layout"></a>RCL sayfaları düzeni

RCL içeriğine, Web uygulamasının *Sayfalar* klasörünün bir parçası olmasına rağmen başvurmak için, aşağıdaki dosya yapısıyla RCL projesini oluşturun:

* *RazorUIClassLib/sayfalar*
* *RazorUIClassLib/sayfalar/paylaşılan*

*RazorUIClassLib/Pages/Shared* iki kısmi dosya Içerir: *_header. cshtml* ve *_footer. cshtml*. @No__t-0 etiketleri *_Layout. cshtml* dosyasına eklenebilir:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
