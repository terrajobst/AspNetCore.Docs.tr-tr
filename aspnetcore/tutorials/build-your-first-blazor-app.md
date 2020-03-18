---
title: İlk Blazor uygulamanızı oluşturma
author: guardrex
description: Blazor uygulaması oluşturun adım adım.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 8b3802a6ffe3613e5d4ca65c57fafc3f404c8329
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434505"
---
# <a name="build-your-first-opno-locblazor-app"></a>İlk Blazor uygulamanızı oluşturma

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Bu öğreticide bir Blazor uygulamasının nasıl oluşturulacağı ve değiştirileceği gösterilmektedir.

## <a name="build-components"></a>Derleme bileşenleri

1. Bu öğreticide bir Blazor projesi oluşturmak için <xref:blazor/get-started> makalesindeki yönergeleri izleyin. Projeyi *ToDoList*olarak adlandırın.

1. *Sayfalar* klasöründe uygulamanın üç sayfasının her birine gidin: giriş, sayaç ve veri getirme. Bu sayfalar, Razor bileşen dosyaları *dizini. Razor*, *Counter. Razor*ve *fetchdata. Razor*tarafından uygulanır.

1. Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin. Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir. Blazor, bunun yerine yazabilirsiniz C# .

1. *Counter. Razor* dosyasındaki `Counter` bileşeninin uygulamasını inceleyin.

   *Pages/Counter. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   `Counter` bileşenin kullanıcı arabirimi HTML kullanılarak tanımlanır. Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir. HTML biçimlendirme ve C# işleme mantığı, derleme zamanında bir bileşen sınıfına dönüştürülür. Oluşturulan .NET sınıfının adı dosya adıyla eşleşir.

   Bileşen sınıfının üyeleri bir `@code` bloğunda tanımlanmıştır. `@code` bloğunda, bileşen durumu (özellikler, alanlar) ve yöntemler olay işleme için veya diğer bileşen mantığını tanımlamak için belirtilir. Bu Üyeler daha sonra bileşenin işleme mantığının bir parçası olarak ve olayları işlemek için kullanılır.

   **Bana tıklama** düğmesi seçildiğinde:

   * `Counter` bileşenin kayıtlı `onclick` işleyicisine (`IncrementCount` yöntemi) denir.
   * `Counter` bileşeni, işleme ağacını yeniden oluşturur.
   * Yeni işleme ağacı öncekiyle karşılaştırılır.
   * Yalnızca Belge Nesne Modeli (DOM) üzerinde yapılan değişiklikler uygulanır. Görünen sayı güncelleştirildi.

1. Sayıyı bir C# yerine iki ile artırmak için `Counter` bileşenin mantığını değiştirin.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Değişiklikleri görmek için uygulamayı yeniden derleyin ve çalıştırın. **Ben tıklama** düğmesini seçin. Sayaç iki olarak artar.

## <a name="use-components"></a>Bileşenleri kullanma

Bir bileşeni, bir HTML söz dizimini kullanarak başka bir bileşene ekleyin.

1. `Index` bileşenine bir `<Counter />` öğesi ekleyerek uygulamanın `Index` bileşenine `Counter` bileşenini ekleyin (*Index. Razor*).

   Bu deneyim için Blazor WebAssembly kullanıyorsanız, `Index` bileşeni tarafından `SurveyPrompt` bir bileşen kullanılır. `<SurveyPrompt>` öğesini bir `<Counter />` öğesiyle değiştirin. Bu deneyim için bir Blazor sunucusu uygulaması kullanıyorsanız, `Index` bileşenine `<Counter />` öğesini ekleyin:

   *Pages/Index. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Uygulamayı yeniden derleyin ve çalıştırın. `Index` bileşeni kendi sayacıdır.

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenler de parametrelere sahip olabilir. Bileşen parametreleri, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanır. Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.

1. Bileşenin `@code` C# kodunu aşağıdaki gibi güncelleştirin:

   * `[Parameter]` özniteliğiyle ortak bir `IncrementAmount` özelliği ekleyin.
   * `IncrementCount` yöntemini, `currentCount`değerini artırıldığında `IncrementAmount` özelliğini kullanacak şekilde değiştirin.

   *Pages/Counter. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. Özniteliği kullanarak `Index` bileşeninin `<Counter>` öğesinde bir `IncrementAmount` parametresi belirtin. Sayacı on olarak artırmak için değeri ayarlayın.

   *Pages/Index. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. `Index` bileşenini yeniden yükleyin. **Beni tıklama** düğmesi seçildiğinde sayaç on bir kez artar. `Counter` bileşenindeki sayaç bir artış ile devam eder.

## <a name="route-to-components"></a>Bileşenlere yönlendir

*Counter. Razor* dosyasının en üstündeki `@page` yönergesi, `Counter` bileşeninin bir yönlendirme uç noktası olduğunu belirtir. `Counter` bileşeni `/counter`gönderilen istekleri işler. `@page` yönergesi olmadan, bileşen yönlendirilmiş istekleri işlemez, ancak bileşen diğer bileşenler tarafından hala kullanılabilir.

## <a name="dependency-injection"></a>Bağımlılık ekleme

### <a name="opno-locblazor-server-experience"></a>Blazor sunucusu deneyimi

