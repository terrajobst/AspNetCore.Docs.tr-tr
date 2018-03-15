---
title: "ASP.NET Core görünümü bileşenler"
author: rick-anderson
description: "Görünüm bileşenleri ASP.NET Core nasıl kullanıldığı ve uygulamaları için eklemeyi öğrenin."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: 95b68e1747296310967b7093bb7019005b92fcd7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="view-components-in-aspnet-core"></a>ASP.NET Core görünümü bileşenler

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="introducing-view-components"></a>Görünüm bileşenleri Tanıtımı

Yeni ASP.NET Core MVC için Görünüm bileşenleri için kısmi görünümler benzerdir, ancak çok daha güçlü. Görünüm bileşenleri yoksa model bağlama kullanın ve yalnızca içine çağrılırken sağlanan verileri bağlıdır. Bir görünümü bileşen:

* Yanıtın tamamını yerine bir öbek işler.
* Bir denetleyici ve görünüm arasında bulunan Test Edilebilirlik avantajları ve aynı ayrımı-in-ile ilgili sorunlar içerir.
* Parametreleri ve iş mantığı sahip olabilir.
* Tipik bir düzen sayfasından çağrılır.

Görünüm bileşenleri herhangi bir yere kısmi görünüm için çok karmaşık olduğu gibi yeniden kullanılabilir işleme mantığı sahip tasarlanmıştır:

* Dinamik Gezinti menüleri
* (Burada veritabanını sorgular) Etiket Bulutu
* Oturum açma paneli
* Alışveriş sepeti
* Son zamanlarda yayımlanan makaleleri
* Tipik bir blog kenar içerik
* Her sayfada işlenir ve oturum kapatma veya bağlı durumda olan kullanıcının günlük olarak oturum açma ya da bağlantılarını göster bir oturum açma paneli

