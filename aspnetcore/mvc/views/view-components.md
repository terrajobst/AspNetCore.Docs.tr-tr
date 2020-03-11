---
title: ASP.NET Core bileşenleri görüntüleme
author: rick-anderson
description: Görünüm bileşenlerinin ASP.NET Core nasıl kullanıldığını ve uygulamalara nasıl ekleneceğini öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
uid: mvc/views/view-components
ms.openlocfilehash: 910fffbf360ed0f62f7fe20bc8bfdf5be8198876
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660652"
---
# <a name="view-components-in-aspnet-core"></a>ASP.NET Core bileşenleri görüntüleme

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="view-components"></a>Görünüm bileşenleri

Görünüm bileşenleri kısmi görünümlere benzerdir, ancak çok daha güçlüdür. Görüntüleme bileşenleri model bağlama kullanmaz ve yalnızca içine çağrılırken belirtilen verilere bağımlıdır. Bu makale, denetleyiciler ve görünümler kullanılarak yazılmıştır, ancak bileşenleri görüntüle Razor Pages ile de çalışır.

Bir görünüm bileşeni:

* Tam yanıt yerine bir öbek işler.
* , Bir denetleyici ve görünüm arasında aynı sorun ayrımı ve test edilebilirlik avantajları içerir.
* Parametrelere ve iş mantığına sahip olabilir.
* Genellikle bir düzen sayfasından çağrılır.

Görünüm bileşenleri, kısmi bir görünüm için çok karmaşık olan işleme mantığınızı her yerde, örneğin:

* Dinamik gezinti menüleri
* Etiket bulutu (veritabanını sorgular)
* Oturum açma paneli
* Alışveriş sepeti
* Son yayımlanan makaleler
* Normal blogdaki kenar çubuğu içeriği
* Her sayfada işlenecek bir oturum açma paneli ve kullanıcının oturum açma durumuna bağlı olarak oturum açma veya oturum açma bağlantılarını gösterme

Bir görünüm bileşeni iki bölümden oluşur: Sınıf (genellikle [Viewcomponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)öğesinden türetilir) ve döndürdüğü sonuç (genellikle bir görünüm). Denetleyiciler gibi, bir görünüm bileşeni de bir POCO olabilir, ancak çoğu geliştirici `ViewComponent`türeterek kullanılabilir yöntemler ve özelliklerden faydalanmak ister.

Görünüm bileşenlerinin bir uygulamanın belirtimlerini karşılayıp karşılamadığını düşünürken, bunun yerine Razor bileşenleri kullanmayı göz önünde bulundurun. Razor bileşenleri ayrıca yeniden kullanılabilir Kullanıcı C# arabirimi birimleri oluşturmak için biçimlendirmeyi kodla birleştirir. Razor bileşenleri, istemci tarafı UI mantığı ve kompozisyonu sağlarken geliştirici üretkenliği için tasarlanmıştır. Daha fazla bilgi için bkz. <xref:blazor/components>.

## <a name="creating-a-view-component"></a>Görünüm bileşeni oluşturma

Bu bölüm, bir görünüm bileşeni oluşturmak için üst düzey gereksinimleri içerir. Makalenin ilerleyen kısımlarında, her adımı ayrıntılı olarak inceleyecek ve bir görünüm bileşeni oluşturacağız.

### <a name="the-view-component-class"></a>Görünüm bileşeni sınıfı

Bir görünüm bileşeni sınıfı, aşağıdakilerden herhangi biri tarafından oluşturulabilir:

* *Viewcomponent* 'dan türetme
* `[ViewComponent]` özniteliğiyle bir sınıfı dekorasyon veya `[ViewComponent]` özniteliğiyle bir sınıftan türetiliyor
* Sonekin son ek *Viewcomponent* ile bittiği bir sınıf oluşturma

Denetleyiciler gibi, görünüm bileşenleri ortak, iç içe olmayan ve soyut olmayan sınıflar olmalıdır. Görünüm bileşeni adı, "ViewComponent" sonekini kaldıran sınıf adıdır. Ayrıca, `ViewComponentAttribute.Name` özelliği kullanılarak açıkça de belirlenebilir.

