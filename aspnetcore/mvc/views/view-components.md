---
title: ASP.NET core'da görünüm bileşenleri
author: rick-anderson
description: ASP.NET Core görünümü bileşenlerin nasıl kullanıldığı ve bunları için uygulamaları nasıl ekleyeceğinizi öğrenin.
ms.author: riande
ms.date: 1/30/2019
uid: mvc/views/view-components
ms.openlocfilehash: d979c9480f7bffff993f0ea526bdc231b940baa2
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410488"
---
# <a name="view-components-in-aspnet-core"></a>ASP.NET core'da görünüm bileşenleri

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="view-components"></a>Görünüm bileşenleri

Kısmi görünüm için Görünüm bileşenleri benzerdir, ancak bunlar çok daha güçlü. Görünüm bileşenleri kullanmayın model bağlama ve içine çağırırken sağlanan verileri yalnızca bağlıdır. Denetleyicileri ve görünümleri kullanarak bu makalenin yazıldığı ancak görünüm bileşenleri de Razor sayfaları kullanmaya çalışır.

Bir görünümü bileşeni:

* Bir yanıtın tamamını yerine bir öbek işler.
* Test Edilebilirlik avantajları bir denetleyici ve görünüm arasında bulunan ve aynı ayrımı-ın-ile ilgili sorunlar içerir.
* Parametreleri ve iş mantığına sahip olabilir.
* Genellikle bir düzen sayfasından çağrılır.

Görünüm bileşenleri herhangi bir kısmi görünüm için çok karmaşık olduğu gibi yeniden kullanılabilir işleme mantığı sahip tasarlanmıştır:

* Dinamik Gezinti menüleri
* Etiket bulutunu (burada veritabanını sorgular)
* Oturum açma paneli
* Alışveriş sepeti
* Yakın zamanda yayımlanan makaleler
* Tipik bir blog kenar çubuğu içeriği
* Bir oturum açma paneli her sayfada işlenir ve oturumu kapatmayın veya kullanıcının durumunu günlüğünde bağlı olarak, oturum için ya da bağlantılarını göster

