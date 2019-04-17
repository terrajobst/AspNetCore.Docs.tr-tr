---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/get-started
ms.openlocfilehash: cc68e1c58af607f840b952f033d3f3b0e54e6cf4
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614912"
---
# <a name="get-started-with-blazor"></a>Blazor ile çalışmaya başlama

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

Aşağıdaki deneyimleri birini uygulayarak Blazor ile kullanmaya başlayın:

* [Sunucu tarafı Razor bileşenleri](#server-side-razor-components-experience)
* [İstemci tarafı Blazor](#client-side-blazor-experience)

## <a name="server-side-razor-components-experience"></a>Sunucu tarafı Razor bileşenleri deneyimi

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Önkoşullar:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Visual Studio'da ilk Razor bileşenleri projenizi oluşturmak için:

1. Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.
1. Visual Studio Önizleme SDK'ları kullanmak etkinleştir:
   1. Açık **Araçları** > **seçenekleri** menü çubuğundaki.
   1. Açık **projeler ve çözümler** düğümü. Açık **.NET Core** sekmesi.
   1. İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**. **Tamam**’ı seçin.
1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. Bir ad sağlayın **proje adı** alan. Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın. **Oluştur**’u seçin.
1. Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.
1. Seçin **Razor bileşenleri** şablonu seçip alt **Oluştur**.
1. Tuşuna **F5** uygulamayı çalıştırmak için.

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:blazor/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

-->

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Önkoşullar:

* [.NET core SDK 3.0 Önizleme](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Bir komut kabuğu'ndan ilk Razor bileşenleri projenizi oluşturmak için:

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. Bir tarayıcıda gidin `https://localhost:5001`.

---

Razor bileşenleri, Razor sözdizimi kullanılarak yazılan ancak Razor sayfaları ve MVC görünümleri farklı derlenir. *.Razor* dosya uzantısı, Razor bileşen belirtmek için kullanılır. Razor sayfaları ve MVC görünümleri kullanmaya devam *.cshtml* dosya uzantısı.

> [!NOTE]
> Razor bileşenlerini kullanarak yazarı olduğu *.cshtml* dosyaları kullanarak Razor bileşen dosyaları tanımlanmış olduğu sürece dosya uzantısı `_RazorComponentInclude` MSBuild özelliği. Örneğin, Razor bileşen şablonu kullanılarak oluşturulan bir uygulamayı belirtir tüm *.cshtml* altında dosyaları *bileşenleri* klasör Razor bileşenleri olarak kabul:
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:

* Ana Sayfası
* Sayaç
* Veri getirme

Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi. Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.

*WebApplication1/Components/Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur. Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.

Her zaman **me tıklayın** düğmesi seçili:

* `onclick` Olay tetiklenir.
* `IncrementCount` Yöntemi çağrılır.
* `currentCount` Artırılır.
* Bileşeni yeniden oluşturulur.

Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.

Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin. Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir. Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.

*WebApplication1/Components/Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfası, kendi sayaç vardır.

Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:

* Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.
* Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.

*WebApplication1/Components/Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4-5,9)]

Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.

*WebApplication1/Components/Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

Uygulamayı çalıştırın. Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.

## <a name="client-side-blazor-experience"></a>İstemci tarafı Blazor deneyimi

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Önkoşullar:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Visual Studio'da ilk Blazor projenizi oluşturmak için:

1. Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.
1. Visual Studio Önizleme SDK'ları kullanmak etkinleştir:
   1. Açık **Araçları** > **seçenekleri** menü çubuğundaki.
   1. Açık **projeler ve çözümler** düğümü. Açık **.NET Core** sekmesi.
   1. İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**. **Tamam**’ı seçin.
1. Son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten. Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.
1. Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları .NET Core CLI ile kullanılabilir duruma getir:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. Bir ad sağlayın **proje adı** alan. Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın. **Oluştur**’u seçin.
1. Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.
1. Seçin **Blazor** şablonu seçip alt **Oluştur**.
1. Tuşuna **F5** uygulamayı çalıştırmak için.

Tebrikler! Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:blazor/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Önkoşullar:

* [.NET core SDK 3.0 Önizleme](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları ekleyin:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Bir komut kabuğu'nda ilk Blazor projenizi oluşturun:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. Bir tarayıcıda gidin `https://localhost:5001`.

Tebrikler! Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!

---

Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:

* Ana Sayfası
* Sayaç
* Veri getirme

Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi. Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur. Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.

Her zaman **me tıklayın** düğmesi seçili:

* `onclick` Olay tetiklenir.
* `IncrementCount` Yöntemi çağrılır.
* `currentCount` Artırılır.
* Bileşeni yeniden oluşturulur.

Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.

Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin. Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir. Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.

İçinde *Pages/Index.cshtml*, anket istemi bileşen bir sayaç bileşeni ile değiştirin:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Uygulamayı çalıştırın. Giriş sayfası, kendi sayaç vardır.

Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:

* Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.
* Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4-5,9)]

Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.

*Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Uygulamayı çalıştırın. Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>
