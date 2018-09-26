---
title: ASP.NET Core ile yerel mobil uygulamalar için arka uç hizmetleri oluşturma
author: ardalis
description: Yerel mobil uygulamaları desteklemek için ASP.NET Core MVC kullanarak arka uç hizmetleri oluşturma konusunda bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 3ebd30ad1ffbd66b256e7f3954a07d682f76a754
ms.sourcegitcommit: 517bb1366da2a28b0014e384fa379755c21b47d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47230184"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a><span data-ttu-id="3f2e8-103">ASP.NET Core ile yerel mobil uygulamalar için arka uç hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="3f2e8-103">Create backend services for native mobile apps with ASP.NET Core</span></span>

<span data-ttu-id="3f2e8-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3f2e8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3f2e8-105">Mobil uygulamalar, ASP.NET Core arka uç Hizmetleri ile kolayca iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="3f2e8-106">Görüntüleme veya indirme örnek arka uç Hizmetleri kodu</span><span class="sxs-lookup"><span data-stu-id="3f2e8-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="3f2e8-107">Örnek yerel mobil uygulama</span><span class="sxs-lookup"><span data-stu-id="3f2e8-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="3f2e8-108">Bu öğreticide, yerel mobil uygulamaları desteklemek için ASP.NET Core MVC kullanarak arka uç hizmetleri oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="3f2e8-109">Kullandığı [Xamarin Forms ToDoRest uygulama](/xamarin/xamarin-forms/data-cloud/consuming/rest) kendi yerel istemci olarak Android, iOS, Windows evrensel ve Windows Phone cihazlar için ayrı yerel istemci içerir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-109">It uses the [Xamarin Forms ToDoRest app](/xamarin/xamarin-forms/data-cloud/consuming/rest) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="3f2e8-110">Yerel uygulama oluşturmak (ve gerekli ücretsiz Xamarin araçları yüklemek için) bağlı bir öğreticiyi izleyin, yapabilir Xamarin örnek çözümü indirin.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="3f2e8-111">Xamarin örnek, bu makalenin ASP.NET Core uygulaması (istemci tarafından hiçbir değişiklik ile) değiştiren bir ASP.NET Web API 2 services projesi içerir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Android bir akıllı telefonda çalışan yapmak Rest uygulamaya](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="3f2e8-113">Özellikler</span><span class="sxs-lookup"><span data-stu-id="3f2e8-113">Features</span></span>

<span data-ttu-id="3f2e8-114">ToDoRest uygulaması, listeleme, ekleme, silme ve güncelleştirme Yapılacaklar öğelerini destekler.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="3f2e8-115">Her öğe bir kimliği, bir ad, notlar ve henüz işiniz olmadığını gösteren bir özelliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="3f2e8-116">Ana görünüm öğeleri, yukarıda gösterildiği gibi her öğenin adını listeler ve bir onay işareti ile yapıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-116">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="3f2e8-117">Dokunarak `+` simgesi Ekle öğesi bir iletişim kutusu açılır:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Öğesi ekleme](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="3f2e8-119">Ana liste ekranı bir öğeye dokunulduğunda, burada öğenin adı, notlar ve yapılan ayarlar değiştirilebilir veya öğenin silinebilmesi için bir düzenleme iletişim kutusu açılır:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Öğe iletişim Düzenle](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="3f2e8-121">Bu örnek salt okunur işlemlere izin veren developer.xamarin.com barındırılan arka uç hizmetlerini kullanmak için varsayılan olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="3f2e8-122">Sonraki bölümde, bilgisayarınızda çalışan ASP.NET Core uygulamayı karşı kendiniz test etmek için uygulamanın güncellemeniz gerekecektir `RestUrl` sabit.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="3f2e8-123">Gidin `ToDoREST` açın ve proje *Constants.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="3f2e8-124">Değiştirin `RestUrl` makinenizin IP içeren bir URL ile (localhost veya 127.0.0.1, bu adresi makinenizden değil cihaz öykücüsünden kullanıldığından değil) adresi.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="3f2e8-125">Bağlantı noktası numarasını de (5000) içerir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-125">Include the port number as well (5000).</span></span> <span data-ttu-id="3f2e8-126">Hizmetlerinizi bir cihazla çalıştığını test etmek için bu bağlantı noktası erişimini engelleyen etkin bir güvenlik duvarı sahip olun.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="3f2e8-127">ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="3f2e8-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="3f2e8-128">Visual Studio'da yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="3f2e8-129">Kimlik doğrulaması yok ve Web API şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="3f2e8-130">Projeyi adlandırın *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-130">Name the project *ToDoApi*.</span></span>