Blazor sunucu uygulamasıyla çalışıyorsanız, `WeatherForecastService` hizmeti bir [tek](xref:fundamentals/dependency-injection#service-lifetimes) `Startup.ConfigureServices`olarak kaydedilir. Uygulamanın tamamında [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)yoluyla hizmetin bir örneği mevcuttur:

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

`@inject` yönergesi, `WeatherForecastService` hizmetinin örneğini `FetchData` bileşenine eklemek için kullanılır.

*Pages/FetchData. Razor*:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

`FetchData` bileşeni, `WeatherForecast` nesnelerinin bir dizisini almak için, `ForecastService`olarak eklenen hizmeti kullanır:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a>Blazor Weelsembly deneyimi

Blazor WebAssembly uygulamasıyla çalışıyorsanız, *Wwwroot/Sample-Data* klasöründeki *Hava durumu. JSON* dosyasından Hava durumu tahmin verileri almak için `HttpClient` eklenir.

*Pages/FetchData. Razor*:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) döngüsü, her tahmin örneğini Hava durumu verileri tablosunda bir satır olarak işlemek için kullanılır:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Yapılacaklar listesi oluşturma

Uygulamaya basit bir yapılacaklar listesi uygulayan yeni bir bileşen ekleyin.

1. Yeni bir `Todo` Razor bileşenini *Sayfalar* klasöründe uygulamaya ekleyin. Visual Studio 'da **Sayfalar** klasörüne sağ tıklayın ve > **yeni öğe** **Ekle** > **Razor bileşeni**' ni seçin. Bileşenin dosya *Todo. Razor*olarak adlandırın. Diğer geliştirme ortamlarında, *Todo. Razor*adlı **Sayfalar** klasörüne boş bir dosya ekleyin.

1. Bileşen için ilk biçimlendirmeyi belirtin:

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. Gezinti çubuğuna `Todo` bileşenini ekleyin.

   `NavMenu` bileşeni (*Shared/NavMenu. Razor*) uygulamanın düzeninde kullanılır. Düzenler, uygulamadaki içeriğin çoğaltılmasını önlemenize olanak sağlayan bileşenlerdir.

   Aşağıdaki liste öğesi işaretlemesini *paylaşılan/NavMenu. Razor* dosyasında var olan liste öğelerinin altına ekleyerek `Todo` bileşeni için bir `<NavLink>` öğesi ekleyin:

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Uygulamayı yeniden derleyin ve çalıştırın. `Todo` bileşeni bağlantısının çalıştığından emin olmak için yeni Todo sayfasını ziyaret edin.

1. Bir Todo öğesini temsil eden bir sınıfı tutmak için projenin köküne bir *TodoItem.cs* dosyası ekleyin. `TodoItem` sınıfı için C# aşağıdaki kodu kullanın:

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. `Todo` bileşenine geri dönün (*Pages/Todo. Razor*):

   * `@code` bloğundaki Todo öğeleri için bir alan ekleyin. `Todo` bileşeni, Todo listesinin durumunu korumak için bu alanı kullanır.
   * Her Todo öğesini bir liste öğesi (`<li>`) olarak işlemek için sıralanmamış liste işaretlemesi ve bir `foreach` döngüsü ekleyin.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Uygulama, listeye Todo öğeleri eklemek için Kullanıcı arabirimi öğeleri gerektirir. Sıralanmamış listenin (`<ul>...</ul>`) altına bir metin girişi (`<input>`) ve bir düğme (`<button>`) ekleyin:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Uygulamayı yeniden derleyin ve çalıştırın. **Todo Ekle** düğmesi seçildiğinde, bir olay işleyicisi düğmeye kablolu olmadığı için hiçbir şey olmaz.

1. `Todo` bileşenine bir `AddTodo` yöntemi ekleyin ve `@onclick` özniteliğini kullanarak düğme seçimleri için kaydedin. Düğme seçildiğinde C# `AddTodo` yöntemi çağrılır:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Yeni Todo öğesinin başlığını almak için, `@code` bloğunun üst kısmına bir `newTodo` dize alanı ekleyin ve `<input>` öğesindeki `bind` özniteliğini kullanarak metin girişinin değerine bağlayın:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. `AddTodo` yöntemini, belirtilen başlığa sahip `TodoItem` listeye eklemek için güncelleştirin. `newTodo` boş bir dizeye ayarlayarak metin girişinin değerini temizleyin:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Uygulamayı yeniden derleyin ve çalıştırın. Yeni kodu test etmek için Todo listesine bazı Todo öğeleri ekleyin.

1. Her Todo öğesi için başlık metni düzenlenebilir hale getirilebilir ve bir onay kutusu kullanıcının tamamlanmış öğeleri izlemesine yardımcı olabilir. Her Todo öğesi için bir onay kutusu girişi ekleyin ve değerini `IsDone` özelliğine bağlayın. `@todo.Title`, `@todo.Title`bağlantılı `<input>` bir öğe olarak değiştirin:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Bu değerlerin bağlandığını doğrulamak için `<h1>` üst bilgisini, tamamlanmamış olan Todo öğelerinin sayısının sayısını gösterecek şekilde güncelleştirin (`IsDone` `false`).

   ```razor
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Tamamlanan `Todo` bileşeni (*Sayfalar/Todo. Razor*):

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Uygulamayı yeniden derleyin ve çalıştırın. Yeni kodu test etmek için Todo öğeleri ekleyin.

> [!div class="nextstepaction"]
> <xref:blazor/components>