Bir görünüm bileşeni Sınıfı:

* Oluşturucu [bağımlılığı ekleme](../../fundamentals/dependency-injection.md) işlemini tamamen destekler

* , Bir görünüm bileşeninde [filtre](../controllers/filters.md) kullanamayabilmeniz için denetleyicinin yaşam döngüsünde bir parçası almaz

### <a name="view-component-methods"></a>Bileşen yöntemlerini görüntüle

Bir görünüm bileşeni, bir `Task<IViewComponentResult>` döndüren `InvokeAsync` yöntemde veya bir `IViewComponentResult`döndüren zaman uyumlu `Invoke` yönteminde mantığını tanımlar. Parametreler, model bağlamalarından değil doğrudan görünüm bileşeni çağrısından gelir. Bir görünüm bileşeni hiçbir şekilde isteği doğrudan işlemez. Genellikle, bir görünüm bileşeni bir modeli başlatır ve `View` yöntemini çağırarak onu bir görünüme geçirir. Özet bölümünde bileşen yöntemlerini görüntüleyin:

* Bir `IViewComponentResult`döndüren bir `Task<IViewComponentResult>` ya da zaman uyumlu `Invoke` yöntemi döndüren `InvokeAsync` yöntemi tanımlayın.
* Genellikle bir modeli başlatır ve `ViewComponent` `View` yöntemini çağırarak bir görünüme geçirir.
* Parametreler, HTTP değil çağırma yönteminden gelir. Model bağlama yok.
* Doğrudan bir HTTP uç noktası olarak erişilemez. Bunlar, kodunuzun içinden çağrılır (genellikle bir görünümde). Bir görünüm bileşeni hiçbir şekilde isteği hiçbir şekilde işlemez.
* Geçerli HTTP isteğindeki tüm ayrıntılar yerine İmzada aşırı yüklendi.

### <a name="view-search-path"></a>Arama yolunu görüntüle

Çalışma zamanı, görünümü aşağıdaki yollarda arar:

* /Views/{Controller adı}/Components/{View bileşen adı}/{View Name}
* /Views/Shared/Components/{View bileşen adı}/{View Name}
* /Pages/Shared/Components/{View bileşen adı}/{View Name}

Arama yolu, denetleyiciler + görünümler ve Razor Pages kullanan projeler için geçerlidir.

Bir görünüm bileşeni için varsayılan görünüm adı *varsayılandır*, yani görünüm dosyanız genellikle *default. cshtml*olarak adlandırılır. Görünüm bileşeni sonucunu oluştururken veya `View` yöntemini çağırırken farklı bir görünüm adı belirtebilirsiniz.

Görünüm dosyasını *default. cshtml* olarak yazmanız ve *görünümleri/paylaşılan/bileşenler/{görünüm bileşen adı}/{View Name}* yolunu kullanmanız önerilir. Bu örnekte kullanılan `PriorityList` görünüm bileşeni, görünüm bileşeni görünümü için *Görünümler/paylaşılan/bileşenler/PriorityList/default. cshtml* kullanır.

### <a name="customize-the-view-search-path"></a>Arama yolunu görüntüle

Arama yolunu görüntüle ' yi özelleştirmek için Razor 'nin <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.ViewLocationFormats> koleksiyonunu değiştirin. Örneğin, "/Components/{View bileşen adı}/{View Name}" yolu içinde görünümleri aramak için, koleksiyona yeni bir öğe ekleyin:

[!code-cs[](view-components/samples_snapshot/2.x/Startup.cs?name=snippet_ViewLocationFormats&highlight=4)]

Yukarıdaki kodda, "{0}" yer tutucusu "bileşenler/{görünüm bileşeni adı}/{View Name}" yolunu temsil eder.

## <a name="invoking-a-view-component"></a>Bir görünüm bileşenini çağırma

Görünüm bileşenini kullanmak için, aşağıdakileri bir görünüm içinde çağırın:

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