Bir görünümü bileşen iki bölümden oluşur: sınıfı (genellikle türetilmiş [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) ve isteğe bağlı olarak sonucu (genellikle bir görünüm) döndürür. Denetleyicileri gibi bir POCO görünümü bileşen olabilir, ancak çoğu Geliştirici türetme tarafından kullanılabilen özellikleri ve yöntemleri yararlanmak istersiniz `ViewComponent`.

## <a name="creating-a-view-component"></a>Bir görünüm bileşeni oluşturma

Bu bölümde bir görünümü bileşen oluşturmak için üst düzey gereksinimleri bulunur. Makalenin sonraki bölümlerinde, biz her adım ayrıntılı inceleyin ve bir görünüm bileşeni oluşturma.

### <a name="the-view-component-class"></a>Görünüm bileşen sınıfı

Bir görünümü bileşen sınıfı aşağıdakilerden herhangi biri tarafından oluşturulabilir:

* Türetme *ViewComponent*
* Bir sınıfla dekorasyon `[ViewComponent]` özniteliği ya da bir sınıf türetme `[ViewComponent]` özniteliği
* Bir sınıf adı sonekiyle sona ereceği oluşturma *ViewComponent*

Denetleyicileri gibi görünüm bileşenleri genel, iç içe olmayan ve Özet olmayan sınıflar olması gerekir. Görünüm bileşen adı kaldırıldı "ViewComponent" soneki ile sınıfı adıdır. Bu aynı zamanda açıkça kullanılarak belirtilebilir `ViewComponentAttribute.Name` özelliği.

Bir görünüm bileşenin sınıfı:

* Tam olarak Oluşturucusu destekleyen [bağımlılık ekleme](../../fundamentals/dependency-injection.md)

* Bölümü kullanamazsınız anlamına gelir denetleyicisi çevriminin almaz [filtreleri](../controllers/filters.md) bir görünüm bileşeninde

### <a name="view-component-methods"></a>Görünüm bileşen yöntemleri

Bir görünüm bileşeni, mantığını tanımlayan bir `InvokeAsync` döndüren yöntemi bir `IViewComponentResult`. Parametreleri doğrudan çağırma görünüm bileşeninin değil, model bağlama gelmektedir. Bir görünüm bileşeni hiçbir zaman doğrudan bir isteği işler. Genellikle, bir görünüm bileşeni bir model başlatır ve çağırarak bir görünümüne geçirir `View` yöntemi. Özet olarak, bileşen yöntemlerini görüntüleyin:

* Tanımlayan bir `InvokeAsync` döndürür yöntemi bir `IViewComponentResult`
* Genellikle bir model başlatır ve çağırarak bir görünümüne geçirir `ViewComponent` `View` yöntemi
* Arama yöntemi, değil HTTP parametreleri gelir, model bağlama yok
* Olan bir HTTP uç noktası olarak doğrudan ulaşılabilir değil, bunlar (genellikle bir görünümde) kodunuzdan çağrılan. Bir görünüm bileşeni hiçbir zaman bir isteği işler
* Geçerli HTTP isteği herhangi bir ayrıntıyı yerine imza aşırı

### <a name="view-search-path"></a>Görünüm arama yolu

Aşağıdaki yolları görünümünde çalışma zamanı arar:

   * Görünümler /\<controller_name > /Components/\<view_component_name > /\<view_name >
   * Görünümler/paylaşılan/bileşenleri/\<view_component_name > /\<view_name >

Bir görünüm bileşeni için varsayılan görünüm adı *varsayılan*, görünüm dosyanızı başka bir deyişle, tipik olarak adlandırılacaktır *Default.cshtml*. Görünüm bileşeni sonuç oluştururken veya çağrılırken farklı görünüm adı belirtebilirsiniz `View` yöntemi.

Görünüm dosyası adı öneririz *Default.cshtml* ve *görünümler/paylaşılan/bileşenleri/\<view_component_name > /\<view_name >* yolu. `PriorityList` Bu örnekte kullanılan görünümü bileşen *Views/Shared/Components/PriorityList/Default.cshtml* görünümünü bileşeni için.

## <a name="invoking-a-view-component"></a>Bir görünümü bileşen çağırma

Görünümü bileşen kullanmak için aşağıdaki çağrı görünümü içinde:

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Parametreleri geçirilecek `InvokeAsync` yöntemi. `PriorityList` Makalesinde geliştirilen görünümü bileşen çağrılan *Views/Todo/Index.cshtml* görünüm dosyası. Aşağıdaki `InvokeAsync` yöntemi iki parametre ile çağrılır:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Bir görünümü bileşen etiket Yardımcısı olarak çağırma

ASP.NET Core 1.1 ve üzeri, bir görünüm bileşeni olarak çağırabileceği bir [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Pascal ortası sınıfı ve yöntem parametreleri etiket Yardımcıları için çevrilen içine kendi [alt kebab durumda](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Bir görünümü bileşen çağrılacak etiket Yardımcısı kullanan `<vc></vc>` öğesi. Görünüm bileşeni gibi belirtilir:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Not: bir görünüm bileşeni etiket Yardımcısı olarak kullanmak için Görünüm bileşenini kullanarak içeren derlemenin kaydetmeniz gerekir `@addTagHelper` yönergesi. Örneğin, görünüm bileşeniniz "Mywebapp şeklindedir" adlı bir derleme varsa, aşağıdaki yönergesi ekleyin. `_ViewImports.cshtml` dosyası:

```cshtml
@addTagHelper *, MyWebApp
```

Bir görünüm bileşeni görünümü bileşen başvuran herhangi bir dosyaya etiketi yardımcı olarak kaydedebilirsiniz. Bkz: [yönetme etiket Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) etiket Yardımcıları kaydetme hakkında daha fazla bilgi için.

`InvokeAsync` Bu öğreticide kullanılan yöntem:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Etiket Yardımcısı biçimlendirmede:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Yukarıdaki örnekteki `PriorityList` görünümü bileşen olur `priority-list`. Görünümü bileşen parametreleri alt kebab durumda öznitelik olarak geçirilir.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Bir görünümü bileşen denetleyicisinden doğrudan çağırma

Görünümü bileşenler genellikle bir görünümden çağrılır, ancak doğrudan denetleyici yönteminden çağırabilirsiniz. Görünüm bileşenleri denetleyicileri gibi uç noktaları tanımlamak yoktur, ancak içeriği döndüren bir denetleyici eylemi kolayca uygulayabilirsiniz bir `ViewComponentResult`.

Bu örnekte, görünümü bileşen doğrudan denetleyicisinden çağrılır:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>İzlenecek yol: basit bir görünümle bileşeni oluşturma

[Karşıdan](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), yapı ve test starter kodunun. Basit bir projeyle olan bir `Todo` listesini görüntüler denetleyicisi *Yapılacaklar* öğeleri.

![ToDos listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>ViewComponent sınıfı ekleme

Oluşturma bir *ViewComponents* klasörü ve aşağıdaki ekleyin `PriorityListViewComponent` sınıfı:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Kodu Notlar:

* Görünümü bileşen sınıfları yer almalıdır içinde **herhangi** proje klasöründe.
* Sınıf adı olduğundan PriorityList**ViewComponent** sonekiyle sona erer **ViewComponent**, çalışma zamanı sınıf bileşen bir görünümden başvururken dizesi "PriorityList" kullanır. I, daha ayrıntılı olarak daha sonra açıklanmıştır.
* `[ViewComponent]` Öznitelik görünümü bileşen başvurmak için kullanılan adını değiştirebilir. Örneğin, biz sınıfı adlı `XYZ` ve uygulanan `ViewComponent` özniteliği:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]` Yukarıda öznitelik adı kullanmak için görünümü bileşen Seçicisi söyler `PriorityList` bileşeni ile ve sınıf bileşen bir görünümden başvururken dizesi "PriorityList" kullanmak için ilişkili görünümleri ararken. I, daha ayrıntılı olarak daha sonra açıklanmıştır.
* Bileşen [bağımlılık ekleme](../../fundamentals/dependency-injection.md) veri bağlamı kullanılabilmesi için.
* `InvokeAsync` çıkarır rastgele sayıda bağımsız değişken bir görünüm ve onu denilen bir yöntem alabilir.
* `InvokeAsync` Yöntemi döndürür kümesini `ToDo` karşılamak öğeleri `isDone` ve `maxPriority` parametreleri.

### <a name="create-the-view-component-razor-view"></a>Görünümü bileşen Razor görünümü oluşturma

* Oluşturma *görünümler/paylaşılan/bileşenleri* klasör. Bu klasör **gerekir** adlı *bileşenleri*.

* Oluşturma *görünümler/paylaşılan/bileşenleri/PriorityList* klasör. Bu klasör adı görünümü bileşen sınıfı adını ya da soneki eksi sınıfın adı eşleşmelidir (kuralı izleyen ve kullandıysanız *ViewComponent* sınıf adı soneki). Kullandıysanız `ViewComponent` özniteliği, sınıf adını öznitelik ataması eşleşmesi gerekir.

* Oluşturma bir *Views/Shared/Components/PriorityList/Default.cshtml* Razor görünümü: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Razor görünüm bir listesini alır `TodoItem` ve bunları görüntüler. Varsa görünümü bileşen `InvokeAsync` yöntemi (olduğu gibi bizim örnek), görünümün adını geçirmek değil *varsayılan* Görünüm adı için kural tarafından kullanılır. Öğreticide daha sonra t, görünümün adını geçirmek nasıl göstereceğiz. Belirli bir denetleyicinin varsayılan stil geçersiz kılmak için denetleyici özel görünüm klasöre bir görünüm ekleyin (örneğin *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Görünüm Bileşen Denetleyicisi özgü ise, denetleyici özgü klasörüne ekleyebilirsiniz (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Ekleme bir `div` altına öncelik liste bileşeni için bir çağrı içeren *Views/Todo/index.cshtml* dosyası:

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

İşaretleme `@await Component.InvokeAsync` görünüm bileşenleri çağırma söz dizimi görülmektedir. İlk bağımsız değişken çağrılamadı veya çağrı istiyoruz bileşen adıdır. Sonraki parametreler bileşenine aktarılır. `InvokeAsync` rastgele sayıda bağımsız değişken alabilir.

Uygulamayı test etme. Aşağıdaki resimde, yapılacaklar listesi ve öncelik öğeleri gösterilmektedir:

![Yapılacaklar listesi ve öncelik öğeleri](view-components/_static/pi.png)

Ayrıca, doğrudan denetleyicisinden görünümü bileşen çağırabilirsiniz:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC eylemden öncelikli öğeler](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Bir görünüm adı belirtme

Karmaşık görünümü bileşen, varsayılan olmayan görünüm bazı koşullar altında belirtmeniz gerekebilir. Aşağıdaki kod "PVC" görünümünden belirtme gösterir `InvokeAsync` yöntemi. Güncelleştirme `InvokeAsync` yönteminde `PriorityListViewComponent` sınıfı.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Kopya *Views/Shared/Components/PriorityList/Default.cshtml* adlı bir görünümü dosyasına *Views/Shared/Components/PriorityList/PVC.cshtml*. PVC görünümü kullanılan belirtmek için bir başlık ekleyin.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Güncelleştirme *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Uygulamayı çalıştırın ve PVC görünüm doğrulayın.

![Öncelik görünümü bileşen](view-components/_static/pvc.png)

PVC görünüm işlenen değil, 4 veya daha yüksek önceliğe sahip görünümü bileşen aradığınız doğrulayın.

### <a name="examine-the-view-path"></a>Görünüm yolu inceleyin

* Priority parametresi, üç veya daha az öncelik görünüm döndürülmezse şekilde değiştirin.
* Geçici olarak yeniden adlandırın *Views/Todo/Components/PriorityList/Default.cshtml* için *1Default.cshtml*.
* Uygulamayı test etme, aşağıdaki hatayı alırsınız:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Kopya *Views/Todo/Components/PriorityList/1Default.cshtml* için *Views/Shared/Components/PriorityList/Default.cshtml*.
* Bazı biçimlendirmeleri eklemek *paylaşılan* görünümü belirtmek için yapılacaklar bileşen görünümünü geldiği *paylaşılan* klasör.
* Test **paylaşılan** bileşeni görünümü.

![Paylaşılan bileşeni görünümü Yapılacaklar çıkışı](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Sihirli dizeleri önleme

Zaman güvenliği derleme istiyorsanız, sabit kodlanmış görünümü bileşen adı sınıf adıyla değiştirin. "ViewComponent" soneki olmayan görünümü bileşen oluşturun:

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Ekleme bir `using` , Razor ifadesine dosya görüntülemek ve kullanmak `nameof` işleci:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection)
