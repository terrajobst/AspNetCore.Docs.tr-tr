---
title: ASP.NET Core Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile kendi tercih ettiğiniz araçları Blazor uygulamayla oluşturarak başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/get-started
ms.openlocfilehash: 51fb531c07de35b08911c8475b192f3bda281ea4
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500437"
---
# <a name="get-started-with-aspnet-core-blazor"></a>ASP.NET Core Blazor ile çalışmaya başlama

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

Blazor ile kullanmaya başlayın:

1. Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.

1. Blazor şablonları, bir komut kabuğu'nda aşağıdaki komutu çalıştırarak yükleyin:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview6.19307.2
   ```

1. Tercih ettiğiniz araçları için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Son yükleme [Visual Studio Önizleme](https://visualstudio.com/vs/preview) ile **ASP.NET ve web geliştirme** iş yükü.

   2\. Son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten. Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.

   3\. Yeni bir proje oluşturun.

   4\. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.

   5\. Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin. Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın. **Oluştur**’u seçin.

   6\. İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.

   7\. Blazor istemci-tarafı deneyimi için seçin **Blazor (istemci-tarafı)** şablonu. Blazor sunucu tarafı deneyimi için seçin **Blazor (sunucu tarafı)** şablonu. **Oluştur**’u seçin. İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.

   8\. Tuşuna **F5** uygulamayı çalıştırmak için.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)
   
   1\. Yükleme [Visual Studio Code'u](https://code.visualstudio.com/).

   2\. Son yükleme [ C# Visual Studio Code uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3\. Blazor istemci-tarafı deneyimi için bir komut kabuğu'nda aşağıdaki komutu yürütün:

      ```console
      dotnet new blazor -o WebApplication1
      ```

      Blazor sunucu tarafı deneyimi için bir komut kabuğu'nda aşağıdaki komutu yürütün:

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.

   4\. Açık *WebApplication1* Visual Studio code'da klasörü.

   5\. Blazor sunucu tarafı proje için yapı ve proje hatalarını ayıklamaya varlıklar ekleme IDE ister. Seçin **Evet**.

   6\. Blazor sunucu-tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor istemci-tarafı uygulaması kullanıyorsanız yürütme `dotnet run` uygulama proje klasöründen.

   7\. Bir tarayıcıda gidin `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Blazor istemci-tarafı deneyimi için bir komut kabuğu'nda aşağıdaki komutları yürütün:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Blazor sunucu tarafı deneyimi için bir komut kabuğu'nda aşağıdaki komutları yürütün:

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.

   Bir tarayıcıda gidin `https://localhost:5001`.

   ---

Birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:

* Ana Sayfası
* Sayaç
* Veri getirme

Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi. Normal olarak artan bir Web sayfasındaki bir sayaç gerekli JavaScript Yazma, ancak Razor bileşenleri sağlamak daha iyi bir yaklaşım kullanarak C#.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst neden `Counter` içeriğini işlemek için bileşen. Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.

Her zaman **me tıklayın** düğmesi seçili:

* `onclick` Olay tetiklenir.
* `IncrementCount` Yöntemi çağrılır.
* `currentCount` Artırılır.
* Bileşeni yeniden oluşturulur.

Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.

Bir bileşen başka bir bileşene HTML sözdizimini kullanarak ekleyin. Örneğin, ekleme `Counter` bileşeni ekleyerek, uygulamanın giriş sayfası için bir `<Counter />` öğesine `Index` bileşeni.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfası tarafından sağlanan kendi sayaç sahip `Counter` bileşeni.

Bileşen parametreleri, öznitelikleri kullanarak belirtilir veya [alt içeriğin](xref:blazor/components#child-content), en alt bileşen özelliklerini ayarlama sağlar. Bir parametre eklemek için `Counter` bileşeni, bileşenin güncelleştirme `@code` engelle:

* Bir özelliği için ekleme `IncrementAmount` ile bir `[Parameter]` özniteliği.
* Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Belirtin `IncrementAmount` içinde `Index` bileşenin `<Counter>` öğesini kullanarak bir öznitelik.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uygulamayı çalıştırın. `Index` Bileşeniyse on tarafından her zaman artırır, kendi sayaç **me tıklayın** düğmesi seçili. `Counter` Bileşeni (*Counter.razor*) adresindeki `/counter` bir ilerlemeye devam eder.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