![Seçili Web API proje şablonunu içeren yeni ASP.NET Web uygulaması iletişim kutusu](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="3f2e8-132">Uygulama bağlantı noktası 5000 yapılan tüm isteklere yanıt vermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="3f2e8-133">Güncelleştirme *Program.cs* içerecek şekilde `.UseUrls("http://*:5000")` Bunu başarmak için:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="3f2e8-134">IIS, varsayılan olarak yerel olmayan istekleri yoksayan Express arkasında yapmak yerine, doğrudan, uygulama çalıştırdığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-134">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="3f2e8-135">Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) bir komut isteminden veya Visual Studio araç çubuğundaki hata ayıklama hedefi açılır listesinden bir uygulama adı profili seçin.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-135">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="3f2e8-136">Yapılacaklar öğelerini temsil eden bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-136">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="3f2e8-137">İşareti gerekli alanları kullanarak `[Required]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-137">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="3f2e8-138">API yöntemleri verilerle çalışmak için bazı yol gerekir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-138">The API methods require some way to work with data.</span></span> <span data-ttu-id="3f2e8-139">Aynı `IToDoRepository` özgün Xamarin örnek kullandığı arabirim:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-139">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="3f2e8-140">Bu örnek için uygulama yalnızca özel bir öğe koleksiyonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-140">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="3f2e8-141">Uygulamasında yapılandırma *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-141">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="3f2e8-142">Bu noktada, oluşturmaya hazır *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-142">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="3f2e8-143">API'leri web oluşturma hakkında daha fazla bilgi edinin [ilk Web API'nizi ASP.NET Core MVC ve Visual Studio ile derleme](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-143">Learn more about creating web APIs in [Build your first Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="3f2e8-144">Denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="3f2e8-144">Creating the Controller</span></span>

<span data-ttu-id="3f2e8-145">Projeye yeni bir denetleyici ekleyeceksiniz *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-145">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="3f2e8-146">Microsoft.AspNetCore.Mvc.Controller devralan.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-146">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="3f2e8-147">Ekleme bir `Route` Denetleyici ile başlayan yollar için istekleri işleyeceğini belirtmek için özniteliği `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-147">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="3f2e8-148">`[controller]` Belirteci rotadaki denetleyici adıyla değiştirilir (atlama `Controller` soneki) ve genel yollar için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-148">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="3f2e8-149">Daha fazla bilgi edinin [yönlendirme](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-149">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="3f2e8-150">Denetleyici gerektiren bir `IToDoRepository` için işlev; bu tür denetleyicinin Oluşturucusu aracılığıyla örneği isteği.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-150">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="3f2e8-151">Framework'ün desteğini kullanarak, çalışma zamanında bu örneği sağlanacaktır [bağımlılık ekleme](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-151">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="3f2e8-152">Bu API, dört farklı HTTP fiilleri CRUD (oluşturma, okuma, güncelleştirme, silme) veri kaynağı işlemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-152">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="3f2e8-153">Bu en basit bir HTTP GET isteğine karşılık gelen okuma işlemi var.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-153">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="3f2e8-154">Öğeler okunuyor</span><span class="sxs-lookup"><span data-stu-id="3f2e8-154">Reading Items</span></span>

<span data-ttu-id="3f2e8-155">Öğeleri listesi istenirken bir GET isteği ile yapılır `List` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-155">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="3f2e8-156">`[HttpGet]` Özniteliği `List` yöntemi, bu eylem yalnızca GET istekleri işleyeceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-156">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="3f2e8-157">Bu eylem için yol üzerindeki denetleyiciye belirtilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-157">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="3f2e8-158">Eylem adı yolun bir parçası olarak kullanmak mutlaka gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-158">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="3f2e8-159">Her bir eylemin benzersiz ve belirsiz bir yol olduğundan emin olun yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-159">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="3f2e8-160">Yönlendirme öznitelikleri denetleyici ve yöntem düzeylerinde belirli rotaları oluşturmak için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-160">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="3f2e8-161">`List` Yöntem bir 200 OK yanıtı kodu ve tüm JSON olarak serileştirilen ToDo öğeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-161">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="3f2e8-162">Yeni API yönteminizi gibi çeşitli araçları kullanarak test edebilirsiniz [Postman](https://www.getpostman.com/docs/), burada gösterilen:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-162">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Todoıtems ve döndürülen üç öğe için JSON gösteren yanıt gövdesi için bir GET isteği gösteren postman konsol](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="3f2e8-164">Öğeleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="3f2e8-164">Creating Items</span></span>

<span data-ttu-id="3f2e8-165">Kural gereği, yeni veri öğeleri oluşturmak için HTTP POST edimi eşlenir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-165">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="3f2e8-166">`Create` Yöntemi olan bir `[HttpPost]` özniteliği uygulanmış ve kabul eden bir `ToDoItem` örneği.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-166">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="3f2e8-167">Bu yana `item` bağımsız değişken POST gövdesinde geçirilir, bu parametre ile donatılmış `[FromBody]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-167">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="3f2e8-168">Yöntemi içinde geçerlilik ve önceki bulunma veri deposundaki öğeyi denetlenir ve herhangi bir sorun meydana gelirse depo kullanmaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-168">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="3f2e8-169">Denetimi `ModelState.IsValid` gerçekleştirir [model doğrulama](../mvc/models/validation.md)ve kullanıcı girişi kabul eden her API yöntemi yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-169">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="3f2e8-170">Örnek mobil istemciye geçirilen hata kodları içeren enum kullanır:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-170">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="3f2e8-171">İstek gövdesi JSON biçiminde yeni nesne sağlama sonrası fiili seçerek Postman kullanarak yeni öğe ekleme test edin.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-171">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="3f2e8-172">İstek üst bilgisi belirtme de eklemeniz gerekir bir `Content-Type` , `application/json`.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-172">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![POST ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="3f2e8-174">Bu yöntem, yanıt olarak yeni oluşturulan öğeyi döndürür.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-174">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="3f2e8-175">Öğeler güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="3f2e8-175">Updating Items</span></span>

<span data-ttu-id="3f2e8-176">Kayıtlarını değiştirme yapılır HTTP PUT isteklerini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-176">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="3f2e8-177">Bu değişikliğin dışında `Edit` yöntemi neredeyse aynı `Create`.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-177">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="3f2e8-178">Kayıt bulunmazsa unutmayın, `Edit` eylem döndürür bir `NotFound` (404) yanıt.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-178">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="3f2e8-179">Postman ile test etmek için PUT fiili değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-179">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="3f2e8-180">İstek gövdesinde güncelleştirilmiş nesne verilerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-180">Specify the updated object data in the Body of the request.</span></span>

![PUT ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="3f2e8-182">Bu yöntem döndürür bir `NoContent` başarılı olduğunda, önceden mevcut olan API tutarlılık (204) yanıt.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-182">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="3f2e8-183">Öğeleri silme</span><span class="sxs-lookup"><span data-stu-id="3f2e8-183">Deleting Items</span></span>

<span data-ttu-id="3f2e8-184">Kayıt silme, hizmette silme isteği yapmak ve silinecek öğenin kimliği geçirerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-184">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="3f2e8-185">Güncelleştirme ile mevcut olmayan öğeler için istekleri alırsınız gibi `NotFound` yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-185">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="3f2e8-186">Aksi takdirde, başarılı bir istek alırsınız bir `NoContent` (204) yanıt.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-186">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="3f2e8-187">Delete işlevi test ederken hiçbir istek gövdesinde gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-187">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![SİLME ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="3f2e8-189">Genel Web API kurallar</span><span class="sxs-lookup"><span data-stu-id="3f2e8-189">Common Web API Conventions</span></span>

<span data-ttu-id="3f2e8-190">Uygulamanız için arka uç Hizmetleri geliştirirken, kuralları veya çapraz kesme konuları işlenmesine yönelik ilkeler tutarlı özellik kümesi ile gelen isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-190">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="3f2e8-191">Örneğin, yukarıda gösterilen hizmete alınan bulunamadı belirli kayıtları için istekleri bir `NotFound` yanıt yerine `BadRequest` yanıt.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-191">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="3f2e8-192">Benzer şekilde, her zaman kullanıma bağlı model türleri geçirilen bu Hizmeti'ne yapılan komutları `ModelState.IsValid` ve döndürülen bir `BadRequest` geçersiz model türleri için.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-192">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="3f2e8-193">Apı'leriniz için ortak bir ilke belirledikten sonra genellikle içinde kapsülleyebilir bir [filtre](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-193">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="3f2e8-194">Daha fazla bilgi edinin [nasıl ASP.NET Core MVC uygulamalarında ortak API ilkelerini Yalıt](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-194">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f2e8-195">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3f2e8-195">Additional resources</span></span>

* [<span data-ttu-id="3f2e8-196">Kimlik Doğrulaması ve Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="3f2e8-196">Authentication and Authorization</span></span>](/xamarin/xamarin-forms/enterprise-application-patterns/authentication-and-authorization)
