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
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a><span data-ttu-id="29749-103">ASP.NET Core ile yerel mobil uygulamalar için arka uç hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="29749-103">Create backend services for native mobile apps with ASP.NET Core</span></span>

<span data-ttu-id="29749-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="29749-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="29749-105">Mobil uygulamalar, ASP.NET Core arka uç Hizmetleri ile iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="29749-105">Mobile apps can communicate with ASP.NET Core backend services.</span></span> <span data-ttu-id="29749-106">Android öykünücüleri ve iOS simülatörleri yerel web hizmetlerine bağlanma ile ilgili yönergeler için bkz: [iOS Simülatörleri ve Android Öykünücüleri yerel Web hizmetlerine Connect'ten](/xamarin/cross-platform/deploy-test/connect-to-local-web-services).</span><span class="sxs-lookup"><span data-stu-id="29749-106">For instructions on connecting local web services from iOS simulators and Android emulators, see [Connect to Local Web Services from iOS Simulators and Android Emulators](/xamarin/cross-platform/deploy-test/connect-to-local-web-services).</span></span>

[<span data-ttu-id="29749-107">Görüntüleme veya indirme örnek arka uç Hizmetleri kodu</span><span class="sxs-lookup"><span data-stu-id="29749-107">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="29749-108">Örnek yerel mobil uygulama</span><span class="sxs-lookup"><span data-stu-id="29749-108">The Sample Native Mobile App</span></span>

<span data-ttu-id="29749-109">Bu öğreticide, yerel mobil uygulamaları desteklemek için ASP.NET Core MVC kullanarak arka uç hizmetleri oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="29749-109">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="29749-110">Kullandığı [Xamarin Forms ToDoRest uygulama](/xamarin/xamarin-forms/data-cloud/consuming/rest) kendi yerel istemci olarak Android, iOS, Windows evrensel ve Windows Phone cihazlar için ayrı yerel istemci içerir.</span><span class="sxs-lookup"><span data-stu-id="29749-110">It uses the [Xamarin Forms ToDoRest app](/xamarin/xamarin-forms/data-cloud/consuming/rest) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="29749-111">Yerel uygulama oluşturmak (ve gerekli ücretsiz Xamarin araçları yüklemek için) bağlı bir öğreticiyi izleyin, yapabilir Xamarin örnek çözümü indirin.</span><span class="sxs-lookup"><span data-stu-id="29749-111">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="29749-112">Xamarin örnek, bu makalenin ASP.NET Core uygulaması (istemci tarafından hiçbir değişiklik ile) değiştiren bir ASP.NET Web API 2 services projesi içerir.</span><span class="sxs-lookup"><span data-stu-id="29749-112">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Android bir akıllı telefonda çalışan yapmak Rest uygulamaya](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="29749-114">Özellikler</span><span class="sxs-lookup"><span data-stu-id="29749-114">Features</span></span>

<span data-ttu-id="29749-115">ToDoRest uygulaması, listeleme, ekleme, silme ve güncelleştirme Yapılacaklar öğelerini destekler.</span><span class="sxs-lookup"><span data-stu-id="29749-115">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="29749-116">Her öğe bir kimliği, bir ad, notlar ve henüz işiniz olmadığını gösteren bir özelliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="29749-116">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="29749-117">Ana görünüm öğeleri, yukarıda gösterildiği gibi her öğenin adını listeler ve bir onay işareti ile yapıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="29749-117">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="29749-118">Dokunarak `+` simgesi Ekle öğesi bir iletişim kutusu açılır:</span><span class="sxs-lookup"><span data-stu-id="29749-118">Tapping the `+` icon opens an add item dialog:</span></span>

![Öğesi ekleme](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="29749-120">Ana liste ekranı bir öğeye dokunulduğunda, burada öğenin adı, notlar ve yapılan ayarlar değiştirilebilir veya öğenin silinebilmesi için bir düzenleme iletişim kutusu açılır:</span><span class="sxs-lookup"><span data-stu-id="29749-120">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Öğe iletişim Düzenle](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="29749-122">Bu örnek salt okunur işlemlere izin veren developer.xamarin.com barındırılan arka uç hizmetlerini kullanmak için varsayılan olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="29749-122">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="29749-123">Sonraki bölümde, bilgisayarınızda çalışan ASP.NET Core uygulamayı karşı kendiniz test etmek için uygulamanın güncellemeniz gerekecektir `RestUrl` sabit.</span><span class="sxs-lookup"><span data-stu-id="29749-123">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="29749-124">Gidin `ToDoREST` açın ve proje *Constants.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="29749-124">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="29749-125">Değiştirin `RestUrl` makinenizin IP içeren bir URL ile (localhost veya 127.0.0.1, bu adresi makinenizden değil cihaz öykücüsünden kullanıldığından değil) adresi.</span><span class="sxs-lookup"><span data-stu-id="29749-125">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="29749-126">Bağlantı noktası numarasını de (5000) içerir.</span><span class="sxs-lookup"><span data-stu-id="29749-126">Include the port number as well (5000).</span></span> <span data-ttu-id="29749-127">Hizmetlerinizi bir cihazla çalıştığını test etmek için bu bağlantı noktası erişimini engelleyen etkin bir güvenlik duvarı sahip olun.</span><span class="sxs-lookup"><span data-stu-id="29749-127">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="29749-128">ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="29749-128">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="29749-129">Visual Studio'da yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="29749-129">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="29749-130">Kimlik doğrulaması yok ve Web API şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="29749-130">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="29749-131">Projeyi adlandırın *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="29749-131">Name the project *ToDoApi*.</span></span>

![Seçili Web API proje şablonunu içeren yeni ASP.NET Web uygulaması iletişim kutusu](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="29749-133">Uygulama bağlantı noktası 5000 yapılan tüm isteklere yanıt vermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="29749-133">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="29749-134">Güncelleştirme *Program.cs* içerecek şekilde `.UseUrls("http://*:5000")` Bunu başarmak için:</span><span class="sxs-lookup"><span data-stu-id="29749-134">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="29749-135">IIS, varsayılan olarak yerel olmayan istekleri yoksayan Express arkasında yapmak yerine, doğrudan, uygulama çalıştırdığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="29749-135">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="29749-136">Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) bir komut isteminden veya Visual Studio araç çubuğundaki hata ayıklama hedefi açılır listesinden bir uygulama adı profili seçin.</span><span class="sxs-lookup"><span data-stu-id="29749-136">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="29749-137">Yapılacaklar öğelerini temsil eden bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="29749-137">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="29749-138">İşareti gerekli alanları kullanarak `[Required]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="29749-138">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="29749-139">API yöntemleri verilerle çalışmak için bazı yol gerekir.</span><span class="sxs-lookup"><span data-stu-id="29749-139">The API methods require some way to work with data.</span></span> <span data-ttu-id="29749-140">Aynı `IToDoRepository` özgün Xamarin örnek kullandığı arabirim:</span><span class="sxs-lookup"><span data-stu-id="29749-140">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="29749-141">Bu örnek için uygulama yalnızca özel bir öğe koleksiyonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="29749-141">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="29749-142">Uygulamasında yapılandırma *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="29749-142">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="29749-143">Bu noktada, oluşturmaya hazır *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="29749-143">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="29749-144">API'leri web oluşturma hakkında daha fazla bilgi edinin [ilk Web API'nizi ASP.NET Core MVC ve Visual Studio ile derleme](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="29749-144">Learn more about creating web APIs in [Build your first Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="29749-145">Denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="29749-145">Creating the Controller</span></span>

<span data-ttu-id="29749-146">Projeye yeni bir denetleyici ekleyeceksiniz *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="29749-146">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="29749-147">Microsoft.AspNetCore.Mvc.Controller devralan.</span><span class="sxs-lookup"><span data-stu-id="29749-147">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="29749-148">Ekleme bir `Route` Denetleyici ile başlayan yollar için istekleri işleyeceğini belirtmek için özniteliği `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="29749-148">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="29749-149">`[controller]` Belirteci rotadaki denetleyici adıyla değiştirilir (atlama `Controller` soneki) ve genel yollar için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="29749-149">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="29749-150">Daha fazla bilgi edinin [yönlendirme](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="29749-150">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="29749-151">Denetleyici gerektiren bir `IToDoRepository` için işlev; bu tür denetleyicinin Oluşturucusu aracılığıyla örneği isteği.</span><span class="sxs-lookup"><span data-stu-id="29749-151">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="29749-152">Framework'ün desteğini kullanarak, çalışma zamanında bu örneği sağlanacaktır [bağımlılık ekleme](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="29749-152">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="29749-153">Bu API, dört farklı HTTP fiilleri CRUD (oluşturma, okuma, güncelleştirme, silme) veri kaynağı işlemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="29749-153">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="29749-154">Bu en basit bir HTTP GET isteğine karşılık gelen okuma işlemi var.</span><span class="sxs-lookup"><span data-stu-id="29749-154">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="29749-155">Öğeler okunuyor</span><span class="sxs-lookup"><span data-stu-id="29749-155">Reading Items</span></span>

<span data-ttu-id="29749-156">Öğeleri listesi istenirken bir GET isteği ile yapılır `List` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="29749-156">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="29749-157">`[HttpGet]` Özniteliği `List` yöntemi, bu eylem yalnızca GET istekleri işleyeceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="29749-157">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="29749-158">Bu eylem için yol üzerindeki denetleyiciye belirtilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="29749-158">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="29749-159">Eylem adı yolun bir parçası olarak kullanmak mutlaka gerekmez.</span><span class="sxs-lookup"><span data-stu-id="29749-159">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="29749-160">Her bir eylemin benzersiz ve belirsiz bir yol olduğundan emin olun yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="29749-160">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="29749-161">Yönlendirme öznitelikleri denetleyici ve yöntem düzeylerinde belirli rotaları oluşturmak için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="29749-161">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="29749-162">`List` Yöntem bir 200 OK yanıtı kodu ve tüm JSON olarak serileştirilen ToDo öğeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="29749-162">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="29749-163">Yeni API yönteminizi gibi çeşitli araçları kullanarak test edebilirsiniz [Postman](https://www.getpostman.com/docs/), burada gösterilen:</span><span class="sxs-lookup"><span data-stu-id="29749-163">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Todoıtems ve döndürülen üç öğe için JSON gösteren yanıt gövdesi için bir GET isteği gösteren postman konsol](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="29749-165">Öğeleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="29749-165">Creating Items</span></span>

<span data-ttu-id="29749-166">Kural gereği, yeni veri öğeleri oluşturmak için HTTP POST edimi eşlenir.</span><span class="sxs-lookup"><span data-stu-id="29749-166">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="29749-167">`Create` Yöntemi olan bir `[HttpPost]` özniteliği uygulanmış ve kabul eden bir `ToDoItem` örneği.</span><span class="sxs-lookup"><span data-stu-id="29749-167">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="29749-168">Bu yana `item` bağımsız değişken POST gövdesinde geçirilir, bu parametre ile donatılmış `[FromBody]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="29749-168">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="29749-169">Yöntemi içinde geçerlilik ve önceki bulunma veri deposundaki öğeyi denetlenir ve herhangi bir sorun meydana gelirse depo kullanmaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="29749-169">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="29749-170">Denetimi `ModelState.IsValid` gerçekleştirir [model doğrulama](../mvc/models/validation.md)ve kullanıcı girişi kabul eden her API yöntemi yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="29749-170">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="29749-171">Örnek mobil istemciye geçirilen hata kodları içeren enum kullanır:</span><span class="sxs-lookup"><span data-stu-id="29749-171">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="29749-172">İstek gövdesi JSON biçiminde yeni nesne sağlama sonrası fiili seçerek Postman kullanarak yeni öğe ekleme test edin.</span><span class="sxs-lookup"><span data-stu-id="29749-172">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="29749-173">İstek üst bilgisi belirtme de eklemeniz gerekir bir `Content-Type` , `application/json`.</span><span class="sxs-lookup"><span data-stu-id="29749-173">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![POST ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="29749-175">Bu yöntem, yanıt olarak yeni oluşturulan öğeyi döndürür.</span><span class="sxs-lookup"><span data-stu-id="29749-175">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="29749-176">Öğeler güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="29749-176">Updating Items</span></span>

<span data-ttu-id="29749-177">Kayıtlarını değiştirme yapılır HTTP PUT isteklerini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="29749-177">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="29749-178">Bu değişikliğin dışında `Edit` yöntemi neredeyse aynı `Create`.</span><span class="sxs-lookup"><span data-stu-id="29749-178">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="29749-179">Kayıt bulunmazsa unutmayın, `Edit` eylem döndürür bir `NotFound` (404) yanıt.</span><span class="sxs-lookup"><span data-stu-id="29749-179">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="29749-180">Postman ile test etmek için PUT fiili değiştirin.</span><span class="sxs-lookup"><span data-stu-id="29749-180">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="29749-181">İstek gövdesinde güncelleştirilmiş nesne verilerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="29749-181">Specify the updated object data in the Body of the request.</span></span>

![PUT ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="29749-183">Bu yöntem döndürür bir `NoContent` başarılı olduğunda, önceden mevcut olan API tutarlılık (204) yanıt.</span><span class="sxs-lookup"><span data-stu-id="29749-183">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="29749-184">Öğeleri silme</span><span class="sxs-lookup"><span data-stu-id="29749-184">Deleting Items</span></span>

<span data-ttu-id="29749-185">Kayıt silme, hizmette silme isteği yapmak ve silinecek öğenin kimliği geçirerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="29749-185">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="29749-186">Güncelleştirme ile mevcut olmayan öğeler için istekleri alırsınız gibi `NotFound` yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="29749-186">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="29749-187">Aksi takdirde, başarılı bir istek alırsınız bir `NoContent` (204) yanıt.</span><span class="sxs-lookup"><span data-stu-id="29749-187">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="29749-188">Delete işlevi test ederken hiçbir istek gövdesinde gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="29749-188">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![SİLME ve yanıtı gösteren postman konsol](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="29749-190">Genel Web API kurallar</span><span class="sxs-lookup"><span data-stu-id="29749-190">Common Web API Conventions</span></span>

<span data-ttu-id="29749-191">Uygulamanız için arka uç Hizmetleri geliştirirken, kuralları veya çapraz kesme konuları işlenmesine yönelik ilkeler tutarlı özellik kümesi ile gelen isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="29749-191">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="29749-192">Örneğin, yukarıda gösterilen hizmete alınan bulunamadı belirli kayıtları için istekleri bir `NotFound` yanıt yerine `BadRequest` yanıt.</span><span class="sxs-lookup"><span data-stu-id="29749-192">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="29749-193">Benzer şekilde, her zaman kullanıma bağlı model türleri geçirilen bu Hizmeti'ne yapılan komutları `ModelState.IsValid` ve döndürülen bir `BadRequest` geçersiz model türleri için.</span><span class="sxs-lookup"><span data-stu-id="29749-193">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="29749-194">Apı'leriniz için ortak bir ilke belirledikten sonra genellikle içinde kapsülleyebilir bir [filtre](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="29749-194">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="29749-195">Daha fazla bilgi edinin [nasıl ASP.NET Core MVC uygulamalarında ortak API ilkelerini Yalıt](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="29749-195">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="29749-196">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="29749-196">Additional resources</span></span>

* [<span data-ttu-id="29749-197">Kimlik Doğrulaması ve Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="29749-197">Authentication and Authorization</span></span>](/xamarin/xamarin-forms/enterprise-application-patterns/authentication-and-authorization)
