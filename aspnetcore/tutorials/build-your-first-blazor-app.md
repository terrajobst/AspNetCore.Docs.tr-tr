---
title: İlk Blazor uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: d235fec4e128ad8622a06d301eeac15c4862c159
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087763"
---
# <a name="build-your-first-blazor-app"></a>İlk Blazor uygulamanızı oluşturun

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

Bu öğreticide oluşturun ve Blazor uygulamayı değiştirmek gösterilmektedir.

Sunulan yönergeleri <xref:blazor/get-started> makalenin Bu öğreticide bir Blazor projesi oluşturun.

## <a name="build-components"></a>Yapı bileşenleri

1. Her uygulamanın üç sayfalarında Gözat *sayfaları* klasörü: Giriş, sayaç ve veri getirilemiyor. Bu sayfalar Razor bileşen dosyaları tarafından uygulanan *Index.razor*, *Counter.razor*, ve *FetchData.razor*.

1. Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi. Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.

1. Uygulama sayacı bileşenin inceleyin *Counter.razor* dosya.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Kullanıcı arabirimini sayacı bileşenin HTML kullanılarak tanımlanır. (Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor). HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür. Oluşturulan .NET sınıf adı dosya adıyla eşleşen.

   Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok. İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilir. Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için kullanılır.

   Zaman **me tıklayın** düğmesi seçili:

   * Sayaç bileşen kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi).
   * Sayaç bileşen kendi işleme ağacında yeniden oluşturur.
   * Yeni bir işleme ağacı Öncekine karşılaştırılır.
   * Yalnızca belge nesne modeli (DOM) değişiklikler uygulanır. Görüntülenen sayısı güncelleştirilir.

1. Değiştirme C# bir yerine ikiye sayısı artması yapmak için sayaç bileşeninin mantığı.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Yeniden oluşturun ve değişiklikleri görmek için uygulamayı çalıştırın. Seçin **me tıklayın** düğmesi. İki sayacını artırır.

## <a name="use-components"></a>Bileşenleri kullanma

Bir bileşen HTML söz dizimi kullanılarak başka bir bileşeni içerir.

1. Sayaç bileşeni için uygulamanın dizin bileşeni ekleyerek bir `<Counter />` dizin bileşeni öğesine (*Index.razor*).

   Bu deneyim için bir anket istemi bileşeni Blazor istemci-tarafı kullanıyorsanız (`<SurveyPrompt>` öğesi) dizin bileşenidir. Değiştirin `<SurveyPrompt>` öğeyle `<Counter>` öğesi. Bu deneyim için Blazor sunucu-tarafı uygulaması kullanıyorsanız, ekleyin `<Counter>` dizin bileşeni öğesi:

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Dizin bileşeni kendi sayaç vardır.

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenleri parametrelerini de sağlayabilirsiniz. Bileşen parametreleri, genel olmayan özellikler ile donatılmış bileşen sınıfı kullanarak tanımlanan `[Parameter]`. Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.

1. Bileşenin güncelleştirme `@functions` C# kod:

   * Ekleme bir `IncrementAmount` özelliği düzenlenmiş ile `[Parameter]` özniteliği.
   * Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Belirtin bir `IncrementAmount` dizin bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik. Sayaç tarafından on Artır bir değere ayarlayın.

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Dizin bileşeni yeniden yükleyin. Sayaç artırılır on tarafından her zaman **me tıklayın** düğmesi seçili. Sayacın sayaç bileşeninde bir ilerlemeye devam eder.

## <a name="route-to-components"></a>Bileşenleri için yol

`@page` En üstündeki yönerge *Counter.razor* dosya sayacı bileşen yönlendirme bir uç nokta olduğunu belirtir. Sayaç bileşeni için gönderilen istekleri işler `/counter`. Olmadan `@page` yönergesi, bir bileşen yönlendirilen istekler da işlemiyor ancak bileşeni hala başka bileşenler tarafından kullanılabilir.

## <a name="dependency-injection"></a>Bağımlılık ekleme

Bileşenleri uygulamanın service kapsayıcısında kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). Halinde bileşenini kullanarak Hizmetleri ekleme `@inject` yönergesi.