Parametreler `InvokeAsync` yöntemine geçirilir. Makalede geliştirilen `PriorityList` görünüm bileşeni, *Görünümler/Todo/Index. cshtml* görünüm dosyasından çağrılır. Aşağıdaki şekilde `InvokeAsync` yöntemi iki parametreyle çağrılır:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Bir görünüm bileşenini etiket Yardımcısı olarak çağırma

ASP.NET Core 1,1 ve üzeri için bir görünüm bileşenini [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro)olarak çağırabilirsiniz:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

Etiket Yardımcıları için Pascal özellikli sınıf ve Yöntem parametreleri, [Kebab durumlarına](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)çevrilir. Bir görünüm bileşenini çağırmak için etiket Yardımcısı `<vc></vc>` öğesini kullanır. Görünüm bileşeni aşağıdaki gibi belirtilir:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Bir görünüm bileşenini etiket Yardımcısı olarak kullanmak için, `@addTagHelper` yönergesini kullanarak görünüm bileşenini içeren derlemeyi kaydedin. Görünüm bileşeniniz `MyWebApp`adlı bir derlemede yer alıyorsa, *_ViewImports. cshtml* dosyasına aşağıdaki yönergeyi ekleyin:

```cshtml
@addTagHelper *, MyWebApp
```

Görünüm bileşenine başvuran herhangi bir dosyaya bir görünüm bileşenini etiket Yardımcısı olarak kaydedebilirsiniz. Etiket yardımcılarını kaydetme hakkında daha fazla bilgi için bkz. [etiket Yardımcısı kapsamını yönetme](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) .

Bu öğreticide kullanılan `InvokeAsync` yöntemi:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

Etiket Yardımcısı biçimlendirmesinde:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

Yukarıdaki örnekte `PriorityList` görünüm bileşeni `priority-list`hale gelir. Görünüm bileşenine yönelik parametreler, Kebab durumunda öznitelik olarak geçirilir.

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Bir görünüm bileşenini doğrudan bir denetleyiciden çağırma

Görünüm bileşenleri genellikle bir görünümden çağrılır, ancak bunları doğrudan bir denetleyici yönteminden çağırabilirsiniz. Görüntüleme bileşenleri denetleyiciler gibi uç noktaları tanımlamıyorsa, `ViewComponentResult`içeriğini döndüren bir denetleyici eylemini kolayca uygulayabilirsiniz.

Bu örnekte, görünüm bileşeni doğrudan denetleyiciden çağrılır:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>İzlenecek yol: basit bir görünüm bileşeni oluşturma

Başlatıcı kodunu [indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample), derleyin ve test edin. Bu, *Todo* öğelerinin listesini görüntüleyen `ToDo` denetleyicisi olan basit bir projem dir.

