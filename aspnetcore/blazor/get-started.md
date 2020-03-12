---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Seçtiğiniz araç ile Blazor bir uygulama oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083245"
---
# <a name="get-started-with-aspnet-core-blazor"></a>ASP.NET Core Blazor kullanmaya başlama

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor kullanmaya başlama:

1. [.NET Core 3,1 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.

1. [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu isteğe bağlı olarak yükler:
   * [.NET Core 3.1.102 veya üzeri (Önizleme) SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.
   * Komut kabuğu 'nda aşağıdaki komutu çalıştırın. [Microsoft. AspNetCore. components. WebAssembly. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) paketinin önizleme sürümü vardır, ancak Blazor WebAssembly önizlemededir.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > 3,2 Preview 2 Blazor WebAssembly şablonunu kullanmak için .NET Core SDK Version 3.1.102 veya üzeri **gereklidir** . Bir komut kabuğunda `dotnet --version` çalıştırarak yüklü .NET Core SDK sürümünü onaylayın.

1. Araç seçiminiz için yönergeleri izleyin:

   # <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019 sürüm 16,4 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .

   2 \. Yeni bir proje oluşturun.

   3 \. **Blazor uygulamasını**seçin. **İleri**’yi seçin.

   4 \. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin. **Oluştur**’u seçin.

   5 \. Blazor WebAssembly deneyimi için **Blazor Webassembly uygulama** şablonunu seçin. Blazor sunucu deneyimi için **Blazor Server uygulama** şablonunu seçin. **Oluştur**’u seçin. İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>. Blazor WebAssembly şablonu yoksa, önceki adıma dönün ve şablonu yeniden yükleyin.

   6 \. Uygulamayı çalıştırmak için **Ctrl**+**F5** tuşuna basın.

   > [!NOTE]
   > ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz. Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.

   # <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. [Visual Studio Code](https://code.visualstudio.com/)'i yükler.

   2 \. En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)yükler.

   3 \. Bir Blazor WebAssembly deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Bir Blazor sunucu deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.

   4 \. Visual Studio Code 'de *WebApplication1* klasörünü açın.

   5 \. Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister. **Evet**’i seçin.

   6 \. Bir Blazor Server uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor WebAssembly uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.

   7 \. Bir tarayıcıda `https://localhost:5001`' a gidin.

   # <a name="visual-studio-for-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

   1 \. [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.

   2 \. **Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.

   3 \. Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.

   4 \. **Blazor Server uygulama** şablonunu seçin. Şu anda yalnızca Blazor Server şablonu Mac için Visual Studio kullanılabilir. Blazor WebAssembly deneyimi için **.NET Core CLI** sekmesindeki yönergeleri izleyin. Blazor sunucu şablonunu seçtikten sonra **İleri**' yi seçin. İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. **Hedef Framework 'ü** **.NET Core 3,1** olarak ayarlayın ve **İleri ' yi**seçin.

   6 \. **Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın. **Oluştur**’u seçin.

   7 \. Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin. Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.

   Geliştirme sertifikasına güvenmek için bir istem görünürse, sertifikaya güvenin ve devam edin.

   # <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Bir Blazor Weelsembly deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Bir Blazor sunucu deneyimi için aşağıdaki komutları bir komut kabuğunda yürütün:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.

   Bir tarayıcıda `https://localhost:5001`' a gidin.

   ---

Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:

* Ana Sayfası
* Sayaç
* Verileri getir

Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin. Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.

*Pages/Counter. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur. Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.

**Bana tıklama** düğmesi her seçildiğinde:

* `onclick` olayı tetiklenir.
* `IncrementCount` yöntemi çağrılır.
* `currentCount` artırılır.
* Bileşen yeniden işlenir.

Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.

HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin. Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.

*Pages/Index. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.

Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir. `Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:

* `[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.
* `currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.

*Pages/Counter. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.

*Pages/Index. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uygulamayı çalıştırın. `Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır. `/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:blazor/templates>
* <xref:signalr/introduction>