Parçalar bileşenin yönergeleri inceleyin.

Blazor sunucu tarafı uygulama ile çalışıyorsanız `WeatherForecastService` hizmet olarak kayıtlı bir [tekil](xref:fundamentals/dependency-injection#service-lifetimes), Hizmeti'nin bir örneğini uygulama boyunca kullanılabilir. `@inject` Yönergesi örneğini ekleme için kullanılan `WeatherForecastService` halinde bileşenini hizmet.

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Parçalar bileşeni olarak eklenen hizmet kullanan `ForecastService`, bir dizi alınacak `WeatherForecast` nesneler:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Bir Blazor istemci-tarafı uygulaması ile çalışıyorsanız `HttpClient` hava durumu tahminini verilerden elde etmek için eklenmiş *weather.json* dosyası *wwwroot/örnek-veri* klasörü:

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

A [ \@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) döngü, her bir tahmin örneği tablosunda bir satıra hava durumu verilerinin işlemek için kullanılır:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a>Bir Yapılacaklar listesi oluşturun

Yeni bir bileşen, bir basit bir Yapılacaklar listesi uygulayan uygulamaya ekleyin.

1. Adlı boş bir dosya ekleme *Todo.razor* uygulamada *sayfaları* klasörü:

1. Bileşen için ilk biçimlendirme sağlar:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Todo bileşeni Gezinti çubuğuna ekleyin.

   NavMenu bileşeni (*Shared/NavMenu.razor*) uygulamanın düzende kullanılır. Düzenleri, uygulama içeriği yinelenmesini önlemek izin bileşenlerdir. Daha fazla bilgi için bkz. <xref:blazor/layouts>.

   Ekleme bir `<NavLink>` aşağıda bulunan mevcut liste öğelerini aşağıdaki liste öğesi işaretleme ekleyerek Todo bileşeni için *Shared/NavMenu.razor* dosyası:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Yeniden oluşturun ve uygulamayı çalıştırın. Todo bileşen bağlantısını çalıştığını onaylamak için yeni Todo sayfasını ziyaret edin.

1. Ekleme bir *TodoItem.cs* bir todo öğesini temsil eden bir sınıf tutmak için projenin kök dosya. Aşağıdaki C# için kod `TodoItem` sınıfı:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Todo bileşenine döndürür (*Pages/Todo.razor*):

   * Todo öğeleri için bir alan eklemek bir `@functions` blok. Todo bileşeni, yapılacaklar listesi durumunu korumak için bu alanı kullanır.
   * Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Uygulama yapılacaklar listesine eklemek için kullanıcı Arabirimi öğeleri gerektirir. Bir metin girişi ve listenin altındaki bir düğme ekleyin:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Zaman **todo ekleyin** düğmesi seçilirse, hiçbir şey olmaz kadar düğmesi olay işleyicisi kablolu değildir çünkü.

1. Ekleme bir `AddTodo` Todo bileşen ve düğme için tıkladığında kullanarak kayıt yöntemi `onclick` özniteliği:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   `AddTodo` C# Yöntemi düğme seçildiğinde çağrılır.

1. Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip. Metin girişi değerini ayarlayarak Temizle `newTodo` boş bir dize:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Yeni kod test etmek için yapılacaklar listesi birkaç Yapılacaklar öğesi ekleyin.

1. Her bir todo öğesi için başlık metnini düzenlenebilir hale getirilebilir ve onay kutusu tamamlanan öğeleri takip kullanıcı yardımcı olabilir. Her bir todo öğesi için bir onay kutusu giriş ekleyin ve değerini bağlama `IsDone` özelliği. Değişiklik `@todo.Title` için bir `<input>` bağlı öğe `@todo.Title`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `<h1>` tamamlanmamış todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Tamamlanmış Todo bileşeni (*Pages/Todo.razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Yeniden oluşturun ve uygulamayı çalıştırın. Yeni kod test etmek için todo öğeleri ekleyin.

## <a name="publish-and-deploy-the-app"></a>Yayımlama ve uygulama dağıtma

Uygulamayı yayımlamak için bkz: <xref:host-and-deploy/blazor/index>.