![ToDos listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>ViewComponent sınıfı ekleme

Bir *Viewcomponents* klasörü oluşturun ve aşağıdaki `PriorityListViewComponent` sınıfını ekleyin:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Koda notlar:

* Görünüm bileşen sınıfları projedeki **herhangi bir** klasörde bulunabilir.
* PriorityList**viewcomponent** sınıf adı, son ek **viewcomponent**ile sona ertiğinden, çalışma zamanı bir görünümden sınıf bileşenine başvururken "prioritylist" dizesini kullanır. Daha sonra ayrıntılı olarak açıklayacağım.
* `[ViewComponent]` özniteliği bir görünüm bileşenine başvurmak için kullanılan adı değiştirebilir. Örneğin, `XYZ` sınıfını adlandırdık ve `ViewComponent` özniteliğini uygulamış olduğumuz:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* Yukarıdaki `[ViewComponent]` özniteliği, görüntüle bileşen seçicisine bileşenle ilişkili görünümleri ararken `PriorityList` adını kullanmasını ve bir görünümden sınıf bileşenine başvururken "PriorityList" dizesini kullanmasını söyler. Daha sonra ayrıntılı olarak açıklayacağım.
* Bileşen, veri bağlamını kullanılabilir hale getirmek için [bağımlılık ekleme](../../fundamentals/dependency-injection.md) işlemini kullanır.
* `InvokeAsync`, bir görünümden çağrılabilecek bir yöntemi ortaya koyar ve bu, rastgele sayıda bağımsız değişken alabilir.
* `InvokeAsync` yöntemi `isDone` ve `maxPriority` parametrelerini karşılayan `ToDo` öğe kümesini döndürür.

### <a name="create-the-view-component-razor-view"></a>Görünüm bileşeni Razor görünümünü oluşturma

* *Görünümler/paylaşılan/bileşenler* klasörünü oluşturun. Bu klasör, adlandırılmış *Bileşenler* **olmalıdır** .

* *Görünümler/paylaşılan/bileşenler/PriorityList* klasörünü oluşturun. Bu klasör adı, görünüm bileşen sınıfının adıyla ya da sınıfın adı eksi sonek ile eşleşmelidir (Bu kural izleniyorsa ve sınıf adında *Viewcomponent* sonekini kullandıysanız). `ViewComponent` özniteliğini kullandıysanız, sınıf adının öznitelik atamasını eşleşmesi gerekir.

* Bir *Görünümler/paylaşılan/bileşenler/PriorityList/default. cshtml* Razor görünümü oluşturun:


  [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]

   Razor görünümü `TodoItem` bir listesini alır ve görüntüler. Görünüm bileşeni `InvokeAsync` yöntemi görünümün adını geçmezse (bizim örneğimizde olduğu gibi), *varsayılan* olarak kural adına göre görünüm adı kullanılır. Öğreticide daha sonra görünümün adının nasıl geçirileceğini göstereceğiz. Belirli bir denetleyicinin varsayılan stilini geçersiz kılmak için denetleyiciye özgü görünüm klasörüne bir görünüm ekleyin (örneğin, *Görünümler/Todo/Components/PriorityList/default. cshtml)* .

    Görünüm bileşeni denetleyiciye özgü ise, denetleyiciyi denetleyiciye özgü klasöre ekleyebilirsiniz (*Görünümler/Todo/bileşenler/PriorityList/default. cshtml*).

* Öncelik listesi bileşenine, *Görünümler/Todo/index. cshtml* dosyasının en altına bir çağrı içeren `div` ekleyin:

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFirst.cshtml?range=34-38)]

Biçimlendirme `@await Component.InvokeAsync`, görünüm bileşenlerini çağırma söz dizimini gösterir. İlk bağımsız değişken, çağırmak veya çağırmak istediğimiz bileşenin adıdır. Sonraki parametreler bileşene geçirilir. `InvokeAsync`, rastgele sayıda bağımsız değişken alabilir.

Uygulamayı test etme. Aşağıdaki görüntüde ToDo listesi ve öncelik öğeleri gösterilmektedir:

![Yapılacaklar listesi ve öncelik öğeleri](view-components/_static/pi.png)

