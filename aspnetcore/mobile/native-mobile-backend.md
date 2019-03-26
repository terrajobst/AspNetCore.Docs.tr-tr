---
title: ASP.NET Core ile yerel mobil uygulamalar için arka uç hizmetleri oluşturma
author: ardalis
description: Yerel mobil uygulamaları desteklemek için ASP.NET Core MVC kullanarak arka uç hizmetleri oluşturma konusunda bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 13149dd4b877b8c17d33d428779ad31d8c51ae9e
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488734"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a>ASP.NET Core ile yerel mobil uygulamalar için arka uç hizmetleri oluşturma

Tarafından [Steve Smith](https://ardalis.com/)

Mobil uygulamalar, ASP.NET Core arka uç Hizmetleri ile iletişim kurabilir. Android öykünücüleri ve iOS simülatörleri yerel web hizmetlerine bağlanma ile ilgili yönergeler için bkz: [iOS Simülatörleri ve Android Öykünücüleri yerel Web hizmetlerine Connect'ten](/xamarin/cross-platform/deploy-test/connect-to-local-web-services).

[Görüntüleme veya indirme örnek arka uç Hizmetleri kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Örnek yerel mobil uygulama

Bu öğreticide, yerel mobil uygulamaları desteklemek için ASP.NET Core MVC kullanarak arka uç hizmetleri oluşturma gösterilmektedir. Kullandığı [Xamarin Forms ToDoRest uygulama](/xamarin/xamarin-forms/data-cloud/consuming/rest) kendi yerel istemci olarak Android, iOS, Windows evrensel ve Windows Phone cihazlar için ayrı yerel istemci içerir. Yerel uygulama oluşturmak (ve gerekli ücretsiz Xamarin araçları yüklemek için) bağlı bir öğreticiyi izleyin, yapabilir Xamarin örnek çözümü indirin. Xamarin örnek, bu makalenin ASP.NET Core uygulaması (istemci tarafından hiçbir değişiklik ile) değiştiren bir ASP.NET Web API 2 services projesi içerir.

![Android bir akıllı telefonda çalışan yapmak Rest uygulamaya](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Özellikler

ToDoRest uygulaması, listeleme, ekleme, silme ve güncelleştirme Yapılacaklar öğelerini destekler. Her öğe bir kimliği, bir ad, notlar ve henüz işiniz olmadığını gösteren bir özelliğe sahiptir.

Ana görünüm öğeleri, yukarıda gösterildiği gibi her öğenin adını listeler ve bir onay işareti ile yapıldığını gösterir.

Dokunarak `+` simgesi Ekle öğesi bir iletişim kutusu açılır:

![Öğesi ekleme](native-mobile-backend/_static/todo-android-new-item.png)

Ana liste ekranı bir öğeye dokunulduğunda, burada öğenin adı, notlar ve yapılan ayarlar değiştirilebilir veya öğenin silinebilmesi için bir düzenleme iletişim kutusu açılır:

![Öğe iletişim Düzenle](native-mobile-backend/_static/todo-android-edit-item.png)

Bu örnek salt okunur işlemlere izin veren developer.xamarin.com barındırılan arka uç hizmetlerini kullanmak için varsayılan olarak yapılandırılır. Sonraki bölümde, bilgisayarınızda çalışan ASP.NET Core uygulamayı karşı kendiniz test etmek için uygulamanın güncellemeniz gerekecektir `RestUrl` sabit. Gidin `ToDoREST` açın ve proje *Constants.cs* dosya. Değiştirin `RestUrl` makinenizin IP içeren bir URL ile (localhost veya 127.0.0.1, bu adresi makinenizden değil cihaz öykücüsünden kullanıldığından değil) adresi. Bağlantı noktası numarasını de (5000) içerir. Hizmetlerinizi bir cihazla çalıştığını test etmek için bu bağlantı noktası erişimini engelleyen etkin bir güvenlik duvarı sahip olun.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>ASP.NET Core projesi oluşturma

Visual Studio'da yeni bir ASP.NET Core Web uygulaması oluşturun. Kimlik doğrulaması yok ve Web API şablonu seçin. Projeyi adlandırın *ToDoApi*.

![Seçili Web API proje şablonunu içeren yeni ASP.NET Web uygulaması iletişim kutusu](native-mobile-backend/_static/web-api-template.png)

Uygulama bağlantı noktası 5000 yapılan tüm isteklere yanıt vermesi gerekir. Güncelleştirme *Program.cs* içerecek şekilde `.UseUrls("http://*:5000")` Bunu başarmak için:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> IIS, varsayılan olarak yerel olmayan istekleri yoksayan Express arkasında yapmak yerine, doğrudan, uygulama çalıştırdığınızdan emin olun. Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) bir komut isteminden veya Visual Studio araç çubuğundaki hata ayıklama hedefi açılır listesinden bir uygulama adı profili seçin.

Yapılacaklar öğelerini temsil eden bir model sınıfı ekleyin. İşareti gerekli alanları kullanarak `[Required]` özniteliği:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

API yöntemleri verilerle çalışmak için bazı yol gerekir. Aynı `IToDoRepository` özgün Xamarin örnek kullandığı arabirim:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Bu örnek için uygulama yalnızca özel bir öğe koleksiyonu kullanır:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Uygulamasında yapılandırma *Startup.cs*:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

Bu noktada, oluşturmaya hazır *ToDoItemsController*.

> [!TIP]
> API'leri web oluşturma hakkında daha fazla bilgi edinin [ilk Web API'nizi ASP.NET Core MVC ve Visual Studio ile derleme](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Denetleyici oluşturma

Projeye yeni bir denetleyici ekleyeceksiniz *ToDoItemsController*. Microsoft.AspNetCore.Mvc.Controller devralan. Ekleme bir `Route` Denetleyici ile başlayan yollar için istekleri işleyeceğini belirtmek için özniteliği `api/todoitems`. `[controller]` Belirteci rotadaki denetleyici adıyla değiştirilir (atlama `Controller` soneki) ve genel yollar için özellikle yararlıdır. Daha fazla bilgi edinin [yönlendirme](../fundamentals/routing.md).

Denetleyici gerektiren bir `IToDoRepository` için işlev; bu tür denetleyicinin Oluşturucusu aracılığıyla örneği isteği. Framework'ün desteğini kullanarak, çalışma zamanında bu örneği sağlanacaktır [bağımlılık ekleme](../fundamentals/dependency-injection.md).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Bu API, dört farklı HTTP fiilleri CRUD (oluşturma, okuma, güncelleştirme, silme) veri kaynağı işlemleri destekler. Bu en basit bir HTTP GET isteğine karşılık gelen okuma işlemi var.

### <a name="reading-items"></a>Öğeler okunuyor

Öğeleri listesi istenirken bir GET isteği ile yapılır `List` yöntemi. `[HttpGet]` Özniteliği `List` yöntemi, bu eylem yalnızca GET istekleri işleyeceğini gösterir. Bu eylem için yol üzerindeki denetleyiciye belirtilen yoldur. Eylem adı yolun bir parçası olarak kullanmak mutlaka gerekmez. Her bir eylemin benzersiz ve belirsiz bir yol olduğundan emin olun yeterlidir. Yönlendirme öznitelikleri denetleyici ve yöntem düzeylerinde belirli rotaları oluşturmak için uygulanabilir.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List` Yöntem bir 200 OK yanıtı kodu ve tüm JSON olarak serileştirilen ToDo öğeleri döndürür.

Yeni API yönteminizi gibi çeşitli araçları kullanarak test edebilirsiniz [Postman](https://www.getpostman.com/docs/), burada gösterilen:

![Todoıtems ve döndürülen üç öğe için JSON gösteren yanıt gövdesi için bir GET isteği gösteren postman konsol](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Öğeleri oluşturma

Kural gereği, yeni veri öğeleri oluşturmak için HTTP POST edimi eşlenir. `Create` Yöntemi olan bir `[HttpPost]` özniteliği uygulanmış ve kabul eden bir `ToDoItem` örneği. Bu yana `item` bağımsız değişken POST gövdesinde geçirilir, bu parametre ile donatılmış `[FromBody]` özniteliği.

Yöntemi içinde geçerlilik ve önceki bulunma veri deposundaki öğeyi denetlenir ve herhangi bir sorun meydana gelirse depo kullanmaya eklenir. Denetimi `ModelState.IsValid` gerçekleştirir [model doğrulama](../mvc/models/validation.md)ve kullanıcı girişi kabul eden her API yöntemi yapılmalıdır.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

Örnek mobil istemciye geçirilen hata kodları içeren enum kullanır:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

İstek gövdesi JSON biçiminde yeni nesne sağlama sonrası fiili seçerek Postman kullanarak yeni öğe ekleme test edin. İstek üst bilgisi belirtme de eklemeniz gerekir bir `Content-Type` , `application/json`.

![POST ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-post.png)

Bu yöntem, yanıt olarak yeni oluşturulan öğeyi döndürür.

### <a name="updating-items"></a>Öğeler güncelleştiriliyor

Kayıtlarını değiştirme yapılır HTTP PUT isteklerini kullanarak. Bu değişikliğin dışında `Edit` yöntemi neredeyse aynı `Create`. Kayıt bulunmazsa unutmayın, `Edit` eylem döndürür bir `NotFound` (404) yanıt.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Postman ile test etmek için PUT fiili değiştirin. İstek gövdesinde güncelleştirilmiş nesne verilerini belirtin.

![PUT ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-put.png)

Bu yöntem döndürür bir `NoContent` başarılı olduğunda, önceden mevcut olan API tutarlılık (204) yanıt.

### <a name="deleting-items"></a>Öğeleri silme

Kayıt silme, hizmette silme isteği yapmak ve silinecek öğenin kimliği geçirerek gerçekleştirilir. Güncelleştirme ile mevcut olmayan öğeler için istekleri alırsınız gibi `NotFound` yanıtlar. Aksi takdirde, başarılı bir istek alırsınız bir `NoContent` (204) yanıt.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Delete işlevi test ederken hiçbir istek gövdesinde gerektiğini unutmayın.

![SİLME ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Genel Web API kurallar

Uygulamanız için arka uç Hizmetleri geliştirirken, kuralları veya çapraz kesme konuları işlenmesine yönelik ilkeler tutarlı özellik kümesi ile gelen isteyeceksiniz. Örneğin, yukarıda gösterilen hizmete alınan bulunamadı belirli kayıtları için istekleri bir `NotFound` yanıt yerine `BadRequest` yanıt. Benzer şekilde, her zaman kullanıma bağlı model türleri geçirilen bu Hizmeti'ne yapılan komutları `ModelState.IsValid` ve döndürülen bir `BadRequest` geçersiz model türleri için.

Apı'leriniz için ortak bir ilke belirledikten sonra genellikle içinde kapsülleyebilir bir [filtre](../mvc/controllers/filters.md). Daha fazla bilgi edinin [nasıl ASP.NET Core MVC uygulamalarında ortak API ilkelerini Yalıt](https://msdn.microsoft.com/magazine/mt767699.aspx).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kimlik Doğrulaması ve Yetkilendirme](/xamarin/xamarin-forms/enterprise-application-patterns/authentication-and-authorization)
