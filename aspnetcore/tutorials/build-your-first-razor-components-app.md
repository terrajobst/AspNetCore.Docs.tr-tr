---
title: Razor bileşenleri ilk uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Razor bileşenleri uygulaması derleme ve Razor bileşenleri temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159349"
---
# <a name="build-your-first-razor-components-app"></a>Razor bileşenleri ilk uygulamanızı oluşturun

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Bu öğretici, Razor bileşenleri ile uygulama oluşturma işlemini göstermektedir ve Razor bileşenleri temel kavramlarını gösterir. Bu öğretici (.NET Core 3.0 veya sonraki sürümlerde desteklenir) ya da bir Razor bileşenleri tabanlı proje veya (.NET Core gelecekteki bir sürümde desteklenir) Blazor tabanlı bir proje kullanarak keyfini çıkarabilirsiniz.

ASP.NET Core Razor bileşenlerini kullanarak bir deneyim için (*önerilen*):

* Sunulan yönergeleri <xref:razor-components/get-started> Razor bileşenleri tabanlı bir proje oluşturmaktır.
* Projeyi adlandırın `RazorComponents`.
* Bir çoklu proje çözümü Razor bileşenleri şablonu oluşturulur. Razor bileşenleri proje olarak oluşturulan *RazorComponents.App*.

Blazor kullanarak bir deneyim için:

* Sunulan yönergeleri <xref:spa/blazor/get-started> Blazor tabanlı bir proje oluşturmaktır.
* Projeyi adlandırın `Blazor`.
* Bir tek proje çözüm Blazor şablonu oluşturulur.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)). Önkoşullar için aşağıdaki konulara bakın:

## <a name="build-components"></a>Yapı bileşenleri

1. Her bir uygulamanın üç göz atın: Giriş, sayaç ve veri getirilemiyor. Bu sayfalar, Razor dosyalarında tarafından uygulanan *sayfaları* klasörü: *Index.cshtml*, *Counter.cshtml*, ve *FetchData.cshtml*.

1. Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi. Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.

1. Uygulama sayacı bileşenin inceleyin *Counter.cshtml* dosya.

   *Pages/Counter.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   Kullanıcı arabirimini sayacı bileşenin HTML kullanılarak tanımlanır. (Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor). HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür. Oluşturulan .NET sınıf adı dosya adıyla eşleşen.

   Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok. İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilir. Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için kullanılır.

   Zaman **me tıklayın** düğmesi seçili:

   * Sayaç bileşen kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi).
   * Sayaç bileşen kendi işleme ağacında yeniden oluşturur.
   * Yeni bir işleme ağacı Öncekine karşılaştırılır.
   * Yalnızca belge nesne modeli (DOM) değişiklikler uygulanır. Görüntülenen sayısı güncelleştirilir.

1. Değiştirme C# bir yerine ikiye sayısı artması yapmak için sayaç bileşeninin mantığı.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. Yeniden oluşturun ve değişiklikleri görmek için uygulamayı çalıştırın. Seçin **me tıklayın** düğmeyi ve iki sayacını artırır.

## <a name="use-components"></a>Bileşenleri kullanma

Bir HTML benzeri sözdizimi kullanarak başka bir bileşene dönüştürerek bileşen içerir.

1. Sayaç bileşen uygulamanın dizini (giriş sayfası) bileşenine ekleyerek bir `<Counter />` dizin bileşeni öğesi.

   Bu deneyim için bir anket istemi bileşeni Blazor kullanıyorsanız (`<SurveyPrompt>` öğesi) dizin bileşenidir. Değiştirin `<SurveyPrompt>` öğeyle `<Counter>` öğesi.

   *Pages/Index.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Giriş sayfası, kendi sayaç vardır.

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenleri parametrelerini de sağlayabilirsiniz. Bileşen parametreleri, genel olmayan özellikler ile donatılmış bileşen sınıfı kullanarak tanımlanan `[Parameter]`. Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.

1. Bileşenin güncelleştirme `@functions` C# kod:

   * Ekleme bir `IncrementAmount` özelliği düzenlenmiş ile `[Parameter]` özniteliği.
   * Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.

   *Pages/Counter.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik. Sayaç tarafından on Artır bir değere ayarlayın.

   *Pages/Index.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. Sayfayı yeniden yükleyin. Giriş sayfası sayaç artırılır on tarafından her zaman **me tıklayın** düğmesi seçili. Sayaca sağ *sayacı* sayfasında artışlarla bir.

