---
title: İlk Blazor uygulamanızı oluşturma
author: guardrex
description: Adım adım Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 1c0492106b888665c23932f16cc83d22947956d2
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334044"
---
# <a name="build-your-first-blazor-app"></a>İlk Blazor uygulamanızı oluşturma

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Bu öğreticide bir Blazor uygulamasının nasıl oluşturulacağı ve değiştirileceği gösterilmektedir.

Bu öğreticide bir Blazor projesi oluşturmak için <xref:blazor/get-started> makalesindeki yönergeleri izleyin. Projeyi *ToDoList*olarak adlandırın.

## <a name="build-components"></a>Derleme bileşenleri

1. *Sayfalar* klasöründe uygulamanın üç sayfasının her birine gidin: giriş, sayaç ve veri getirme. Bu sayfalar, Razor bileşen dosyaları *dizini. Razor*, *Counter. Razor*ve *fetchdata. Razor*tarafından uygulanır.

1. Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin. Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak Blazor kullanarak C#daha iyi bir yaklaşım sağlar.

1. *Counter. Razor* dosyasındaki `Counter` bileşeninin uygulamasını inceleyin.

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   @No__t-0 bileşeninin Kullanıcı arabirimi HTML kullanılarak tanımlanır. Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir. HTML biçimlendirme ve C# işleme mantığı, derleme zamanında bir bileşen sınıfına dönüştürülür. Oluşturulan .NET sınıfının adı dosya adıyla eşleşir.

   Bileşen sınıfının üyeleri `@code` bloğunda tanımlanır. @No__t-0 bloğunda, bileşen durumu (özellikler, alanlar) ve yöntemler olay işleme için veya diğer bileşen mantığını tanımlamak için belirtilir. Bu Üyeler daha sonra bileşenin işleme mantığının bir parçası olarak ve olayları işlemek için kullanılır.

   **Bana tıklama** düğmesi seçildiğinde:

   * @No__t-0 bileşeninin kayıtlı `onclick` işleyicisi (`IncrementCount` yöntemi) olarak adlandırılır.
   * @No__t-0 bileşeni, kendi işleme ağacını yeniden oluşturur.
   * Yeni işleme ağacı öncekiyle karşılaştırılır.
   * Yalnızca Belge Nesne Modeli (DOM) üzerinde yapılan değişiklikler uygulanır. Görünen sayı güncelleştirildi.

1. @No__t- C# 1 bileşeninin mantığını değiştirerek sayıyı bir tane yerine iki ile artırın.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Değişiklikleri görmek için uygulamayı yeniden derleyin ve çalıştırın. **Ben tıklama** düğmesini seçin. Sayaç iki olarak artar.

## <a name="use-components"></a>Bileşenleri kullanma

Bir bileşeni, bir HTML söz dizimini kullanarak başka bir bileşene ekleyin.

1. @No__t-3 bileşenine (*Index. Razor*) bir `<Counter />` öğesi ekleyerek uygulamanın `Index` bileşenine `Counter` bileşenini ekleyin.

   Bu deneyim için Blazor WebAssembly kullanıyorsanız, `SurveyPrompt` bileşeni `Index` bileşeni tarafından kullanılır. @No__t-0 öğesini bir `<Counter />` öğesiyle değiştirin. Bu deneyim için bir Blazor Server uygulaması kullanıyorsanız, `<Counter />` öğesini `Index` bileşenine ekleyin:

   *Pages/Index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Uygulamayı yeniden derleyin ve çalıştırın. @No__t-0 bileşeninin kendi sayacı vardır.

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenler de parametrelere sahip olabilir. Bileşen parametreleri, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanır. Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.

1. Bileşenin `@code` C# kodunu güncelleştirin:

   * @No__t-1 özniteliğiyle ortak bir `IncrementAmount` özelliği ekleyin.
   * @No__t-0 yöntemini `currentCount` değerini artırdığınızda `IncrementAmount` ' i kullanacak şekilde değiştirin.

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Bir öznitelik kullanarak `Index` bileşeninin `<Counter>` öğesinde bir `IncrementAmount` parametresi belirtin. Sayacı on olarak artırmak için değeri ayarlayın.

   *Pages/Index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. @No__t-0 bileşenini yeniden yükleyin. **Beni tıklama** düğmesi seçildiğinde sayaç on bir kez artar. @No__t-0 bileşenindeki sayaç bir artış ile devam eder.

## <a name="route-to-components"></a>Bileşenlere yönlendir

*Counter. Razor* dosyasının en üstündeki `@page` yönergesi, `Counter` bileşeninin bir yönlendirme uç noktası olduğunu belirtir. @No__t-0 bileşeni, `/counter` ' e gönderilen istekleri işler. @No__t-0 yönergesi olmadan bir bileşen yönlendirilmiş istekleri işlemez, ancak bileşen diğer bileşenler tarafından hala kullanılabilir.

## <a name="dependency-injection"></a>Bağımlılık ekleme

### <a name="blazor-server-experience"></a>Blazor sunucusu deneyimi

