---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Oluşturma ve değiştirme Blazor proje Blazor ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 667c57d536450fa2f8ae1cabc7c5a76a16d38a55
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665593"
---
# <a name="get-started-with-blazor"></a>Blazor ile çalışmaya başlama

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Önkoşullar:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Visual Studio'da ilk Blazor projenizi oluşturmak için:

1. Son yükleme [Blazor dil Hizmetleri Uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten. Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.
1. Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları .NET Core CLI ile kullanılabilir duruma getir:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.
1. Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.
1. Seçin **Blazor** şablonu seçip alt **Tamam**.
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

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

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

## <a name="blazor-project"></a>Blazor proje

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

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.

*Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Uygulamayı çalıştırın. Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-razor-components-app>
