---
title: İlk Blazor uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Blazor uygulaması derleme ve hızlı Razor bileşenleri framework'ün temel özellikleri öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668161"
---
# <a name="build-your-first-blazor-app"></a>İlk Blazor uygulamanızı oluşturun

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Bu öğreticide, bir adım adım Blazor uygulaması derleme ve hızlı Razor bileşenleri framework'ün temel özellikleri öğrenin.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)). Bkz: [başlama](xref:razor-components/get-started) Önkoşullar için konu.

Visual Studio projesi oluşturmak için:

1. **Dosya** > **Yeni** > **Proje**’yi seçin. Seçin **Web** > **ASP.NET Core Web uygulaması**. İçinde "BlazorApp1" proje adı **adı** alan. **Tamam**’ı seçin.

    ![Yeni ASP.NET Core projesi](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. **Yeni ASP.NET Core Web uygulaması** iletişim kutusu görüntülenir. Emin **.NET Core** üstünde seçilir. Seçin **ASP.NET Core 2.1**. Seçin **Blazor** şablonu seçip alt **Tamam**.

    ![Yeni Blazor uygulaması iletişim kutusu](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. Proje oluşturulduktan sonra basın **Ctrl-F5** uygulamayı çalıştırmak için *hata ayıklayıcı olmadan*. Hata ayıklayıcısı ile çalıştırma (**F5**) şu anda desteklenmiyor.

> [!NOTE]
> Aksi durumda Visual Studio kullanarak oluşturduğunuz Blazor uygulamayı Windows, macOS veya Linux üzerinde bir komut isteminde:
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> Localhost adresi ve bağlantı noktası konsol penceresi çıktısı sonra sağlanan kullanarak uygulamaya gidin `dotnet run` yürütülür. Kullanım **Ctrl-C** konsol penceresinde uygulamayı kapatmak için.

Blazor uygulama tarayıcıda çalışır:

![Blazor uygulama ana sayfası](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a>Yapı bileşenleri

1. Her bir uygulamanın üç göz atın: Giriş, sayaç ve veri getirilemiyor.

    Bu üç sayfaları üç Razor dosyalarında tarafından uygulanan *sayfaları* klasörü: *Index.cshtml*, *Counter.cshtml*, ve *FetchData.cshtml*. Bu dosyaların her biri, derlenmiş ve istemci tarafı tarayıcıda çalıştırılan bir bileşen uygular.

1. Sayaç sayfasında düğmesini seçin.

    ![Blazor uygulama ana sayfası](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    Düğme seçildiğinde her zaman bir sayfa yenileme sayaç artırılır. Normalde, bu tür bir istemci-tarafı davranışı JavaScript'te işlenir; ancak burada öğesinde uygulanır C# ve .NET tarafından `Counter` bileşeni.

1. Uygulamasını göz atın `Counter` bileşeninin *Counter.cshtml* dosyası:

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    Kullanıcı Arabiriminde `Counter` bileşen normal HTML kullanılarak tanımlanır. (Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür. Oluşturulan .NET sınıf adı dosya adıyla aynıdır.

    Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok. İçinde `@functions` engelleme, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilebilir. Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için bir parçası olarak kullanılabilir.

    Düğme seçildiğinde `Counter` bileşeni kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi) ve `Counter` bileşen kendi işleme ağacında yeniden oluşturur. Blazor, yeni bir işleme ağacı Öncekine karşı karşılaştırır ve tarayıcı belge nesne modeli (DOM) yapılan tüm değişiklikler uygulanır. Görüntülenen sayısı güncelleştirilir.

1. İşaretleme için güncelleştirme `Counter` daha üst düzey başlık olmak için bileşeni *heyecan verici*.

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. Ayrıca değişiklik C# mantığını `Counter` bir yerine ikiye sayısı artması yapmak bileşeni.

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. Sayaç sayfanın değişiklikleri görmek için tarayıcıyı yenileyin.

    ![Heyecan verici sayacı](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a>Bileşenleri kullanma

Bir bileşen tanımlandıktan sonra bileşenin diğer bileşenlere uygulamak için kullanılabilir. Bir bileşen kullanma için işaretleme, etiketin adını bileşen türü olduğu gibi HTML etiketleri arar.

1. Ekleme bir `Counter` ana sayfaya uygulaması bileşen (*Index.cshtml*).

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. Giriş sayfasını tarayıcıda yenileyin. Ayrı bir örneğini Not `Counter` giriş sayfasında bileşen.

    ![Sayaç ile Blazor giriş sayfası](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenleri de özel özellikleri ile donatılmış bileşen sınıfı kullanarak tanımlanan parametreler sahip `[Parameter]`. Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın. 

1. Güncelleştirme `Counter` sahip bileşen bir `IncrementAmount` varsayılan olarak 1 parametresi.

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > Visual Studio'dan kolayca bir bileşen parametresi kullanarak ekleyebilirsiniz `para` kod parçacığı. Tür `para` ve tuşuna `Tab` tuşunu iki kez.

1. Giriş sayfasında (*Index.cshtml*), artış için değiştirebilirsiniz `Counter` bileşenin özelliği adıyla eşleşen bir öznitelik ayarlayarak 10 `IncrementCount`.

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. Sayfayı yeniden yükleyin.

    Sayacın sayaç sayfasında hala 1 artar ancak 10 giriş sayfası artık artışlarla üzerinde sayacı.

    ![On göre Blazor sayısı](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a>Bileşenleri için yol

`@page` En üstündeki yönerge *Counter.cshtml* dosya bu bileşen için istekleri yönlendirilir bir sayfa olduğunu belirtir. Özellikle, `Counter` bileşeni gönderilen istekleri işler `/Counter`. Olmadan `@page` yönergesi, bileşen yönlendirilmiş istekleri işleyen mıydı ancak bileşeni hala başka bileşenler tarafından kullanılabilir.

## <a name="dependency-injection"></a>Bağımlılık ekleme

Bileşenleri uygulamanın hizmet sağlayıcısına kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Hizmetler eklenen halinde bileşenini kullanarak `@inject` yönergesi.

Uygulamasını göz atın `FetchData` bileşeninin *FetchData.cshtml*. `@inject` Yönergesi ekleme için kullanılan bir [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) örneğine bileşeni.

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

`FetchData` Bileşen eklenen `HttpClient` bileşeni başlatılırken JSON verileri sunucudan almak için. Kapak altında `HttpClient` isteği göndermek için temel alınan tarayıcının Fetch API'sini çağırmak için JavaScript birlikte çalışması kullanarak çalışma zamanı uygulanan Blazor tarafından sağlanan (gelen C#, herhangi bir JavaScript kitaplığı veya tarayıcı API'yi çağırmak mümkündür). Alınan verileri seri durumdan `forecasts` C# bir dizi değişkenini `WeatherForecast` nesneleri.

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

A `@foreach` döngü, her bir tahmin örneği hava durumu tablosundaki bir satır olarak işlemek için kullanılır.

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a>Bir Yapılacaklar listesi oluşturun

Bir basit bir Yapılacaklar listesi uygulayan uygulamaya yeni bir sayfa ekleyin.

1. Boş bir metin dosyası için ekleme *sayfaları* adlı klasöre *Todo.cshtml*.

1. Sayfa için ilk biçimlendirme sağlar.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. Todo sayfası güncelleştirerek Gezinti çubuğuna ekleyin *Shared/NavMenu.cshtml*. Ekleme bir `NavLink` aşağıdaki liste öğesi işaretleme varolan liste öğelerini aşağıda ekleyerek Todo sayfası.

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. Uygulamayı tarayıcıda yenileyin. Yeni Todo sayfasına bakın.

    ![Blazor todo Başlat](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. Ekleme bir *TodoItem.cs* todo öğelerini temsil etmek için bir sınıf tutmak için projenin kök dosya.

1. Aşağıdaki C# için kod `TodoItem` sınıfı.

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. Geri Git `Todo` bileşeninin *Todo.cshtml* ve içinde açıklamada için bir alan eklemek bir `@functions` blok. `Todo` Bileşen Yapılacaklar listesi durumunu korumak için bu alanı kullanır.

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. Uygulama, açıklamada listeye eklemek için kullanıcı Arabirimi öğeleri gerektirir. Bir metin girişi ve listenin altındaki bir düğme ekleyin.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Tarayıcıyı yenileyin.

    ![TODO düğmesi ekleme](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    Hiçbir şey olmaz, **todo ekleyin** düğmesi seçili hiçbir olay işleyicisi, düğmenin kadar kablolu olduğundan.

1. Ekleme bir `AddTodo` yönteme `Todo` bileşeni ve kayıt için düğmeyi tıkladığında kullanarak `onclick` özniteliği.

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    `AddTodo` C# Düğme seçili her zaman yöntemi çağrılır.

1. Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği.

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip. Metin girişi değerini ayarlayarak temizlemeyi unutmayın `newTodo` boş bir dize.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. Tarayıcıyı yenileyin. Bazı açıklamada yapılacaklar listesine ekleyin.

    ![Açıklamada Ekle](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. Son olarak, onay kutularını olmadan bir Yapılacaklar listesi nedir? Her bir todo öğesi için başlık metni düzenlenebilir de yapılabilir. Bir onay kutusu giriş ve her bir todo öğesi için metin girişi ekleme ve değerlerine bağlama `Title` ve `IsDone` özellikleri, sırasıyla.

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `h1` henüz yapılmadı todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. Tamamlanan `Todo` bileşeni şu şekilde görünmelidir:

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

Uygulamayı tarayıcıda yenileyin. Birkaç Yapılacaklar öğesi eklemeyi deneyin.

![Tamamlanmış Blazor Yapılacaklar listesi](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a>Yayımlama ve dağıtma

Visual Studio kullanırken Todo Blazor uygulamasını Azure'da yayımlamak için aşağıdaki adımları gerçekleştirin:

1. Projeye sağ tıklayarak **Çözüm Gezgini** seçip **Yayımla**.

1. İçinde **yayımlama hedefi seçin** iletişim kutusunda **App Service** ve **Yeni Oluştur**. **Yayımla**’yı seçin.

    ![Yayımlama hedefi seçin](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. İçinde **App Service Oluştur** iletişim kutusunda, uygulama için bir ad seçin ve abonelik, kaynak grubu ve barındırma planı seçin. Seçin **Oluştur** app service oluşturma ve uygulamayı yayımlayın.

    ![App service oluştur](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

Veya bunu dağıtılacak uygulama için bir dakika bekleyin.

Uygulama artık Azure'da çalıştırıyor olmalıdır. Todo öğesini ilk Blazor uygulamanızı oluşturun işaretlemek *Bitti*. İyi iş!

![Azure'da Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> Aksi durumda Visual Studio'yu kullanarak Windows, macOS veya Linux üzerinde bir komut isteminde Blazor uygulama yayımlama:
>
> ```console
> dotnet publish -c Release
> ```
>
> Dağıtım oluşturulan */bin/yayın/\<hedef çerçeve > / publish* klasör. İçeriği Taşı *yayımlama* klasörü sunucu ya da barındırma hizmeti.
>
> Daha fazla bilgi için [konak dağıtıp](xref:host-and-deploy/razor-components/index#publish-the-app) konu.

## <a name="additional-resources"></a>Ek kaynaklar

Daha Blazor örnek uygulamasını dahil göz atın [uçuş Bulucu örnek](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) GitHub üzerinde.

![Blazor uçuş Bulucu](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
