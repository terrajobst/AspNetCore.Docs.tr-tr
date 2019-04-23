---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2019
uid: blazor/get-started
ms.openlocfilehash: 45ae0acc6aaee433cce4eddb2fe9c59c306581d7
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982671"
---
# <a name="get-started-with-blazor"></a>Blazor ile çalışmaya başlama

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

Birkaç adımda Blazor ile kullanmaya başlayın:

1. Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.

1. Blazor şablonları, bir komut kabuğu'nda aşağıdaki komutu çalıştırarak yükleyin:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview4-19216-03
   ```

1. Tercih ettiğiniz araçları için yönergeleri izleyin:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1.&nbsp;en son önizlemesi yükleme [Visual Studio 2019](https://visualstudio.com/preview) ile **ASP.NET ve web geliştirme** iş yükü.

   2.&nbsp;en son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten. Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.

   3.&nbsp;Önizleme SDK'ları kullanmak için Visual Studio etkinleştir: Açık **Araçları** > **seçenekleri** menü çubuğundaki. Açık **projeler ve çözümler** düğümü. Açık **.NET Core** sekmesi. İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**. **Tamam**’ı seçin.

   4.&nbsp;yeni bir proje oluşturun.

   5.&nbsp;seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.

   6.&nbsp;bir adı belirtin **proje adı** alan. Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın. **Oluştur**’u seçin.

   7.&nbsp;emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.

   8.&nbsp;Blazor istemci-tarafı bir deneyim için seçin **Blazor (istemci-tarafı)** şablonu. Sunucu tarafı Blazor bir deneyim için seçin **Blazor (sunucu tarafı)** şablonu. **Oluştur**’u seçin.

   9.&nbsp;tuşuna **F5** uygulamayı çalıştırmak için.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)
   
   1.&nbsp;yükleme [Visual Studio Code'u](https://code.visualstudio.com/).

   2.&nbsp;en son yükleme [ C# Visual Studio Code uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3.&nbsp;Blazor istemci-tarafı bir deneyim için bir komut kabuğu'ndan aşağıdaki komutu yürütün:

      ```console
      dotnet new blazor -o WebApplication1
      ```

      Sunucu tarafı Blazor bir deneyim için bir komut kabuğu'ndan aşağıdaki komutu yürütün:

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      > [!NOTE]
      > Yalnızca Blazor istemci-tarafı macOS ASP.NET Core 3.0 Preview 4'te desteklenir. Daha fazla bilgi için [Blazor sunucu tarafı: çalıştırma dotnet macos'ta InvalidOperationException ile başarısız oluyor](https://github.com/aspnet/AspNetCore/issues/9402).

   4.&nbsp;açık *WebApplication1* Visual Studio code'da klasörü.

   5.&nbsp;Visual Studio yapı ve proje hatalarını ayıklamaya, seçmek için gerekli varlıkları eklemek için kod tarafından Blazor sunucu tarafı projesi için istendiğinde **Evet**.

   6.&nbsp;Blazor sunucu-tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın. Blazor istemci-tarafı uygulaması kullanıyorsanız yürütme `dotnet run` uygulama proje klasöründen.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For an experience with Blazor server-side, select the **ASP.NET Core Blazor (server-side)** template. For an experience with Blazor server-side, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   İstemci tarafı Blazor bir deneyim için bir komut kabuğu'ndan aşağıdaki komutları yürütün:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Sunucu tarafı Blazor bir deneyim için bir komut kabuğu'ndan aşağıdaki komutları yürütün:

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   > [!NOTE]
   > MacOS üzerinde bir Blazor istemci-tarafı uygulaması kullanın. Sunucu tarafı Blazor macOS üzerinde ASP.NET Core 3.0 Preview 4 için desteklenmiyor. Daha fazla bilgi için [Blazor sunucu tarafı: çalıştırma dotnet macos'ta InvalidOperationException ile başarısız oluyor](https://github.com/aspnet/AspNetCore/issues/9402).

   ---

Bir tarayıcıda gidin `https://localhost:5001`.

Birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:

* Ana Sayfası
* Sayaç
* Veri getirme

Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi. Normal olarak artan bir Web sayfasındaki bir sayaç gerekli JavaScript Yazma, ancak Razor bileşenleri sağlamak daha iyi bir yaklaşım kullanarak C#.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur. Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.

Her zaman **me tıklayın** düğmesi seçili:

* `onclick` Olay tetiklenir.
* `IncrementCount` Yöntemi çağrılır.
* `currentCount` Artırılır.
* Bileşeni yeniden oluşturulur.

Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.

Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin. Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir. Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfası, kendi sayaç vardır.

Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:

* Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.
* Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4-5,9)]

Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

Uygulamayı çalıştırın. Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
