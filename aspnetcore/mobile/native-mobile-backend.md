---
title: "Yerel mobil uygulamalar için arka uç hizmetleri oluşturma"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mobile/native-mobile-backend
ms.openlocfilehash: f5c0e5832b43f365c11a785af37b32e381624420
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="6cdd0-102">Yerel mobil uygulamalar için arka uç hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6cdd0-102">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="6cdd0-103">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6cdd0-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6cdd0-104">Mobil uygulamaları kolayca ASP.NET Core arka uç hizmetleriyle iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-104">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="6cdd0-105">Görüntülemek veya karşıdan örnek arka uç hizmetlerini kod</span><span class="sxs-lookup"><span data-stu-id="6cdd0-105">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="6cdd0-106">Örnek yerel mobil uygulama</span><span class="sxs-lookup"><span data-stu-id="6cdd0-106">The Sample Native Mobile App</span></span>

<span data-ttu-id="6cdd0-107">Bu öğretici, yerel mobil uygulamalar desteklemek için ASP.NET Core MVC kullanarak arka uç hizmetlerini oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-107">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="6cdd0-108">Kullandığı [Xamarin Forms ToDoRest uygulama](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) kendi yerel istemci olarak Android, iOS, Windows evrensel ve Windows Phone cihazları için ayrı yerel istemcilerde içerir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-108">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="6cdd0-109">Sizin yerel uygulama oluşturma (ve gerekli boş Xamarin Araçları'nı yüklemek için) bağlı öğreticisini izleyin yanı Xamarin örnek çözümü indirin.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-109">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="6cdd0-110">Xamarin örnek bu makalenin ASP.NET Core uygulama (istemci tarafından gerekli değişiklik yok) yerine bir ASP.NET Web API 2 services projesi içerir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-110">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Bir Android smartphone üzerinde çalışan Rest yapmak uygulama](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="6cdd0-112">Özellikler</span><span class="sxs-lookup"><span data-stu-id="6cdd0-112">Features</span></span>

<span data-ttu-id="6cdd0-113">ToDoRest uygulama listeleme, ekleme, silme ve yapılacaklar öğelerini güncelleştirme destekler.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-113">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="6cdd0-114">Her bir öğe kimliği, bir ad, notlar ve henüz girildiği olup olmadığını gösteren bir özellik vardır.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-114">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="6cdd0-115">Ana görünümü öğeleri, yukarıda gösterildiği gibi her öğenin adını listeler ve işareti ile yapılır, gösterir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-115">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="6cdd0-116">Dokunarak `+` simgesi Ekle öğesi iletişim açar:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-116">Tapping the `+` icon opens an add item dialog:</span></span>

![Öğe iletişim ekleyin](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="6cdd0-118">Öğenin ana listesi ekranında dokunarak burada öğenin adı, notlar ve Bitti'yi ayarları değiştirilebilir veya öğenin silinebilmesi bir düzenleme iletişim kutusunu açar:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-118">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Öğesi iletişim Düzenle](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="6cdd0-120">Bu örnek, varsayılan olarak developer.xamarin.com barındırılan salt okunur işlemlere izin arka uç hizmetlerine kullanacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-120">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="6cdd0-121">Bilgisayarınızda çalışan sonraki bölümde oluşturulan ASP.NET Core uygulaması karşı kendiniz sınamak için uygulamanın güncelleştirme gerekecektir `RestUrl` sabit.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-121">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="6cdd0-122">Gidin `ToDoREST` proje ve açık *Constants.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-122">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="6cdd0-123">Değiştir `RestUrl` makinenizin IP içeren bir URL adresini (localhost veya 127.0.0.1, bu adres, makineden aygıt öykücüsünden kullanıldığından değil).</span><span class="sxs-lookup"><span data-stu-id="6cdd0-123">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="6cdd0-124">Bağlantı noktası numarasını da (5000) içerir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-124">Include the port number as well (5000).</span></span> <span data-ttu-id="6cdd0-125">Hizmetlerinizin bir cihazla çalışmayı test etmek için bu bağlantı noktası erişimi engelleme etkin bir güvenlik duvarı olmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-125">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="6cdd0-126">ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="6cdd0-126">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="6cdd0-127">Visual Studio'da yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-127">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="6cdd0-128">Hayır kimlik doğrulaması ve Web API şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-128">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="6cdd0-129">Proje adı *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-129">Name the project *ToDoApi*.</span></span>

![Seçilen Web API projesi şablonuyla yeni ASP.NET Web uygulaması iletişim kutusu](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="6cdd0-131">Uygulama bağlantı noktası 5000 yapılan tüm isteklere yanıt.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-131">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="6cdd0-132">Güncelleştirme *Program.cs* içerecek şekilde `.UseUrls("http://*:5000")` Bunu başarmak için:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-132">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="6cdd0-133">IIS, varsayılan olarak yerel olmayan isteklerini yoksayar Express, arkasında yapmak yerine, doğrudan, uygulama çalıştırdığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-133">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="6cdd0-134">Çalıştırma [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) bir komut isteminden veya Visual Studio araç çubuğundaki hata ayıklama hedefi açılır uygulama adı profili seçin.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-134">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="6cdd0-135">Yapılacaklar öğelerini göstermek için bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-135">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="6cdd0-136">İşareti gerekli alanlarını kullanarak `[Required]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-136">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="6cdd0-137">API yöntemlerini verilerle çalışmak için bazı yol gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-137">The API methods require some way to work with data.</span></span> <span data-ttu-id="6cdd0-138">Aynı `IToDoRepository` arabirim özgün Xamarin örnek kullanır:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-138">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="6cdd0-139">Bu örnek için uygulama, yalnızca öğeleri özel koleksiyonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-139">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="6cdd0-140">Uygulamasında yapılandırma *haline*:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-140">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="6cdd0-141">Bu noktada, oluşturmak için hazır *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-141">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="6cdd0-142">Oluşturma hakkında daha fazla web API'leri öğrenin [yapı bilgisayarınızı ilk Web API ile ASP.NET Core MVC ve Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6cdd0-142">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="6cdd0-143">Denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="6cdd0-143">Creating the Controller</span></span>

<span data-ttu-id="6cdd0-144">Yeni bir denetleyici projeye ekleyin *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-144">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="6cdd0-145">Microsoft.AspNetCore.Mvc.Controller devralan.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-145">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="6cdd0-146">Ekleme bir `Route` denetleyicisi ile başlayan yollar için yapılan istekleri işlemesini belirtmek için öznitelik `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-146">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="6cdd0-147">`[controller]` Belirteci rotadaki denetleyici adıyla değiştirilir (atlama `Controller` soneki) ve genel yollar için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-147">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="6cdd0-148">Daha fazla bilgi edinmek [yönlendirme](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="6cdd0-148">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="6cdd0-149">Denetleyici gerektiren bir `IToDoRepository` için işlev; denetleyicinin Oluşturucusu aracılığıyla bu türünün bir örneği isteği.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-149">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="6cdd0-150">Framework'ün desteğini kullanarak bu örnek çalışma zamanında sağlanacak [bağımlılık ekleme](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6cdd0-150">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="6cdd0-151">Bu API veri kaynağında (oluşturma, okuma, güncelleştirme, silme) CRUD işlemleri gerçekleştirmek için dört farklı HTTP fiilleri destekler.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-151">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="6cdd0-152">Bu bir HTTP GET isteği karşılık gelen okuma işlemi en kolayıdır.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-152">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="6cdd0-153">Öğeleri okuma</span><span class="sxs-lookup"><span data-stu-id="6cdd0-153">Reading Items</span></span>

<span data-ttu-id="6cdd0-154">Bir öğe listesi isteyen bir GET isteğine ile yapılır `List` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-154">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="6cdd0-155">`[HttpGet]` Özniteliği `List` yöntemi gösterir Bu eylem yalnızca GET isteklerini işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-155">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="6cdd0-156">Bu eylem için yol denetleyicisinde belirtilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-156">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="6cdd0-157">Mutlaka rota bir parçası olarak eylem adı kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-157">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="6cdd0-158">Yalnızca benzersiz ve anlaşılır bir rota her bir eylem içerdiğinden emin olmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-158">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="6cdd0-159">Yönlendirme öznitelikleri, denetleyici ve belirli rotaları oluşturmak için yöntem düzeylerinde uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-159">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="6cdd0-160">`List` Yöntemi 200 Tamam yanıt kodu ve tüm JSON olarak serileştirilen Yapılacaklar öğelerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-160">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="6cdd0-161">Yeni API yönteminizi gibi çeşitli araçları, kullanarak test edebilirsiniz [Postman](https://www.getpostman.com/docs/), burada gösterilen:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-161">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Todoıtems ve döndürülen üç öğeleri için JSON gösteren yanıt gövdesi için bir GET isteği gösteren postman konsol](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="6cdd0-163">Öğeleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6cdd0-163">Creating Items</span></span>

<span data-ttu-id="6cdd0-164">Kurala göre yeni veri öğeleri oluşturmak için HTTP POST fiil eşlenir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-164">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="6cdd0-165">`Create` Yöntemi sahip bir `[HttpPost]` özniteliği uygulanan ve kabul eden bir `ToDoItem` örneği.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-165">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="6cdd0-166">Bu yana `item` bağımsız değişken POST gövdesinde geçirilecektir, bu parametre ile donatılmış `[FromBody]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-166">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="6cdd0-167">Yöntemi içinde öğe geçerlilik ve veri deposunda önceki varlığı için denetlenir ve herhangi bir sorun oluşursa, depo kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-167">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="6cdd0-168">Denetimi `ModelState.IsValid` gerçekleştirir [model doğrulama](../mvc/models/validation.md)ve kullanıcı girişini kabul etme her API yönteminde yapılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-168">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="6cdd0-169">Örnek mobil istemciye geçirilen hata kodları içeren bir enum kullanır:</span><span class="sxs-lookup"><span data-stu-id="6cdd0-169">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="6cdd0-170">Postman yeni nesnesi istek gövdesinde JSON biçiminde sağlama sonrası fiil seçerek yeni öğeler eklemek sınayın.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-170">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="6cdd0-171">Belirten bir istek üstbilgisi de eklemeniz gerekir bir `Content-Type` , `application/json`.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-171">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![POST ve yanıt gösteren postman konsol](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="6cdd0-173">Yöntemi yeni oluşturulan öğeyi yanıt olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-173">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="6cdd0-174">Öğeler güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="6cdd0-174">Updating Items</span></span>

<span data-ttu-id="6cdd0-175">Kayıtları değiştirme yapılır HTTP PUT isteklerini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-175">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="6cdd0-176">Bu değişiklik dışında `Edit` yöntemdir neredeyse aynı `Create`.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-176">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="6cdd0-177">Kaydı bulunmazsa unutmayın, `Edit` eylem döndürecektir bir `NotFound` (404) yanıt.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-177">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="6cdd0-178">Postman ile test etmek için PUT fiili değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-178">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="6cdd0-179">Güncelleştirilmiş nesne verilerini istek gövdesinde belirtin.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-179">Specify the updated object data in the Body of the request.</span></span>

![PUT ve yanıt gösteren postman konsol](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="6cdd0-181">Bu yöntem bir `NoContent` başarılı olduğunda, önceden varolan API ile tutarlılığını (204) yanıt.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-181">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="6cdd0-182">Öğeleri silme</span><span class="sxs-lookup"><span data-stu-id="6cdd0-182">Deleting Items</span></span>

<span data-ttu-id="6cdd0-183">Kayıtları silme silme isteklerinin hizmete yapma ve silinecek öğe kimliği geçirme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-183">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="6cdd0-184">Güncelleştirme ile var olmayan öğeler için istekleri alacak şekilde `NotFound` yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-184">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="6cdd0-185">Aksi takdirde, başarılı bir istek alırsınız bir `NoContent` (204) yanıt.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-185">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="6cdd0-186">Delete işlevselliğini test etme, hiçbir şey istek gövdesinde gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-186">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Bir silme ve yanıt gösteren postman konsol](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="6cdd0-188">Ortak Web API kuralları</span><span class="sxs-lookup"><span data-stu-id="6cdd0-188">Common Web API Conventions</span></span>

<span data-ttu-id="6cdd0-189">Uygulamanız için arka uç hizmetlerini geliştirirken, kurallar veya çapraz kesme sorunları işlenmesine yönelik ilkeler tutarlı kümesiyle gündeme isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-189">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="6cdd0-190">Örneğin, yukarıda gösterilen hizmetinde alınan bulunamadı belirli kayıtlarını ister bir `NotFound` yanıtı yerine bir `BadRequest` yanıt.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-190">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="6cdd0-191">Benzer şekilde, her zaman kullanıma modele bağlı türlerinde geçirilen bu hizmete yapılan komutları `ModelState.IsValid` ve döndürülen bir `BadRequest` geçersiz model türü için.</span><span class="sxs-lookup"><span data-stu-id="6cdd0-191">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="6cdd0-192">Apı'leriniz için ortak bir ilke tanımladıktan sonra genellikle içinde sarmalayabilen bir [filtre](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="6cdd0-192">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="6cdd0-193">Daha fazla bilgi edinmek [ASP.NET Core MVC uygulamalarında ortak API ilkelerini kapsülleyen nasıl](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="6cdd0-193">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
