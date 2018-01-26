---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Bir denetleyici ekleme | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: c8f317b2ac133f560461917af1588b7a1fa51c4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-controller"></a><span data-ttu-id="5184b-102">Bir denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="5184b-102">Adding a Controller</span></span>
====================
<span data-ttu-id="5184b-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5184b-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="5184b-104">MVC anlamına gelir *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="5184b-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="5184b-105">MVC, iyi tasarlanmış, test edilebilir ve bakımını kolay uygulamaları geliştirmek için bir desen olur.</span><span class="sxs-lookup"><span data-stu-id="5184b-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="5184b-106">MVC tabanlı uygulamalar içerir:</span><span class="sxs-lookup"><span data-stu-id="5184b-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="5184b-107">**M** odels: uygulama verilerini temsil eden ve bu verilere iş kurallarını zorunlu tutmak için doğrulama mantığını kullanan sınıfları.</span><span class="sxs-lookup"><span data-stu-id="5184b-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="5184b-108">**V** iews: dinamik olarak HTML yanıtları oluşturmak için uygulamanızın kullandığı şablon dosyalarını.</span><span class="sxs-lookup"><span data-stu-id="5184b-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="5184b-109">**C** ontrollers: gelen tarayıcı istekleri işleyen sınıflar model verileri almak ve tarayıcıya bir yanıt döndürür görünüm şablonları belirtin.</span><span class="sxs-lookup"><span data-stu-id="5184b-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="5184b-110">Biz, Bu öğretici serisinde, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="5184b-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="5184b-111">Önceki adımda varsayılan MVC şablon seçilmedi.</span><span class="sxs-lookup"><span data-stu-id="5184b-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="5184b-112">Bu giriş, hesabı ve varsayılan olarak Denetleyicilerini Yönet oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5184b-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="5184b-113">Denetleyici sınıfı oluşturarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="5184b-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="5184b-114">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **Ekle**, ardından **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="5184b-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="5184b-115">İçinde **İskele Ekle** iletişim kutusu, tıklatın **MVC 5 denetleyici - boş**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5184b-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="5184b-116">Yeni denetleyicinize "HelloWorldController" olarak adlandırın ve tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5184b-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Denetleyici ekleme](adding-a-controller/_static/image3.png)

