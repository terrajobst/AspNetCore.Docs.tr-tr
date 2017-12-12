---
title: "Yerel mobil uygulamalar için arka uç hizmetleri oluşturma"
author: ardalis
description: 
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: be1cd9f4fe41f1a79669975cb6a89439cdd9e5c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>Yerel mobil uygulamalar için arka uç hizmetleri oluşturma

Tarafından [Steve Smith](https://ardalis.com/)

Mobil uygulamaları kolayca ASP.NET Core arka uç hizmetleriyle iletişim kurabilir.

[Görüntülemek veya karşıdan örnek arka uç hizmetlerini kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Örnek yerel mobil uygulama

Bu öğretici, yerel mobil uygulamalar desteklemek için ASP.NET Core MVC kullanarak arka uç hizmetlerini oluşturulacağını gösterir. Kullandığı [Xamarin Forms ToDoRest uygulama](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) kendi yerel istemci olarak Android, iOS, Windows evrensel ve Windows Phone cihazları için ayrı yerel istemcilerde içerir. Sizin yerel uygulama oluşturma (ve gerekli boş Xamarin Araçları'nı yüklemek için) bağlı öğreticisini izleyin yanı Xamarin örnek çözümü indirin. Xamarin örnek bu makalenin ASP.NET Core uygulama (istemci tarafından gerekli değişiklik yok) yerine bir ASP.NET Web API 2 services projesi içerir.

![Bir Android smartphone üzerinde çalışan Rest yapmak uygulama](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Özellikler

ToDoRest uygulama listeleme, ekleme, silme ve yapılacaklar öğelerini güncelleştirme destekler. Her bir öğe kimliği, bir ad, notlar ve henüz girildiği olup olmadığını gösteren bir özellik vardır.

Ana görünümü öğeleri, yukarıda gösterildiği gibi her öğenin adını listeler ve işareti ile yapılır, gösterir.

Dokunarak `+` simgesi Ekle öğesi iletişim açar:

![Öğe iletişim ekleyin](native-mobile-backend/_static/todo-android-new-item.png)

Öğenin ana listesi ekranında dokunarak burada öğenin adı, notlar ve Bitti'yi ayarları değiştirilebilir veya öğenin silinebilmesi bir düzenleme iletişim kutusunu açar:

![Öğesi iletişim Düzenle](native-mobile-backend/_static/todo-android-edit-item.png)

Bu örnek, varsayılan olarak developer.xamarin.com barındırılan salt okunur işlemlere izin arka uç hizmetlerine kullanacak şekilde yapılandırılır. Bilgisayarınızda çalışan sonraki bölümde oluşturulan ASP.NET Core uygulaması karşı kendiniz sınamak için uygulamanın güncelleştirme gerekecektir `RestUrl` sabit. Gidin `ToDoREST` proje ve açık *Constants.cs* dosya. Değiştir `RestUrl` makinenizin IP içeren bir URL adresini (localhost veya 127.0.0.1, bu adres, makineden aygıt öykücüsünden kullanıldığından değil). Bağlantı noktası numarasını da (5000) içerir. Hizmetlerinizin bir cihazla çalışmayı test etmek için bu bağlantı noktası erişimi engelleme etkin bir güvenlik duvarı olmadığından emin olun.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>ASP.NET Core projesi oluşturma

Visual Studio'da yeni bir ASP.NET çekirdek Web uygulaması oluşturun. Hayır kimlik doğrulaması ve Web API şablonu seçin. Proje adı *ToDoApi*.

![Seçilen Web API projesi şablonuyla yeni ASP.NET Web uygulaması iletişim kutusu](native-mobile-backend/_static/web-api-template.png)

Uygulama bağlantı noktası 5000 yapılan tüm isteklere yanıt. Güncelleştirme *Program.cs* içerecek şekilde `.UseUrls("http://*:5000")` Bunu başarmak için:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> IIS, varsayılan olarak yerel olmayan isteklerini yoksayar Express, arkasında yapmak yerine, doğrudan, uygulama çalıştırdığınızdan emin olun. Çalıştırma `dotnet run` bir komut isteminden veya Visual Studio araç çubuğundaki hata ayıklama hedefi açılır uygulama adı profili seçin.

Yapılacaklar öğelerini göstermek için bir model sınıfı ekleyin. İşareti gerekli alanlarını kullanarak `[Required]` özniteliği:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

API yöntemlerini verilerle çalışmak için bazı yol gerekir. Aynı `IToDoRepository` arabirim özgün Xamarin örnek kullanır:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Bu örnek için uygulama, yalnızca öğeleri özel koleksiyonu kullanır:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Uygulamasında yapılandırma *haline*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

Bu noktada, oluşturmak için hazır *ToDoItemsController*.

> [!TIP]
> Oluşturma hakkında daha fazla web API'leri öğrenin [yapı bilgisayarınızı ilk Web API ile ASP.NET Core MVC ve Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Denetleyici oluşturma

Yeni bir denetleyici projeye ekleyin *ToDoItemsController*. Microsoft.AspNetCore.Mvc.Controller devralan. Ekleme bir `Route` denetleyicisi ile başlayan yollar için yapılan istekleri işlemesini belirtmek için öznitelik `api/todoitems`. `[controller]` Belirteci rotadaki denetleyici adıyla değiştirilir (atlama `Controller` soneki) ve genel yollar için özellikle yararlıdır. Daha fazla bilgi edinmek [yönlendirme](../fundamentals/routing.md).

Denetleyici gerektiren bir `IToDoRepository` için işlev; denetleyicinin Oluşturucusu aracılığıyla bu türünün bir örneği isteği. Framework'ün desteğini kullanarak bu örnek çalışma zamanında sağlanacak [bağımlılık ekleme](../fundamentals/dependency-injection.md).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Bu API veri kaynağında (oluşturma, okuma, güncelleştirme, silme) CRUD işlemleri gerçekleştirmek için dört farklı HTTP fiilleri destekler. Bu bir HTTP GET isteği karşılık gelen okuma işlemi en kolayıdır.

### <a name="reading-items"></a>Öğeleri okuma

Bir öğe listesi isteyen bir GET isteğine ile yapılır `List` yöntemi. `[HttpGet]` Özniteliği `List` yöntemi gösterir Bu eylem yalnızca GET isteklerini işlemesi gerekir. Bu eylem için yol denetleyicisinde belirtilen yoldur. Mutlaka rota bir parçası olarak eylem adı kullanmanız gerekmez. Yalnızca benzersiz ve anlaşılır bir rota her bir eylem içerdiğinden emin olmak yeterlidir. Yönlendirme öznitelikleri, denetleyici ve belirli rotaları oluşturmak için yöntem düzeylerinde uygulanabilir.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List` Yöntemi 200 Tamam yanıt kodu ve tüm JSON olarak serileştirilen Yapılacaklar öğelerini döndürür.

Yeni API yönteminizi gibi çeşitli araçları, kullanarak test edebilirsiniz [Postman](https://www.getpostman.com/docs/), burada gösterilen:

![Todoıtems ve döndürülen üç öğeleri için JSON gösteren yanıt gövdesi için bir GET isteği gösteren postman konsol](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Öğeleri oluşturma

Kurala göre yeni veri öğeleri oluşturmak için HTTP POST fiil eşlenir. `Create` Yöntemi sahip bir `[HttpPost]` özniteliği uygulanan ve kabul eden bir `ToDoItem` örneği. Bu yana `item` bağımsız değişken POST gövdesinde geçirilecektir, bu parametre ile donatılmış `[FromBody]` özniteliği.

Yöntemi içinde öğe geçerlilik ve veri deposunda önceki varlığı için denetlenir ve herhangi bir sorun oluşursa, depo kullanılarak eklenir. Denetimi `ModelState.IsValid` gerçekleştirir [model doğrulama](../mvc/models/validation.md)ve kullanıcı girişini kabul etme her API yönteminde yapılması gerekir.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

Örnek mobil istemciye geçirilen hata kodları içeren bir enum kullanır:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Postman yeni nesnesi istek gövdesinde JSON biçiminde sağlama sonrası fiil seçerek yeni öğeler eklemek sınayın. Belirten bir istek üstbilgisi de eklemeniz gerekir bir `Content-Type` , `application/json`.

![POST ve yanıt gösteren postman konsol](native-mobile-backend/_static/postman-post.png)

Yöntemi yeni oluşturulan öğeyi yanıt olarak döndürür.

### <a name="updating-items"></a>Öğeler güncelleştiriliyor

Kayıtları değiştirme yapılır HTTP PUT isteklerini kullanarak. Bu değişiklik dışında `Edit` yöntemdir neredeyse aynı `Create`. Kaydı bulunmazsa unutmayın, `Edit` eylem döndürecektir bir `NotFound` (404) yanıt.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Postman ile test etmek için PUT fiili değiştirin. Güncelleştirilmiş nesne verilerini istek gövdesinde belirtin.

![PUT ve yanıt gösteren postman konsol](native-mobile-backend/_static/postman-put.png)

Bu yöntem bir `NoContent` başarılı olduğunda, önceden varolan API ile tutarlılığını (204) yanıt.

### <a name="deleting-items"></a>Öğeleri silme

Kayıtları silme silme isteklerinin hizmete yapma ve silinecek öğe kimliği geçirme gerçekleştirilir. Güncelleştirme ile var olmayan öğeler için istekleri alacak şekilde `NotFound` yanıtlar. Aksi takdirde, başarılı bir istek alırsınız bir `NoContent` (204) yanıt.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Delete işlevselliğini test etme, hiçbir şey istek gövdesinde gerektiğini unutmayın.

![Bir silme ve yanıt gösteren postman konsol](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Ortak Web API kuralları

Uygulamanız için arka uç hizmetlerini geliştirirken, kurallar veya çapraz kesme sorunları işlenmesine yönelik ilkeler tutarlı kümesiyle gündeme isteyeceksiniz. Örneğin, yukarıda gösterilen hizmetinde alınan bulunamadı belirli kayıtlarını ister bir `NotFound` yanıtı yerine bir `BadRequest` yanıt. Benzer şekilde, her zaman kullanıma modele bağlı türlerinde geçirilen bu hizmete yapılan komutları `ModelState.IsValid` ve döndürülen bir `BadRequest` geçersiz model türü için.

Apı'leriniz için ortak bir ilke tanımladıktan sonra genellikle içinde sarmalayabilen bir [filtre](../mvc/controllers/filters.md). Daha fazla bilgi edinmek [ASP.NET Core MVC uygulamalarında ortak API ilkelerini kapsülleyen nasıl](https://msdn.microsoft.com/magazine/mt767699.aspx).
