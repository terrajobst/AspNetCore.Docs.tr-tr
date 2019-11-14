---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Seçtiğiniz araç ile Blazor bir uygulama oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 9b4aee0be30568f098c756e9ab4cb5298e9a049b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963003"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a>ASP.NET Core Blazor kullanmaya başlama

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazorkullanmaya başlayın:

::: moniker range=">= aspnetcore-3.1"

1. [.NET Core 3,1 Preview SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.

1. Komut kabuğu 'nda aşağıdaki komutu çalıştırarak [WebAssembly şablonunuBlazor](xref:blazor/hosting-models#blazor-webassembly) . [Microsoft. AspNetCore.Blazor. Şablon](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü varsa Blazor WebAssembly önizleme aşamasındadır.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. Araç seçiminiz için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 Preview 2 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .

   2 \. Yeni bir proje oluşturun.

   3 \. **Blazor uygulama**' yı seçin. **İleri ' yi**seçin.

   4 \. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin. **Oluştur**' u seçin.

   5 \. Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin. Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin. **Oluştur**' u seçin. Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.

   6 \. Uygulamayı çalıştırmak için **Ctrl** +**F5** tuşuna basın.

   > [!NOTE]
   > Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz. Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. [Visual Studio Code](https://code.visualstudio.com/)'i yükler.

   2 \. En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.

   3 \. Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.

   4 \. Visual Studio Code 'de *WebApplication1* klasörünü açın.

   5 \. Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister. **Evet**' i seçin.

   6 \. Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.

   7 \. Bir tarayıcıda `https://localhost:5001`' a gidin.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.

   Bir tarayıcıda `https://localhost:5001`' a gidin.

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. En son [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) sürümünü yükler.

1. İsteğe bağlı olarak, [.NET Core 3,1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) ' yı yükleyerek ve sonra aşağıdaki komutu bir komut kabuğunda çalıştırarak [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükleme:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. Araç seçiminiz için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. **ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio 'yu](https://visualstudio.com/vs/) yükler.

   2 \. İsteğe bağlı olarak [Visual Studio 16,4 Preview 2 veya üstünü](https://visualstudio.microsoft.com/vs/preview/) , **ASP.net ve Web geliştirme** Iş yüküyle Blazor webassembly uygulama geliştirmesi ile birlikte yükler.

   3 \. Yeni bir proje oluşturun.

   4 \. **Blazor uygulama**' yı seçin. **İleri ' yi**seçin.

   5 \. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin. **Oluştur**' u seçin.

   6 \. Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin. Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin. **Oluştur**' u seçin. Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.

   7 \. Uygulamayı çalıştırmak için **F5** tuşuna basın.

   > [!NOTE]
   > Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz. Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. [Visual Studio Code](https://code.visualstudio.com/)'i yükler.

   2 \. En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.

   3 \. Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.

   4 \. Visual Studio Code 'de *WebApplication1* klasörünü açın.

   5 \. Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister. **Evet**' i seçin.

   6 \. Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.

   7 \. Bir tarayıcıda `https://localhost:5001`' a gidin.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.

   Bir tarayıcıda `https://localhost:5001`' a gidin.

   ---

::: moniker-end

Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:

* Ana Sayfası
* Sayaç
* Verileri getir

Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin. Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur. Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.

**Bana tıklama** düğmesi her seçildiğinde:

* `onclick` olayı tetiklenir.
* `IncrementCount` yöntemi çağrılır.
* `currentCount` artırılır.
* Bileşen yeniden işlenir.

Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.

HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin. Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.

*Pages/Index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.

Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir. `Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:

* `[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.
* `currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.

*Pages/Index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uygulamayı çalıştırın. `Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır. `/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
