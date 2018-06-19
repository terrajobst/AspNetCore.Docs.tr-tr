---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: JavaScript istemci oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879315"
---
<a name="create-the-javascript-client"></a><span data-ttu-id="c19e9-102">JavaScript istemci oluşturma</span><span class="sxs-lookup"><span data-stu-id="c19e9-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="c19e9-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c19e9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c19e9-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="c19e9-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c19e9-105">Bu bölümde, HTML, JavaScript kullanılarak istemci uygulaması oluşturacak ve [Knockout.js](http://knockoutjs.com/) kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="c19e9-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="c19e9-106">Biz aşamada istemci uygulaması oluşturma:</span><span class="sxs-lookup"><span data-stu-id="c19e9-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="c19e9-107">Kitap listesi gösteriliyor.</span><span class="sxs-lookup"><span data-stu-id="c19e9-107">Showing a list of books.</span></span>
- <span data-ttu-id="c19e9-108">Defteri ayrıntısı gösteriliyor.</span><span class="sxs-lookup"><span data-stu-id="c19e9-108">Showing a book detail.</span></span>
- <span data-ttu-id="c19e9-109">Yeni bir rehberi ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="c19e9-109">Adding a new book.</span></span>

<span data-ttu-id="c19e9-110">Boşaltılan kitaplığı Model View ViewModel (MVVM) deseni kullanır:</span><span class="sxs-lookup"><span data-stu-id="c19e9-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="c19e9-111">**Modeli** iş etki alanında (bizim çalışması, books ve yazarlar) verileri sunucu tarafı gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="c19e9-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="c19e9-112">**Görünüm** sunu (HTML) katmanıdır.</span><span class="sxs-lookup"><span data-stu-id="c19e9-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="c19e9-113">**Görünüm modeli** modelleri tutan bir JavaScript nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="c19e9-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="c19e9-114">Görünüm modeli UI kod soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="c19e9-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="c19e9-115">HTML temsilini olanağıyla sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c19e9-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="c19e9-116">Bunun yerine, görünümün soyut özellikler gibi temsil ettiği &quot;books listesini&quot;.</span><span class="sxs-lookup"><span data-stu-id="c19e9-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="c19e9-117">Görünüm verilere görünüm modeline bağlı.</span><span class="sxs-lookup"><span data-stu-id="c19e9-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="c19e9-118">Görünüm modeli güncelleştirmeleri otomatik olarak görünümünde yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="c19e9-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="c19e9-119">Düğmesine tıklar gibi görünüm modeli olayları da görünümden alır.</span><span class="sxs-lookup"><span data-stu-id="c19e9-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="c19e9-120">Herhangi bir kod yeniden yazma işlemi olmadan bağlamaları değişebileceği için bu yaklaşım, uygulamanızın kullanıcı Arabirimi ve düzenini değiştirmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="c19e9-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="c19e9-121">Örneğin, bir öğe listesi gösterebilir. bir `<ul>`, tabloya daha sonra değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c19e9-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="c19e9-122">Boşaltılan kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="c19e9-122">Add the Knockout Library</span></span>

<span data-ttu-id="c19e9-123">Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="c19e9-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="c19e9-124">Ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="c19e9-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="c19e9-125">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="c19e9-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="c19e9-126">Bu komut Boşaltılan dosyaları betikler klasörüne ekler.</span><span class="sxs-lookup"><span data-stu-id="c19e9-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="c19e9-127">Görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="c19e9-127">Create the View Model</span></span>

<span data-ttu-id="c19e9-128">Betikler klasörüne App.js adlı bir JavaScript dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c19e9-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="c19e9-129">(Çözüm Gezgini'nde betikler klasörüne sağ tıklayın, **Ekle**seçeneğini belirleyip **JavaScript dosyası**.) Aşağıdaki kodu yapıştırın:</span><span class="sxs-lookup"><span data-stu-id="c19e9-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="c19e9-130">Boşaltılan içinde `observable` veri bağlama sınıfı sağlar.</span><span class="sxs-lookup"><span data-stu-id="c19e9-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="c19e9-131">Observable içeriğini değiştirdiğinizde, kendilerini güncelleştirecek şekilde observable tüm verilere bağlı denetimler bildirir.</span><span class="sxs-lookup"><span data-stu-id="c19e9-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="c19e9-132">( `observableArray` Sınıftır dizi sürümü *observable*.) İle başlamak iki tane observable bizim görünüm modeli vardır:</span><span class="sxs-lookup"><span data-stu-id="c19e9-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="c19e9-133">`books` books listesini tutar.</span><span class="sxs-lookup"><span data-stu-id="c19e9-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="c19e9-134">`error` AJAX çağrısı başarısız olursa bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="c19e9-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="c19e9-135">`getAllBooks` Yöntemi books listesini almak için bir AJAX çağrısı yapar.</span><span class="sxs-lookup"><span data-stu-id="c19e9-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="c19e9-136">Sonucun üzerine iter sonra `books` dizi.</span><span class="sxs-lookup"><span data-stu-id="c19e9-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="c19e9-137">`ko.applyBindings` Yöntemi Boşaltılan kitaplığı bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="c19e9-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="c19e9-138">Görünüm modeli parametre olarak alır ve veri bağlamanın ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c19e9-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="c19e9-139">Bir komut dosyası paket ekleme</span><span class="sxs-lookup"><span data-stu-id="c19e9-139">Add a Script Bundle</span></span>

<span data-ttu-id="c19e9-140">Paketleme, birleştirebilir veya tek bir dosyaya birden çok dosya paketini daha kolay hale getirir ASP.NET 4.5 özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="c19e9-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="c19e9-141">Paketleme sayfa yükleme süresini artırabilir sunucu isteklerinin sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="c19e9-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="c19e9-142">Uygulama dosyasını açın\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="c19e9-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="c19e9-143">RegisterBundles yöntemine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c19e9-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="c19e9-144">[Önceki](part-5.md)
> [sonraki](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="c19e9-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
