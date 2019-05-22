---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile kendi tercih ettiğiniz araçları Blazor uygulamayla oluşturarak başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/06/2019
uid: blazor/get-started
ms.openlocfilehash: 09613f5d8a4d130f7dca53f31bdd33de527fc776
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969864"
---
# <a name="get-started-with-blazor"></a>Blazor ile çalışmaya başlama

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

Blazor ile kullanmaya başlayın:

1. Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.

1. Blazor şablonları, bir komut kabuğu'nda aşağıdaki komutu çalıştırarak yükleyin:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview5-19227-01
   ```

1. Tercih ettiğiniz araçları için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1.&nbsp;en son yükleme [Visual Studio Önizleme](https://visualstudio.com/preview) ile **ASP.NET ve web geliştirme** iş yükü.

   2.&nbsp;en son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten. Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.

   3.&nbsp;Önizleme SDK'ları kullanmak Visual Studio, Visual Studio (Önizleme sürümü değil) en son kararlı sürümünü kullanıyorsanız, etkinleştir: Açık **Araçları** > **seçenekleri** menü çubuğundaki. Açık **projeler ve çözümler** düğümü. Açık **.NET Core** sekmesi. İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**. **Tamam**’ı seçin.

   4.&nbsp;yeni bir proje oluşturun.

   5.&nbsp;seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.

   6.&nbsp;bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin. Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın. **Oluştur**’u seçin.

   7.&nbsp;içinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.

   8.&nbsp;Blazor istemci-tarafı deneyimi için seçin **Blazor (istemci-tarafı)** şablonu. Blazor sunucu tarafı deneyimi için seçin **Blazor (sunucu tarafı)** şablonu. **Oluştur**’u seçin. İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.

   9.&nbsp;tuşuna **F5** uygulamayı çalıştırmak için.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)
   
   1.&nbsp;yükleme [Visual Studio Code'u](https://code.visualstudio.com/).

   2.&nbsp;en son yükleme [ C# Visual Studio Code uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3.&nbsp;Blazor istemci-tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutu yürütün:

      ```console
      dotnet new blazor -o WebApplication1
      ```

      Blazor sunucu tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutu yürütün:

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.

   4.&nbsp;açık *WebApplication1* Visual Studio code'da klasörü.

   5.&nbsp;bir Blazor için sunucu tarafı proje, IDE istekleri oluşturmak ve projede hata ayıklamak için varlıklar ekleyin. Seçin **Evet**.

   6.&nbsp;Blazor sunucu-tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor istemci-tarafı uygulaması kullanıyorsanız yürütme `dotnet run` uygulama proje klasöründen.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Blazor istemci-tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutları yürütün:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Blazor sunucu tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutları yürütün:

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.

   ---

Bir tarayıcıda gidin `https://localhost:5001`.

Birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:

* Ana Sayfası
* Sayaç
* Veri getirme

Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi. Normal olarak artan bir Web sayfasındaki bir sayaç gerekli JavaScript Yazma, ancak Razor bileşenleri sağlamak daha iyi bir yaklaşım kullanarak C#.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur. Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.

Her zaman **me tıklayın** düğmesi seçili:

* `onclick` Olay tetiklenir.
* `IncrementCount` Yöntemi çağrılır.
* `currentCount` Artırılır.
* Bileşeni yeniden oluşturulur.

Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.

Bir bileşen başka bir bileşene HTML söz dizimi kullanılarak ekleyin. Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir. Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfası sayacı bileşen tarafından sağlanan kendi sayaç vardır.

Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:

* Bir özelliği için ekleme `IncrementAmount` ile bir `[Parameter]` özniteliği.
* Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Belirtin `IncrementAmount` dizin bileşenin içinde `<Counter>` öğesini kullanarak bir öznitelik.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uygulamayı çalıştırın. Dizin bileşenini on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili. Sayaç bileşeni (*Counter.razor*) adresindeki `/counter` bir ilerlemeye devam eder.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