Görünüm bileşenini doğrudan denetleyiciden de çağırabilirsiniz:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![ındexvc eyleminden öncelikli öğeler](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Bir görünüm adı belirtme

Karmaşık bir görünüm bileşeninin bazı koşullarda varsayılan olmayan bir görünüm belirtmesi gerekebilir. Aşağıdaki kod, `InvokeAsync` yönteminden "PVC" görünümünün nasıl ekleneceğini gösterir. `PriorityListViewComponent` sınıfındaki `InvokeAsync` yöntemini güncelleştirin.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

*Görünümleri/paylaşılan/bileşenler/prioritylist/default. cshtml* dosyasını *views/Shared/Components/PRIORITYLIST/PVC. cshtml*adlı bir görünüme kopyalayın. PVC görünümünün kullanıldığını belirtmek için bir başlık ekleyin.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Güncelleştirme *görünümleri/Todo/Index. cshtml*:

<!-- Views/ToDo/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

Uygulamayı çalıştırın ve PVC görünümünü doğrulayın.

![Öncelik görünümü bileşeni](view-components/_static/pvc.png)

PVC görünümü işlenmemişse, görünüm bileşenini 4 veya daha yüksek bir önceliğe sahip olduğunuzu doğrulayın.

### <a name="examine-the-view-path"></a>Görünüm yolunu inceleyin

* Öncelik görünümü döndürülmemesi için öncelik parametresini üç veya daha az olacak şekilde değiştirin.
* *Views/Todo/Components/PriorityList/default. cshtml* 'Yi *1default. cshtml*olarak geçici olarak yeniden adlandırın.
* Uygulamayı test edin, şu hatayı alırsınız:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Görünümleri/ *Todo/bileşenler/PriorityList/1Default. cshtml* 'yi *views/Shared/Components/Prioritylist/default. cshtml*olarak kopyalayın.
* Görünümün *paylaşılan* klasörden olduğunu göstermek için *paylaşılan* Todo görünümü bileşen görünümüne bir biçimlendirme ekleyin.
* **Paylaşılan** bileşen görünümünü test edin.

![Paylaşılan bileşen görünümü ile ToDo çıkışı](view-components/_static/shared.png)

### <a name="avoiding-hard-coded-strings"></a>Sabit kodlanmış dizelerin önleme

Derleme zamanı güvenliğini istiyorsanız, sabit kodlanmış görünüm bileşeni adını sınıf adıyla değiştirebilirsiniz. "ViewComponent" soneki olmadan görünüm bileşenini oluşturun:

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Razor görünümü dosyanıza `using` bir ifade ekleyin ve `nameof` işlecini kullanın:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a>Zaman uyumlu iş gerçekleştirin

Zaman uyumsuz iş gerçekleştirmeniz gerekmiyorsa çerçeve, zaman uyumlu `Invoke` yöntemini çağırmayı işler. Aşağıdaki yöntem, zaman uyumlu bir `Invoke` görünüm bileşeni oluşturur:

```csharp
public class PriorityList : ViewComponent
{
    public IViewComponentResult Invoke(int maxPriority, bool isDone)
    {
        var items = new List<string> { $"maxPriority: {maxPriority}", $"isDone: {isDone}" };
        return View(items);
    }
}
```

Görünüm bileşeninin Razor dosyası `Invoke` metoduna geçirilen dizeleri listeler (*Görünümler/giriş/bileşenler/PriorityList/default. cshtml*):

```cshtml
@model List<string>

<h3>Priority Items</h3>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

::: moniker range=">= aspnetcore-1.1"

Görünüm bileşeni, aşağıdaki yaklaşımlardan birini kullanarak bir Razor dosyasında (örneğin, *views/Home/Index. cshtml*) çağrılır:

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [Etiket Yardımcısı](xref:mvc/views/tag-helpers/intro)

<xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> yaklaşımını kullanmak için, `Component.InvokeAsync`çağırın:

::: moniker-end

::: moniker range="< aspnetcore-1.1"

Görünüm bileşeni, <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>ile bir Razor dosyasında çağrılır (örneğin, *views/Home/Index. cshtml*).

`Component.InvokeAsync`çağrısı:

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

Etiket Yardımcısını kullanmak için, `@addTagHelper` yönergesini kullanarak görünüm bileşenini içeren derlemeyi kaydedin (görünüm bileşeni, `MyWebApp`adlı bir derlemede bulunur):

```cshtml
@addTagHelper *, MyWebApp
```

Razor biçimlendirme dosyasında bileşen etiketini görüntüle yardımcısını kullanın:

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```

::: moniker-end

`PriorityList.Invoke` yöntemi imzası zaman uyumludur, ancak Razor, biçimlendirme dosyasında `Component.InvokeAsync` olan yöntemi bulur ve çağırır.

## <a name="all-view-component-parameters-are-required"></a>Tüm görünüm bileşeni parametreleri gereklidir

Bir görünüm bileşenindeki her bir parametre gerekli bir özniteliktir. [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore/issues/5011)bakın. Herhangi bir parametre atlanırsa:

* `InvokeAsync` yöntemi imzası eşleşmez, bu nedenle Yöntem yürütülmez.
* ViewComponent hiçbir biçimlendirmeyi işlemez.
* Hata oluşturulmayacak.

## <a name="additional-resources"></a>Ek kaynaklar

* [Görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection)