Bileşeni görüntüle iki bölümden oluşur: sınıfı (genellikle türetilen [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) ve isteğe bağlı olarak sonucu (genellikle bir görünüm) döndürür. Denetleyicileri gibi bir POCO görünümü bileşen olabilir, ancak çoğu Geliştirici türetme tarafından kullanılabilen özellikler ve yöntemler yararlanmak isteyeceksiniz `ViewComponent`.

## <a name="creating-a-view-component"></a>Bir görünümü bileşeni oluşturma

Bu bölümde, bir görünüm bileşeni oluşturmak için en üst düzey gereksinimleri bulunur. Makalenin sonraki bölümlerinde size her adım ayrıntılı inceleyin ve bir görünüm bileşeni oluşturmak.

### <a name="the-view-component-class"></a>Görünümü bileşen sınıfı

Bir görünümü bileşen sınıfı aşağıdakilerden birini oluşturulabilir:

* Öğesinden türetme *ViewComponent*
* Bir sınıf ile dekorasyon `[ViewComponent]` özniteliği veya bir sınıf türetmek `[ViewComponent]` özniteliği
* Bir sınıf adı soneki ile sona ereceği oluşturma *ViewComponent*

Denetleyicileri gibi görünüm bileşenleri, genel, iç içe olmayan ve soyut olmayan sınıflar olması gerekir. Görünümü bileşen adı kaldırıldı "ViewComponent" sonekine sahip sınıf adıdır. Onu da açıkça kullanılarak belirtilebilir `ViewComponentAttribute.Name` özelliği.

Bir görünümü bileşen sınıfı:

* Oluşturucu tam olarak destekler [bağımlılık ekleme](../../fundamentals/dependency-injection.md)

* Bölümü kullanamazsınız yani denetleyici yaşam döngüsünde almaz [filtreleri](../controllers/filters.md) görünümü bileşeninde

### <a name="view-component-methods"></a>Görünümü bileşen yöntemleri

Görünümü bileşen kendi mantığı tanımlayan bir `InvokeAsync` döndüren yöntem bir `Task<IViewComponentResult>` veya bir zaman uyumlu olarak `Invoke` döndüren yöntem bir `IViewComponentResult`. Parametreler doğrudan çağrı görünümü bileşeninin değil, model bağlama gelir. Bir görünümü bileşeni hiçbir zaman doğrudan bir isteği işler. Genellikle, bir görünüm bileşeni bir model başlatır ve çağırarak görünümüne geçirir `View` yöntemi. Özet olarak, bileşen yöntemleri görüntüleyin:

* Tanımlayan bir `InvokeAsync` döndüren yöntem bir `Task<IViewComponentResult>` veya bir zaman uyumlu `Invoke` döndüren yöntem bir `IViewComponentResult`.
* Genellikle bir model başlatır ve çağırarak görünümüne geçirir `ViewComponent` `View` yöntemi.
* Parametreleri HTTP değil çağırma yönteminden gelir. Hiçbir model bağlama yoktur.
* Doğrudan HTTP uç noktası ulaşılamıyor. Bunlar kodunuzu (genellikle, bir görünüm) çağrılır. Bir görünümü bileşeni hiçbir zaman bir isteği işler.
* Geçerli HTTP istek tüm ayrıntıları yerine imza aşırı yüklenmesi sebebiyledir.

### <a name="view-search-path"></a>Görünüm arama yolu

Çalışma zamanı aşağıdaki yollardan görünümünde arar:

* {Denetleyici adı} /Views/ /Components/ {görünümü bileşen adı} / {View adı}
* / Görünümler/paylaşılan/bileşenleri / {View bileşen adı} / {View adı}
* / Sayfaları/paylaşılan/bileşenleri / {View bileşen adı} / {View adı}

Arama yolu, denetleyicileri + görünümleri ve Razor sayfaları kullanan projeler için geçerlidir.

Bir görünümü bileşen için varsayılan görünüm adı *varsayılan*, yani dosyasını görüntüle genellikle adlandırılacağını *Default.cshtml*. Görünüm bileşen sonucu oluştururken veya çağırırken farklı görünüm adı belirtebilirsiniz `View` yöntemi.

Görünüm dosyası adı öneririz *Default.cshtml* ve *görünümler/paylaşılan/Components / {View bileşen adı} / {View Name}* yolu. `PriorityList` Bu örnekte kullanılan görünümü bileşen *Views/Shared/Components/PriorityList/Default.cshtml* görünümünü bileşeni için.

## <a name="invoking-a-view-component"></a>Bileşeni görüntüle çağırma

Bileşeni görüntüle kullanmak için aşağıdaki çağrı görünümü içinde:

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

Parametreleri geçirilecek `InvokeAsync` yöntemi. `PriorityList` Makalesinde geliştirilen görünümü bileşen ınvoked from *Views/ToDo/Index.cshtml* görünüm dosyası. Aşağıdaki `InvokeAsync` yöntemi, iki parametre ile çağrılır:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Bileşeni görüntüle etiket Yardımcısı çağırma

ASP.NET Core 1.1 ve sonraki bir görünüm bileşeni olarak çağırabilirsiniz bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

Pascal büyük küçük harfleri sınıf ve yöntem parametreleri etiket Yardımcıları için çevrilmiş halinde kullanıcıların [kebab çalışması](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Bileşeni görüntüle çağırmak için etiket Yardımcısı kullanan `<vc></vc>` öğesi. Bileşeni görüntüle gibi belirtilir:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Bir görünümü bileşeni etiket Yardımcısı kullanılacak görünümünü kullanarak bileşeni içeren derlemenin kaydetme `@addTagHelper` yönergesi. Bileşeni görüntüle adlı bir derlemede ise `MyWebApp`, eklemek için aşağıdaki yönerge *_viewımports.cshtml* dosyası:

```cshtml
@addTagHelper *, MyWebApp
```

Bileşeni görüntüle görünümü bileşenine başvurduğunu herhangi bir dosyaya etiket Yardımcısı olarak kaydedebilirsiniz. Bkz: [yönetme etiket Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) etiket Yardımcıları kaydetme hakkında daha fazla bilgi için.

`InvokeAsync` Bu öğreticide kullanılan yöntemi:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

Etiket Yardımcısı biçimlendirme içinde:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

Yukarıdaki örnekteki `PriorityList` görünümü bileşen olur `priority-list`. Bileşeni görüntüle parametreleri, öznitelikleri kebab durumda olarak geçirilir.

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Bir görünümü bileşen denetleyicisinden doğrudan çağırma

Görünüm bileşenleri, genellikle bir görünümden çağrılır, ancak doğrudan bir denetleyici yöntemi çağırabilirsiniz. Görünüm bileşenleri denetleyicileri gibi uç noktaları tanımlamak yoktur, ancak içeriği döndüren bir denetleyici eylemi kolayca uygulayabilirsiniz bir `ViewComponentResult`.

Bu örnekte, doğrudan denetleyiciden görünüm bileşen çağrılır:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>İzlenecek yol: Basit Görünüm bileşeni oluşturma

[İndirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), derleme ve Başlatıcı kodu test edin. Basit bir proje olan bir `ToDo` listesini görüntüler denetleyicisi *ToDo* öğeleri.

![Açıklamada listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>ViewComponent sınıfı Ekle

Oluşturma bir *ViewComponents* klasörü ve aşağıdaki `PriorityListViewComponent` sınıfı:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Kod ile ilgili notlar:

* Görünüm bileşen sınıfları kapsanıyorsa **herhangi** proje klasöründe.
* Sınıf adı olduğundan PriorityList**ViewComponent** soneki ile sona erer **ViewComponent**, çalışma zamanı sınıf bileşen görünümden başvururken "PriorityList" dizesini kullanır. I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.
* `[ViewComponent]` Öznitelik, bir görünüm bileşeni başvurmak için kullanılan adını değiştirebilirsiniz. Örneğin, biz adlı sınıfı `XYZ` ve uygulanan `ViewComponent` özniteliği:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]` Özniteliği yukarıdaki söyler adı kullanacak şekilde görünümü Bileşen Seçici `PriorityList` bileşeni ile ve "PriorityList" dize sınıfı bileşen görünümden başvururken kullanılacak ilişkili görünümleri ararken. I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.
* Bileşen [bağımlılık ekleme](../../fundamentals/dependency-injection.md) veri bağlamı kullanılabilir hale getirmek için.
* `InvokeAsync` kullanıma sunan bir görünümü ve onu çağrılabilen bir yöntem rastgele bir sayıda bağımsız değişken alabilir.
* `InvokeAsync` Yöntemi kümesini döndürür `ToDo` karşılayan öğelerinin `isDone` ve `maxPriority` parametreleri.

### <a name="create-the-view-component-razor-view"></a>Bileşen Razor görünümünü oluşturma

* Oluşturma *görünümler/paylaşılan/Components* klasör. Bu klasör **gerekir** adı *bileşenleri*.

* Oluşturma *görünümler/paylaşılan/bileşenleri/PriorityList* klasör. Bu klasör adı görünümü bileşen sınıfının adı ve soneki eksi sınıfının adı eşleşmelidir (kuralını izleyen ve kullandıysanız *ViewComponent* sınıf adı soneki). Kullandıysanız `ViewComponent` özniteliği, sınıf adı öznitelik atamasını eşleşmesi gerekir.

* Oluşturma bir *Views/Shared/Components/PriorityList/Default.cshtml* Razor Görünüm: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]

   Razor görünümü listesini alır `TodoItem` ve bunları görüntüler. Varsa görünümü bileşen `InvokeAsync` yöntemi (olduğu gibi örneğimizi), görünüm adını geçirin değil *varsayılan* görünümü adı için kural olarak kullanılır. Öğreticinin sonraki bölümlerinde miyim, görünümün adının ona nasıl iletileceğini göstereceğiz. Belirli bir denetleyicinin varsayılan stillerini geçersiz kılmak için bir görünüm denetleyicisine özgü görünüm klasöre ekleyin (örneğin *Views/ToDo/Components/PriorityList/Default.cshtml)*.

    Denetleyicisine özgü görünüm bileşeni ise denetleyicisi özgü klasöre ekleyebilirsiniz (*Views/ToDo/Components/PriorityList/Default.cshtml*).

* Ekleme bir `div` altına öncelik listesi bileşeni için bir çağrı içeren *Views/ToDo/index.cshtml* dosyası:

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFirst.cshtml?range=34-38)]

Biçimlendirme `@await Component.InvokeAsync` görünüm bileşenleri çağırma söz dizimi görülmektedir. İlk bağımsız değişken çağırma veya çağrı istiyoruz bileşen adıdır. Sonraki parametreler bileşenine aktarılır. `InvokeAsync` rastgele bir sayıda bağımsız değişken alabilir.

Uygulamayı test etme. Aşağıdaki görüntüde, yapılacaklar listesi ve öncelikli öğeleri gösterir:

![Yapılacaklar listesi ve öncelikli öğeleri](view-components/_static/pi.png)

Doğrudan denetleyiciden görünüm bileşeni de çağırabilirsiniz:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![öncelikli öğeleri IndexVC eylemi](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Görünüm adı belirtme

Karmaşık görünümü bileşen, bazı koşullar altında bir varsayılan olmayan görünüm belirtmeniz gerekebilir. Aşağıdaki kod "PVC" görünümünden belirteceğiniz gösterilmektedir `InvokeAsync` yöntemi. Güncelleştirme `InvokeAsync` yönteminde `PriorityListViewComponent` sınıfı.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Kopyalama *Views/Shared/Components/PriorityList/Default.cshtml* adlı bir görünüm dosyasına *Views/Shared/Components/PriorityList/PVC.cshtml*. PVC görünümü kullanıldığını belirtmek için bir başlık ekleyin.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Güncelleştirme *Views/ToDo/Index.cshtml*:

<!-- Views/ToDo/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

Uygulamayı çalıştırın ve PVC görünümü doğrulayın.

![Öncelik bileşeni görüntüle](view-components/_static/pvc.png)

PVC görünüm işlenen değil, 4 veya daha yüksek bir önceliğe sahip görünümü bileşen aradığınız doğrulayın.

### <a name="examine-the-view-path"></a>Görünüm yolunu inceleyin

* Öncelik parametresi, üç veya daha düşük öncelikli görünüm döndürülmez şekilde değiştirin.
* Geçici olarak yeniden adlandırın *Views/ToDo/Components/PriorityList/Default.cshtml* için *1Default.cshtml*.
* Uygulamayı test etme, aşağıdaki hatayı alırsınız:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Kopyalama *Views/ToDo/Components/PriorityList/1Default.cshtml* için *Views/Shared/Components/PriorityList/Default.cshtml*.
* Bazı biçimlendirme eklemek *paylaşılan* ToDo görünümü bileşen görünümü, görünümün belirtmenizi geldiği *paylaşılan* klasör.
* Test **paylaşılan** bileşeni görünümü.

![ToDo çıkış paylaşılan bileşen görünümü](view-components/_static/shared.png)

### <a name="avoiding-hard-coded-strings"></a>Sabit kodlanmış dizeleri kaçınma

Zaman güvenlik derlemek isterseniz, sabit kodlanmış görünümü bileşen adı sınıf adını değiştirebilirsiniz. Bileşeni görüntüle "ViewComponent" soneki olmadan oluşturun:

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Ekleme bir `using` , Razor ifadesine dosyayı görüntüle ve Kullan `nameof` işleci:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a>Zaman uyumlu çalışma gerçekleştirme

Çerçeve işleme bir zaman uyumlu çağırma `Invoke` zaman uyumsuz çalışmayı gerçekleştirmek gerekmiyorsa yöntemi. Aşağıdaki yöntem zaman uyumlu bir oluşturur `Invoke` bileşeni görüntüle:

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

Geçirilen dizeler görünümü bileşenin Razor dosyasını listeler `Invoke` yöntemi (*Views/Home/Components/PriorityList/Default.cshtml*):

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

Bileşeni görüntüle Razor dosyasında çağrılır (örneğin, *Views/Home/Index.cshtml*) aşağıdaki yaklaşımlardan birini kullanarak:

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [Etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro)

Kullanılacak <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> yaklaşımı, çağrı `Component.InvokeAsync`:

::: moniker-end

::: moniker range="< aspnetcore-1.1"

Bileşeni görüntüle Razor dosyasında çağrılır (örneğin, *Views/Home/Index.cshtml*) ile <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.

Çağrı `Component.InvokeAsync`:

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

Etiket Yardımcısı'nı kullanmak için görünümü bileşen kullanarak içeren derlemenin kaydetme `@addTagHelper` yönergesi (adlı bir derlemede görünümü bileşendir `MyWebApp`):

```cshtml
@addTagHelper *, MyWebApp
```

Etiket Yardımcısı görünümü bileşen Razor biçimlendirme dosyasında kullanın:

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```
::: moniker-end

Yöntem imzası `PriorityList.Invoke` zaman uyumludur, ancak Razor bulur ve içeren yöntemi çağıran `Component.InvokeAsync` biçimlendirme dosyası.

## <a name="additional-resources"></a>Ek kaynaklar

* [Görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection)
