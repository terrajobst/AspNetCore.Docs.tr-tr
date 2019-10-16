---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Tercih ettiğiniz araç ile bir Blazor uygulaması oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/get-started
ms.openlocfilehash: fc368be5eb2e5d8f7c80071dc86a02ae986a685f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391048"
---
# <a name="get-started-with-aspnet-core-blazor"></a>ASP.NET Core Blazor kullanmaya başlama

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor kullanmaya başlama:

::: moniker range=">= aspnetcore-3.1"

1. [.NET Core 3,1 Preview SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.

1. Komut kabuğu 'nda aşağıdaki komutu çalıştırarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler. [Microsoft. AspNetCore. Blazor. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü vardır, ancak Blazor WebAssembly önizlemededir.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. Araç seçiminiz için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 Preview 2 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .

   2 \. Yeni bir proje oluşturun.

   3 \. **Blazor uygulamasını**seçin. **İleri ' yi**seçin.

   4 \. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin. **Oluştur**' u seçin.

   5 \. Blazor WebAssembly deneyimi için **Blazor Webassembly uygulama** şablonunu seçin. Blazor sunucu deneyimi için **Blazor Server uygulama** şablonunu seçin. **Oluştur**' u seçin. İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.

   6 \. Uygulamayı çalıştırmak için **F5** tuşuna basın.

   > [!NOTE]
   > ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz. Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. [Visual Studio Code](https://code.visualstudio.com/)'i yükler.

   2 \. En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.

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

   5 \. Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister. **Evet**' i seçin.

   6 \. Bir Blazor Server uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor WebAssembly uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.

   7 \. Bir tarayıcıda `https://localhost:5001` ' a gidin.

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

   Bir tarayıcıda `https://localhost:5001` ' a gidin.

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. En son [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) sürümünü yükler.

1. İsteğe bağlı olarak, [.NET Core 3,1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) ' yı yükleyerek ve sonra bir komut kabuğunda aşağıdaki komutu çalıştırarak [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükleme:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. Araç seçiminiz için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. **ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio 'yu](https://visualstudio.com/vs/) yükler.

   2 \. **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 Preview 2 veya üstünü](https://visualstudio.microsoft.com/vs/preview/) , Blazor webassembly uygulama geliştirmesi için de yükler.

   3 \. Yeni bir proje oluşturun.

   4 \. **Blazor uygulamasını**seçin. **İleri ' yi**seçin.

   5 \. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin. **Oluştur**' u seçin.

   6 \. Blazor WebAssembly deneyimi için **Blazor Webassembly uygulama** şablonunu seçin. Blazor sunucu deneyimi için **Blazor Server uygulama** şablonunu seçin. **Oluştur**' u seçin. İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.

   7 \. Uygulamayı çalıştırmak için **F5** tuşuna basın.

   > [!NOTE]
   > ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz. Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. [Visual Studio Code](https://code.visualstudio.com/)'i yükler.

   2 \. En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.

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

   5 \. Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister. **Evet**' i seçin.

   6 \. Bir Blazor Server uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor WebAssembly uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.

   7 \. Bir tarayıcıda `https://localhost:5001` ' a gidin.

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

   Bir tarayıcıda `https://localhost:5001` ' a gidin.

   ---

::: moniker-end

Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:

* Ana Sayfası
* Sayaç
* Verileri getir

Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin. Bir Web sayfasındaki sayacı normal şekilde artırma, JavaScript yazma gerektirir, ancak Blazor ile kullanabilirsiniz C#.

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

En üstteki `@page` yönergesinde belirtilen şekilde tarayıcıda `/counter` için bir istek, `Counter` bileşeninin içeriğini işlemesine neden olur. Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.

**Bana tıklama** düğmesi her seçildiğinde:

* @No__t-0 olayı tetiklenir.
* @No__t-0 yöntemi çağrılır.
* @No__t-0 artırılır.
* Bileşen yeniden işlenir.

Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.

HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin. Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek uygulamanın giriş sayfasına `Counter` bileşenini ekleyin.

*Pages/Index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfasının `Counter` bileşeni tarafından sağlanmış kendi sayacı vardır.

Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir. @No__t-0 bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:

* @No__t-1 özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.
* @No__t-0 yöntemini `currentCount` değerini artırdığınızda `IncrementAmount` ' i kullanacak şekilde değiştirin.

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Bir özniteliği kullanarak `Index` bileşeninin `<Counter>` öğesinde `IncrementAmount` belirtin.

*Pages/Index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uygulamayı çalıştırın. @No__t-0 bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır. @No__t-2 ' deki `Counter` bileşeni (*Counter. Razor*) bir artış ile devam eder.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
