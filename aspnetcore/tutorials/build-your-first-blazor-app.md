---
title: İlk Blazor uygulamanızı oluşturma
author: guardrex
description: Adım adım Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/26/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 88999350b9c7631e15ba9d0242da5880c8ae71ff
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994221"
---
# <a name="build-your-first-blazor-app"></a>İlk Blazor uygulamanızı oluşturma

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

Bu öğreticide bir Blazor uygulamasının nasıl oluşturulacağı ve değiştirileceği gösterilmektedir.

Bu öğreticide bir Blazor <xref:blazor/get-started> projesi oluşturmak için makalesindeki yönergeleri izleyin. Projeyi *ToDoList*olarak adlandırın.

## <a name="build-components"></a>Derleme bileşenleri

1. *Sayfalar* klasöründe uygulamanın üç sayfasının her birine gidin: Giriş, sayaç ve veri getirme. Bu sayfalar, Razor bileşen dosyaları *dizini. Razor*, *Counter. Razor*ve *fetchdata. Razor*tarafından uygulanır.

1. Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin. Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak Blazor kullanarak C#daha iyi bir yaklaşım sağlar.

1. `Counter` *Counter. Razor* dosyasındaki bileşenin uygulamasını inceleyin.

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   `Counter` Bileşenin kullanıcı arabirimi HTML kullanılarak tanımlanır. Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir. HTML biçimlendirme ve C# işleme mantığı, derleme zamanında bir bileşen sınıfına dönüştürülür. Oluşturulan .NET sınıfının adı dosya adıyla eşleşir.

   Bileşen sınıfının üyeleri bir `@code` blokta tanımlanır. `@code` Bloğunda, bileşen durumu (özellikler, alanlar) ve yöntemler olay işleme için veya diğer bileşen mantığını tanımlamak için belirtilir. Bu Üyeler daha sonra bileşenin işleme mantığının bir parçası olarak ve olayları işlemek için kullanılır.

   **Bana tıklama** düğmesi seçildiğinde:

   * `Counter` Bileşenin kayıtlı `onclick` işleyicisine(`IncrementCount` yöntemi) denir.
   * `Counter` Bileşen, işleme ağacını yeniden oluşturur.
   * Yeni işleme ağacı öncekiyle karşılaştırılır.
   * Yalnızca Belge Nesne Modeli (DOM) üzerinde yapılan değişiklikler uygulanır. Görünen sayı güncelleştirildi.

1. Bir sayının C# yerine iki tane `Counter` olmak üzere, bileşenin mantığını değiştirin.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Değişiklikleri görmek için uygulamayı yeniden derleyin ve çalıştırın. **Ben tıklama** düğmesini seçin. Sayaç iki olarak artar.

## <a name="use-components"></a>Bileşenleri kullanma

Bir bileşeni, bir HTML söz dizimini kullanarak başka bir bileşene ekleyin.

1. `Index`Bileşenebir öğe`Index`ekleyerek bileşeni uygulamanın bileşenine ekleyin (*Index. Razor*). `Counter` `<Counter />`

   Bu deneyim için Blazor istemci tarafı kullanıyorsanız, bileşen `SurveyPrompt` `Index` tarafından bir bileşen kullanılır. `<SurveyPrompt>` Öğesini bir`<Counter />` öğesiyle değiştirin. Bu deneyim için bir Blazor sunucu tarafı uygulaması kullanıyorsanız, `<Counter />` öğesini `Index` bileşene ekleyin:

   *Pages/Index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Uygulamayı yeniden derleyin ve çalıştırın. `Index` Bileşenin kendi sayacı vardır.

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenler de parametrelere sahip olabilir. Bileşen parametreleri, ile `[Parameter]`donatılmış bileşen sınıfında ortak özellikler kullanılarak tanımlanır. Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.

1. Bileşenin `@code` C# kodunu güncelleştirin:

   * Özniteliği ile donatılmış bir `IncrementAmount` özellik ekleyin. `[Parameter]`
   * Değerini değerini `IncrementAmount`artırdığınızda kullanmak için `IncrementCount` yöntemini değiştirin `currentCount`.

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Öznitelik kullanarak `IncrementAmount` `Index` bileşenin`<Counter>` öğesinde bir parametre belirtin. Sayacı on olarak artırmak için değeri ayarlayın.

   *Pages/Index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. `Index` Bileşeni yeniden yükleyin. **Beni tıklama** düğmesi seçildiğinde sayaç on bir kez artar. `Counter` Bileşendeki sayaç bir artırmaya devam eder.

## <a name="route-to-components"></a>Bileşenlere yönlendir

*Counter. Razor* dosyasının en üstündeki `Counter` yönerge,bileşeninbiryönlendirmeuçnoktasıolduğunubelirtir.`@page` `Counter` Bileşen öğesine`/counter`gönderilen istekleri işler. `@page` Yönerge olmadan, bileşen yönlendirilmiş istekleri işlemez, ancak bileşen diğer bileşenler tarafından hala kullanılabilir.

## <a name="dependency-injection"></a>Bağımlılık ekleme

Uygulamanın hizmet kapsayıcısına kayıtlı hizmetler, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)yoluyla bileşenler için kullanılabilir. `@inject` Yönergesi kullanarak bir bileşene hizmet ekleme.