Bir Blazor sunucu uygulamasıyla çalışıyorsanız, `WeatherForecastService` hizmeti `Startup.ConfigureServices` ' de [tek](xref:fundamentals/dependency-injection#service-lifetimes) bir kayıt olarak kaydedilir. Uygulamanın tamamında [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)yoluyla hizmetin bir örneği mevcuttur:

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

@No__t-0 yönergesi, `WeatherForecastService` hizmetinin örneğini `FetchData` bileşenine eklemek için kullanılır.

*Pages/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

@No__t-0 bileşeni, `WeatherForecast` nesnelerinin bir dizisini almak için eklenen hizmeti `ForecastService` olarak kullanır:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a>Blazor WebAssembly deneyimi

Blazor WebAssembly uygulamasıyla çalışıyorsanız, *Wwwroot/Sample-Data* klasöründeki *Hava durumu. JSON* dosyasından Hava durumu tahmin verileri almak için `HttpClient` eklenir.

*Pages/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

[@No__t-1](/dotnet/csharp/language-reference/keywords/foreach-in) döngüsü, her tahmin örneğini Hava durumu verileri tablosunda bir satır olarak işlemek için kullanılır:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Yapılacaklar listesi oluşturma

Uygulamaya basit bir yapılacaklar listesi uygulayan yeni bir bileşen ekleyin.

1. Uygulamalar *klasörüne* *Todo. Razor* adlı boş bir dosya ekleyin:

1. Bileşen için ilk biçimlendirmeyi belirtin:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. @No__t-0 bileşenini gezinti çubuğuna ekleyin.

   @No__t-0 bileşeni (*Shared/NavMenu. Razor*) uygulamanın düzeninde kullanılır. Düzenler, uygulamadaki içeriğin çoğaltılmasını önlemenize olanak sağlayan bileşenlerdir.

   Aşağıdaki liste öğesi işaretlemesini *paylaşılan/NavMenu. Razor* dosyasında var olan liste öğelerinin altına ekleyerek `Todo` bileşeni için `<NavLink>` öğesi ekleyin:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Uygulamayı yeniden derleyin ve çalıştırın. @No__t-0 bileşeni bağlantısının çalıştığından emin olmak için yeni Todo sayfasını ziyaret edin.

1. Bir Todo öğesini temsil eden bir sınıfı tutmak için projenin köküne bir *TodoItem.cs* dosyası ekleyin. @No__t-1 C# sınıfı için aşağıdaki kodu kullanın:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. @No__t-0 bileşenine geri dönün (*Pages/Todo. Razor*):

   * @No__t-0 bloğunda Todo öğeleri için bir alan ekleyin. @No__t-0 bileşeni, Todo listesinin durumunu korumak için bu alanı kullanır.
   * Her Todo öğesini bir liste öğesi (`<li>`) olarak işlemek için sıralanmamış liste işaretlemesi ve bir `foreach` döngüsü ekleyin.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Uygulama, listeye Todo öğeleri eklemek için Kullanıcı arabirimi öğeleri gerektirir. Sıralanmamış listenin altına bir metin girişi (`<input>`) ve bir düğme (`<button>`) ekleyin (`<ul>...</ul>`):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Uygulamayı yeniden derleyin ve çalıştırın. **Todo Ekle** düğmesi seçildiğinde, bir olay işleyicisi düğmeye kablolu olmadığı için hiçbir şey olmaz.

1. @No__t-1 bileşenine `AddTodo` yöntemi ekleyin ve `@onclick` özniteliğini kullanarak düğme seçimleri için kaydedin. @No__t-0 C# yöntemi düğme seçildiğinde çağrılır:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Yeni Todo öğesinin başlığını almak için, `@code` bloğunun üst kısmına bir `newTodo` dize alanı ekleyin ve `<input>` öğesindeki `bind` özniteliğini kullanarak metin girişinin değerine bağlayın:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. @No__t-0 yöntemini, belirtilen başlığa sahip `TodoItem` ' i listeye eklemek için güncelleştirin. @No__t-0 ' i boş bir dizeye ayarlayarak metin girişinin değerini temizleyin:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Uygulamayı yeniden derleyin ve çalıştırın. Yeni kodu test etmek için Todo listesine bazı Todo öğeleri ekleyin.

1. Her Todo öğesi için başlık metni düzenlenebilir hale getirilebilir ve bir onay kutusu kullanıcının tamamlanmış öğeleri izlemesine yardımcı olabilir. Her Todo öğesi için bir onay kutusu girişi ekleyin ve değerini `IsDone` özelliğine bağlayın. @No__t-0 ' yı `@todo.Title` ' e bağlanacak bir `<input>` öğesine değiştirin:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Bu değerlerin bağlandığını doğrulamak için `<h1>` üst bilgisini, tamamlanmamış olan Todo öğelerinin sayısını gösterecek şekilde güncelleştirin (`IsDone` `false` ' dir).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Tamamlanan `Todo` bileşeni (*Sayfalar/Todo. Razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Uygulamayı yeniden derleyin ve çalıştırın. Yeni kodu test etmek için Todo öğeleri ekleyin.

> [!div class="nextstepaction"]
> <xref:blazor/components>