## <a name="route-to-components"></a>Bileşenleri için yol

`@page` En üstündeki yönerge *Counter.cshtml* dosya bu bileşen yönlendirme bir uç nokta olduğunu belirtir. Sayaç bileşeni için gönderilen istekleri işler `/Counter`. Olmadan `@page` yönergesi, bileşen yönlendirilen istekler da işlemiyor ancak bileşeni hala başka bileşenler tarafından kullanılabilir.

## <a name="dependency-injection"></a>Bağımlılık ekleme

Bileşenleri uygulamanın service kapsayıcısında kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). Halinde bileşenini kullanarak Hizmetleri ekleme `@inject` yönergesi.

Parçalar bileşenin yönergeleri inceleyin (*Pages/FetchData.cshtml*). `@inject` Yönergesi örneğini ekleme için kullanılan `WeatherForecastService` hizmet bileşeni içinde:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

`WeatherForecastService` Hizmet olarak kayıtlı bir [singleton](xref:fundamentals/dependency-injection#service-lifetimes), Hizmeti'nin bir örneğini uygulama boyunca kullanılabilir.

Parçalar bileşeni olarak eklenen hizmet kullanan `ForecastService`, bir dizi alınacak `WeatherForecast` nesneler:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

A [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) döngü, her bir tahmin örneği tablosunda bir satıra hava durumu verilerinin işlemek için kullanılır:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a>Bir Yapılacaklar listesi oluşturun

Bir basit bir Yapılacaklar listesi uygulayan uygulamaya yeni bir sayfa ekleyin.

1. Boş bir dosyaya eklemek *sayfaları* adlı klasöre *Todo.cshtml*.

1. Sayfa için ilk biçimlendirme sağlar:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Todo sayfa gezinti çubuğuna ekleyin.

   NavMenu bileşeni (*Shared/NavMenu.csthml*) uygulamanın düzende kullanılır. Düzenleri, uygulama içeriği yinelenmesini önlemek izin bileşenlerdir. Daha fazla bilgi için bkz. <xref:razor-components/layouts>.

   Ekleme bir `<NavLink>` için aşağıda bulunan mevcut liste öğelerini aşağıdaki liste öğesi işaretleme ekleyerek Todo sayfası *Shared/NavMenu.csthml* dosyası:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Yeniden oluşturun ve uygulamayı çalıştırın. Todo sayfanın bağlantısını çalıştığını onaylamak için yeni Todo sayfasını ziyaret edin.

1. Ekleme bir *TodoItem.cs* bir todo öğesini temsil eden bir sınıf tutmak için projenin kök dosya. Aşağıdaki C# için kod `TodoItem` sınıfı:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. Todo bileşenine döndürür (*Todo.cshtml*):

   * Açıklamada içinde için bir alan eklemek bir `@functions` blok. Todo bileşeni, yapılacaklar listesi durumunu korumak için bu alanı kullanır.
   * Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. Uygulama, açıklamada listeye eklemek için kullanıcı Arabirimi öğeleri gerektirir. Bir metin girişi ve listenin altındaki bir düğme ekleyin:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Zaman **todo ekleyin** düğmesi seçilirse, hiçbir şey olmaz kadar düğmesi olay işleyicisi kablolu değildir çünkü.

1. Ekleme bir `AddTodo` Todo bileşen ve düğme için tıkladığında kullanarak kayıt yöntemi `onclick` özniteliği:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   `AddTodo` C# Yöntemi düğme seçildiğinde çağrılır.

1. Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip. Metin girişi değerini ayarlayarak Temizle `newTodo` boş bir dize:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Bazı açıklamada yeni kodu test etmek için yapılacaklar listesine ekleyin.

1. Her bir todo öğesi için başlık metnini düzenlenebilir hale getirilebilir ve onay kutusu tamamlanan öğeleri takip kullanıcı yardımcı olabilir. Her bir todo öğesi için bir onay kutusu giriş ekleyin ve değerini bağlama `IsDone` özelliği. Değişiklik `@todo.Title` için bir `<input>` bağlı öğe `@todo.Title`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `<h1>` tamamlanmamış todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Tamamlanmış Todo bileşeni (*Todo.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Yeni kod test etmek için todo öğeleri ekleyin.

## <a name="publish-and-deploy-the-app"></a>Yayımlama ve uygulama dağıtma

Uygulamayı yayımlamak için bkz: <xref:host-and-deploy/razor-components/index#publish-the-app>.