<span data-ttu-id="5184b-118">Fark **Çözüm Gezgini** yeni bir dosya adlandırılmış oluşturulduğunu *HelloWorldController.cs* ve yeni bir klasör *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="5184b-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="5184b-119">IDE denetleyicisi açıktır.</span><span class="sxs-lookup"><span data-stu-id="5184b-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="5184b-120">Dosya içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5184b-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="5184b-121">Denetleyici yöntemleri HTML dizesi örnek olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="5184b-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="5184b-122">Denetleyici adlı `HelloWorldController` ve ilk yöntem adlı `Index`.</span><span class="sxs-lookup"><span data-stu-id="5184b-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="5184b-123">Şimdi bir tarayıcıdan çağırır.</span><span class="sxs-lookup"><span data-stu-id="5184b-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="5184b-124">Uygulama (F5 veya Ctrl + F5'e basın) çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5184b-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="5184b-125">Tarayıcıda append &quot;HelloWorld&quot; adres çubuğundaki yolu.</span><span class="sxs-lookup"><span data-stu-id="5184b-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="5184b-126">(Örneğin, aşağıdaki, çizimdeki `http://localhost:1234/HelloWorld.`) sayfasını tarayıcıda aşağıdaki ekran görüntüsüne benzeyen arar.</span><span class="sxs-lookup"><span data-stu-id="5184b-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="5184b-127">Yukarıdaki yöntemi kodu doğrudan bir dize döndürdü.</span><span class="sxs-lookup"><span data-stu-id="5184b-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="5184b-128">Yalnızca bazı HTML döndürmek için sistem söylediyse ve olduğu!</span><span class="sxs-lookup"><span data-stu-id="5184b-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="5184b-129">ASP.NET MVC, gelen URL bağlı olarak farklı bir denetleyici sınıfları (ve bunların içindeki farklı eylem yöntemleri) çağırır.</span><span class="sxs-lookup"><span data-stu-id="5184b-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="5184b-130">ASP.NET MVC tarafından kullanılan varsayılan URL yönlendirme mantığı çağırmak için kodu nedir belirlemek için bu gibi bir biçim kullanır:</span><span class="sxs-lookup"><span data-stu-id="5184b-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="5184b-131">Yönlendirme için biçimini ayarlama *uygulama\_Start/RouteConfig.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="5184b-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="5184b-132">Uygulamayı çalıştırın ve tüm URL kesimleri kaynağı yok, "Home" denetleyiciye Varsayılanları ve yukarıdaki kod Varsayılanları bölümünde "Dizin" eylem yöntemi belirtilen.</span><span class="sxs-lookup"><span data-stu-id="5184b-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="5184b-133">URL ilk bölümü yürütmek için denetleyici sınıfını belirler.</span><span class="sxs-lookup"><span data-stu-id="5184b-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="5184b-134">Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5184b-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="5184b-135">URL ikinci bölümü eylem yöntemine yürütmek için sınıf belirler.</span><span class="sxs-lookup"><span data-stu-id="5184b-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="5184b-136">Bu nedenle */HelloWorld/dizin* neden olacağından `Index` yöntemi `HelloWorldController` yürütmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="5184b-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="5184b-137">Biz yalnızca Gözat gerekiyordu bildirimi */HelloWorld* ve `Index` yöntemi, varsayılan olarak kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="5184b-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="5184b-138">Bir yöntem adlı olmasıdır `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak varsayılan yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="5184b-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="5184b-139">URL kesimi üçüncü parçası ( `Parameters`) için rota veri.</span><span class="sxs-lookup"><span data-stu-id="5184b-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="5184b-140">Bu öğreticide rota verilerini daha sonra göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="5184b-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="5184b-141">Gözat `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="5184b-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="5184b-142">`Welcome` Yöntemi çalışır ve bir dize döndürür &quot;Hoş Geldiniz eylem yöntem budur... &quot;.</span><span class="sxs-lookup"><span data-stu-id="5184b-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="5184b-143">Varsayılan MVC eşlemesi `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="5184b-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="5184b-144">Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="5184b-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="5184b-145">Kullandığınız parolalardan `[Parameters]` URL henüz parçası.</span><span class="sxs-lookup"><span data-stu-id="5184b-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="5184b-146">Böylece denetleyiciye URL'den bazı parametre bilgilerini geçirebilirsiniz örnek biraz değiştirelim (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="5184b-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="5184b-147">Değişiklik, `Welcome` yöntemi iki parametre aşağıda gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5184b-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="5184b-148">Kod belirtmek için C# isteğe bağlı parametre özelliği kullandığına dikkat edin `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.</span><span class="sxs-lookup"><span data-stu-id="5184b-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="5184b-149">Güvenlik Notu: Kullanır yukarıdaki kodu [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) kötü amaçlı giriş (yani JavaScript) uygulama korumak için.</span><span class="sxs-lookup"><span data-stu-id="5184b-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="5184b-150">Daha fazla bilgi için bkz: [nasıl yapılır: koruma karşı betik yararlanan bir Web uygulamasında HTML kodlaması uygulama tarafından dizelere](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="5184b-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="5184b-151">Uygulamanızı çalıştırın ve örnek URL'sine gidin (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="5184b-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="5184b-152">İçin farklı değerler deneyin `name` ve `numtimes` URL.</span><span class="sxs-lookup"><span data-stu-id="5184b-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="5184b-153">[ASP.NET MVC model bağlama sistem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) adres çubuğundaki sorgu dizesi yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşlenir.</span><span class="sxs-lookup"><span data-stu-id="5184b-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="5184b-154">URL kesimi Yukarıdaki örnekteki ( `Parameters`) kullanılmaz, `name` ve `numTimes` parametre olarak geçirilir [sorgu dizeleri](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="5184b-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="5184b-155">?</span><span class="sxs-lookup"><span data-stu-id="5184b-155">The ?</span></span> <span data-ttu-id="5184b-156">(soru işareti) yukarıdaki URL'de bir ayırıcı olduğu ve sorgu dizeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="5184b-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="5184b-157">&amp; Karakter sorgu dizeleri ayırır.</span><span class="sxs-lookup"><span data-stu-id="5184b-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="5184b-158">Hoş Geldiniz yöntemini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="5184b-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="5184b-159">Uygulamayı çalıştırın ve aşağıdaki URL'yi girin:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="5184b-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="5184b-160">Bu süre üçüncü URL kesimi eşleşen rota parametresi `ID.` `Welcome` eylem yöntemini içeren bir parametre (`ID`) URL belirtiminde eşleşen `RegisterRoutes` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5184b-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="5184b-161">ASP.NET MVC uygulamalarında, bu parametre olarak sorgu dizeleri geçirme daha (yukarıdaki Kimliğiyle yaptığımız gibi) rota verilerini geçirmek için daha genel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="5184b-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="5184b-162">Her ikisi de geçirmek için bir yol da ekleyebilirsiniz `name` ve `numtimes` parametrelerinde URL rota veri olarak.</span><span class="sxs-lookup"><span data-stu-id="5184b-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="5184b-163">İçinde *uygulama\_Start\RouteConfig.cs* dosya, "Hello" yolu Ekle:</span><span class="sxs-lookup"><span data-stu-id="5184b-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="5184b-164">Uygulamayı çalıştırın ve Gözat `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="5184b-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="5184b-165">Birçok MVC uygulamaları için varsayılan yolu düzgün çalışır.</span><span class="sxs-lookup"><span data-stu-id="5184b-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="5184b-166">Model bağlayıcı kullanılarak veri iletmek için daha sonra Bu öğreticide öğreneceksiniz ve varsayılan rota için değiştirmek zorunda kalmazsınız.</span><span class="sxs-lookup"><span data-stu-id="5184b-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="5184b-167">Bu örneklerde denetleyicisi bulunurken &quot;VC&quot; MVC kısmı — diğer bir deyişle, Görünüm ve denetleyici çalışma.</span><span class="sxs-lookup"><span data-stu-id="5184b-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="5184b-168">Denetleyici HTML doğrudan döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="5184b-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="5184b-169">Normalde kodu çok kullanışsız hale beri HTML doğrudan döndürerek denetleyicileri istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="5184b-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="5184b-170">Bunun yerine genellikle ayrı görünüm şablon dosyası HTML yanıtı oluşturmak yardımcı olmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="5184b-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="5184b-171">Nasıl biz bunu yapmak için sonraki olarak bakalım.</span><span class="sxs-lookup"><span data-stu-id="5184b-171">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5184b-172">[Önceki](getting-started.md)
[sonraki](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="5184b-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
