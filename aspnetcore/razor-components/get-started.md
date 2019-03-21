---
title: Razor bileşenleri ile çalışmaya başlama
author: guardrex
description: Razor bileşenler oluşturma ve Razor bileşenleri projesini değiştirme kullanmaya nasıl başlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: razor-components/get-started
ms.openlocfilehash: 026bc5b3222a8ffa35a064bef8bbf64834b67a90
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209698"
---
# <a name="get-started-with-razor-components"></a>Razor bileşenleri ile çalışmaya başlama

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Önkoşullar:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Visual Studio'da ilk Razor bileşenleri projenizi oluşturmak için:

1. Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.
1. Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.
1. Seçin **Razor bileşenleri** şablonu seçip alt **Tamam**.
1. Tuşuna **F5** uygulamayı çalıştırmak için.

Tebrikler! Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!

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

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
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

Tebrikler! Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!

---

## <a name="razor-components-project"></a>Razor bileşenleri proje

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

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.

*WebApplication1/Components/Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

Uygulamayı çalıştırın. Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-razor-components-app>