`FetchData` Bileşenin yönergelerini inceleyin.

Blazor sunucu tarafı bir uygulamayla çalışıyorsanız, `WeatherForecastService` hizmet [tek](xref:fundamentals/dependency-injection#service-lifetimes)bir olarak kaydedilir, bu yüzden hizmetin bir örneği uygulama genelinde kullanılabilir. Yönergesi, `WeatherForecastService` hizmet örneğini bileşene eklemek için kullanılır. `@inject`

*Pages/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Bileşen, `WeatherForecast` nesne dizisini almak için olarak `ForecastService`eklenen hizmeti kullanır: `FetchData`

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Bir Blazor istemci tarafı uygulamasıyla çalışıyorsanız, `HttpClient` *wwwroot/Örnek-veri* klasöründeki *Hava durumu. JSON* dosyasından Hava durumu tahmin verileri elde etmek için eklenen:

*Pages/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

Bir foreach döngüsü, her tahmin örneğini Hava durumu verileri tablosunda bir satır olarak işlemek için kullanılır: [ \@](/dotnet/csharp/language-reference/keywords/foreach-in)

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a>Yapılacaklar listesi oluşturma

Uygulamaya basit bir yapılacaklar listesi uygulayan yeni bir bileşen ekleyin.

1. Uygulamalar klasörüne *Todo. Razor* adlı boş bir dosya ekleyin:

1. Bileşen için ilk biçimlendirmeyi belirtin:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. `Todo` Bileşeni gezinti çubuğuna ekleyin.

   Bileşen (*Shared/navmenu. Razor*) uygulamanın düzeninde kullanılır. `NavMenu` Düzenler, uygulamadaki içeriğin çoğaltılmasını önlemenize olanak sağlayan bileşenlerdir.

   `<NavLink>` *Paylaşılan/navmenu. Razor* dosyasındaki mevcut liste öğelerinin altına aşağıdaki liste öğesi işaretlemesini ekleyerek `Todo` bileşen için bir öğe ekleyin:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Uygulamayı yeniden derleyin ve çalıştırın. `Todo` Bileşen bağlantısının çalıştığından emin olmak için yeni Todo sayfasını ziyaret edin.

1. Bir Todo öğesini temsil eden bir sınıfı tutmak için projenin köküne bir *TodoItem.cs* dosyası ekleyin. `TodoItem` Sınıfı için aşağıdaki C# kodu kullanın:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Bileşene geri dön (*Pages/Todo. Razor*): `Todo`

   * Bir `@code` bloktaki Todo öğeleri için bir alan ekleyin. `Todo` Bileşen, Todo listesinin durumunu korumak için bu alanı kullanır.
   * Her Todo öğesini bir liste öğesi `foreach` (`<li>`) olarak işlemek için sıralanmamış liste işaretlemesi ve bir döngüsü ekleyin.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Uygulama, listeye Todo öğeleri eklemek için Kullanıcı arabirimi öğeleri gerektirir. Sıralanmamış listenin`<input>`(`<ul>...</ul>`) altına bir metin girişi ()`<button>`ve bir düğme () ekleyin:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Uygulamayı yeniden derleyin ve çalıştırın. **Todo Ekle** düğmesi seçildiğinde, bir olay işleyicisi düğmeye kablolu olmadığı için hiçbir şey olmaz.

1. `Todo` Bileşene bir `AddTodo` yöntem ekleyin ve `@onclick` özniteliği kullanarak düğme seçimleri için kaydedin. `AddTodo` Düğme seçildiğinde C# yöntemi çağrılır:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Yeni Todo öğesinin `newTodo` başlığını almak için, `@code` bloğunun üst kısmına bir dize alanı ekleyin ve bunu, `<input>` öğesindeki `bind` özniteliği kullanarak metin girişinin değerine bağlayın:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="@newTodo" />
   ```

1. Belirtilen başlığa `TodoItem` sahip öğesini listeye eklemek için yönteminigüncelleştirin.`AddTodo` Metin girişinin değerini boş bir dizeye ayarlayarak `newTodo` temizleyin:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Uygulamayı yeniden derleyin ve çalıştırın. Yeni kodu test etmek için Todo listesine bazı Todo öğeleri ekleyin.

1. Her Todo öğesi için başlık metni düzenlenebilir hale getirilebilir ve bir onay kutusu kullanıcının tamamlanmış öğeleri izlemesine yardımcı olabilir. Her Todo öğesi için bir onay kutusu girişi ekleyin ve değerini `IsDone` özelliğine bağlayın. Öğesine göre bir `<input>` öğeye`@todo.Title`geç: `@todo.Title`

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Bu değerlerin bağlandığını doğrulamak için `<h1>` üst bilgiyi, tamamlanmamış olan Todo öğelerinin sayısının sayısını gösterecek şekilde güncelleştirin (`IsDone` yani `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Tamamlanan `Todo` bileşen (*sayfa/Todo. Razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Uygulamayı yeniden derleyin ve çalıştırın. Yeni kodu test etmek için Todo öğeleri ekleyin.

> [!div class="nextstepaction"]
> <xref:blazor/components>
