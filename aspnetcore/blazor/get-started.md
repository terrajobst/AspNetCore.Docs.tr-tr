---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Tercih ettiğiniz araç ile bir Blazor uygulaması oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/get-started
ms.openlocfilehash: 7bff1d38de77adfa1835ee37f4e5f8912ba629cc
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800317"
---
# <a name="get-started-with-aspnet-core-blazor"></a>ASP.NET Core Blazor kullanmaya başlama

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

Blazor kullanmaya başlama:

1. En son [.NET Core 3,0 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) sürümünü yükler.

1. Komut kabuğu 'nda aşağıdaki komutu çalıştırarak Blazor şablonlarını yüklemelisiniz:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19424.4
   ```

1. Araç seçiminiz için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. **ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio önizlemesini](https://visualstudio.com/vs/preview) yüklersiniz.

   2 \. Yeni bir proje oluşturun.

   3 \. **Blazor uygulamasını**seçin. **İleri**’yi seçin.

   4 \. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin. **Oluştur**’u seçin.

   5 \. Blazor istemci tarafı deneyimi için **Blazor WebAssembly uygulama** şablonunu seçin. Blazor sunucu tarafı deneyimi için **Blazor Server uygulama** şablonunu seçin. **Oluştur**’u seçin. İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.

   6 \. Uygulamayı çalıştırmak için **F5** tuşuna basın.

   > [!NOTE]
   > ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz. Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. [Visual Studio Code](https://code.visualstudio.com/)'i yükler.

   2 \. En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.

   3 \. Bir Blazor istemci tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```console
      dotnet new blazorwasm -o WebApplication1
      ```

      Bir Blazor sunucu tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:

      ```console
      dotnet new blazorserver -o WebApplication1
      ```

      İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.

   4 \. Visual Studio Code 'de *WebApplication1* klasörünü açın.

   5 \. Blazor sunucu tarafı bir proje için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister. **Evet**' i seçin.

   6 \. Blazor sunucu tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Bir Blazor istemci tarafı uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.

   7 \. Bir tarayıcıda öğesine `https://localhost:5001`gidin.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **Blazor Server App** template. For a Blazor client-side experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Bir Blazor istemci tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```console
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Bir Blazor sunucu tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```console
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.

   Bir tarayıcıda öğesine `https://localhost:5001`gidin.

   ---

Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:

* Ana Sayfası
* Sayaç
* Verileri getir

Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin. Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak Razor bileşenleri kullanarak C#daha iyi bir yaklaşım sağlar.

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

En üstteki `/counter` `@page` yönergeyle belirtilen şekilde tarayıcıda için bir istek, `Counter` bileşenin içeriğini işlemesine neden olur. Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.

**Bana tıklama** düğmesi her seçildiğinde:

* `onclick` Olay tetiklenir.
* `IncrementCount` Yöntemi çağrılır.
* `currentCount` Artırılır.
* Bileşen yeniden işlenir.

Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.

HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin. Örneğin, bileşene bir `Counter` `<Counter />` öğe `Index` ekleyerek bileşeni uygulamanın giriş sayfasına ekleyin.

*Pages/Index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfası, `Counter` bileşen tarafından sağlanmış kendi sayacıdır.

Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir. `Counter` Bileşene bir parametre eklemek için `@code` bileşenin bloğunu güncelleştirin:

* Özniteliği ile için `IncrementAmount` bir public özelliği ekleyin. `[Parameter]`
* Değerini değerini `IncrementAmount`artırdığınızda kullanmak için `IncrementCount` yöntemini değiştirin `currentCount`.

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Özniteliği kullanarak bileşenin`<Counter>`öğesindeöğesinibelirtin. `Index` `IncrementAmount`

*Pages/Index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uygulamayı çalıştırın. Bileşenin, bana tıklama düğmesi seçildiğinde her seferinde on ile artan kendi sayacı vardır. `Index` ' `Counter` De`/counter` bileşen (*Counter. Razor*), bir tane tarafından arttırmaya devam eder.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
