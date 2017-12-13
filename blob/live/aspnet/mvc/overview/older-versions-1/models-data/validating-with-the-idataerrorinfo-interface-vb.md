---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: "IDataErrorInfo arabirimi (VB) ile doğrulanıyor | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther bir model sınıfı IDataErrorInfo arabirimi uygulayarak özel doğrulama hata iletilerinin görüntülenip gösterilmiştir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 1439d470a7fa3cb1171dbdd0b7eec6a6aa52912d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="572e8-103">IDataErrorInfo arabirimi (VB) ile doğrulama</span><span class="sxs-lookup"><span data-stu-id="572e8-103">Validating with the IDataErrorInfo Interface (VB)</span></span>
====================
<span data-ttu-id="572e8-104">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="572e8-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="572e8-105">Stephen Walther bir model sınıfı IDataErrorInfo arabirimi uygulayarak özel doğrulama hata iletilerinin görüntülenip gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="572e8-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="572e8-106">Bu öğretici bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için bir yaklaşım açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="572e8-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="572e8-107">Birisi gerekli form alanları için değerleri sağlamadan bir HTML formuna göndermelerini engellemek öğrenin.</span><span class="sxs-lookup"><span data-stu-id="572e8-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="572e8-108">Bu öğreticide, IErrorDataInfo arabirimini kullanarak doğrulama gerçekleştirmek nasıl öğrenin.</span><span class="sxs-lookup"><span data-stu-id="572e8-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="572e8-109">Varsayımlar</span><span class="sxs-lookup"><span data-stu-id="572e8-109">Assumptions</span></span>

<span data-ttu-id="572e8-110">Bu öğreticide, MoviesDB veritabanı ile filmler veritabanı tablosu kullanmam.</span><span class="sxs-lookup"><span data-stu-id="572e8-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="572e8-111">Bu tablo şu sütunları vardır:</span><span class="sxs-lookup"><span data-stu-id="572e8-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>


| <span data-ttu-id="572e8-112">**Sütun adı**</span><span class="sxs-lookup"><span data-stu-id="572e8-112">**Column Name**</span></span> | <span data-ttu-id="572e8-113">**Veri türü**</span><span class="sxs-lookup"><span data-stu-id="572e8-113">**Data Type**</span></span> | <span data-ttu-id="572e8-114">**Null değerlere izin ver**</span><span class="sxs-lookup"><span data-stu-id="572e8-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="572e8-115">Kimliği</span><span class="sxs-lookup"><span data-stu-id="572e8-115">Id</span></span> | <span data-ttu-id="572e8-116">int</span><span class="sxs-lookup"><span data-stu-id="572e8-116">Int</span></span> | <span data-ttu-id="572e8-117">False</span><span class="sxs-lookup"><span data-stu-id="572e8-117">False</span></span> |
| <span data-ttu-id="572e8-118">Başlık</span><span class="sxs-lookup"><span data-stu-id="572e8-118">Title</span></span> | <span data-ttu-id="572e8-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="572e8-119">Nvarchar(100)</span></span> | <span data-ttu-id="572e8-120">False</span><span class="sxs-lookup"><span data-stu-id="572e8-120">False</span></span> |
| <span data-ttu-id="572e8-121">Director</span><span class="sxs-lookup"><span data-stu-id="572e8-121">Director</span></span> | <span data-ttu-id="572e8-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="572e8-122">Nvarchar(100)</span></span> | <span data-ttu-id="572e8-123">False</span><span class="sxs-lookup"><span data-stu-id="572e8-123">False</span></span> |
| <span data-ttu-id="572e8-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="572e8-124">DateReleased</span></span> | <span data-ttu-id="572e8-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="572e8-125">DateTime</span></span> | <span data-ttu-id="572e8-126">False</span><span class="sxs-lookup"><span data-stu-id="572e8-126">False</span></span> |


<span data-ttu-id="572e8-127">Bu öğreticide, ı my veritabanı modeli sınıfları oluşturmak için Microsoft Entity Framework kullanın.</span><span class="sxs-lookup"><span data-stu-id="572e8-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="572e8-128">Entity Framework tarafından oluşturulan film sınıfı Şekil 1'de görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="572e8-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="572e8-129">[![Film varlık](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="572e8-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="572e8-130">**Şekil 01**: film varlık ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="572e8-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="572e8-131">Veritabanı modeli sınıfları oluşturmak için Entity Framework kullanma hakkında daha fazla bilgi için Model sınıflarıyla oluşturma Entity Framework my öğretici başlıklı bakın.</span><span class="sxs-lookup"><span data-stu-id="572e8-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="572e8-132">Denetleyici sınıfı</span><span class="sxs-lookup"><span data-stu-id="572e8-132">The Controller Class</span></span>

<span data-ttu-id="572e8-133">Biz listesi filmler giriş denetleyiciye kullanın ve yeni filmler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="572e8-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="572e8-134">Bu sınıf için kod listeleme 1'de yer alır.</span><span class="sxs-lookup"><span data-stu-id="572e8-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="572e8-135">**1 - Controllers\HomeController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="572e8-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="572e8-136">Listeleme 1 giriş controller sınıfında iki Create() eylemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="572e8-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="572e8-137">İlk eylem yeni film oluşturmaya için HTML formu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="572e8-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="572e8-138">İkinci Create() eylemi veritabanına yeni film gerçek INSERT gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="572e8-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="572e8-139">İkinci Create() eylemi sunucuya ilk Create() eylem tarafından görüntülenen form gönderildiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="572e8-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="572e8-140">İkinci Create() eylemi aşağıdaki kod satırlarını içerdiğine dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="572e8-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="572e8-141">Bir doğrulama hatası olduğunda IsValid özelliği false değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="572e8-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="572e8-142">Bu durumda, bir filmi oluşturmak için HTML formu içeren Oluştur görünümünün görünürler.</span><span class="sxs-lookup"><span data-stu-id="572e8-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="572e8-143">Kısmi bir sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="572e8-143">Creating a Partial Class</span></span>

<span data-ttu-id="572e8-144">Film sınıf Entity Framework tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="572e8-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="572e8-145">Çözüm Gezgini penceresinde MoviesDBModel.edmx dosyasını genişletin ve kod düzenleyicisinde MoviesDBModel.Designer.vb dosyasını açın, film sınıfı için kod görebilirsiniz (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="572e8-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="572e8-146">[![Film varlık için kod](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="572e8-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="572e8-147">**Şekil 02**: film varlık için kod ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="572e8-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>


<span data-ttu-id="572e8-148">Film sınıfı kısmi bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="572e8-148">The Movie class is a partial class.</span></span> <span data-ttu-id="572e8-149">Film sınıf işlevselliğini genişletmek için aynı ada sahip başka bir parçalı sınıf ekleyebiliriz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="572e8-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="572e8-150">Yeni sınıfa doğrulama mantığımızı ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="572e8-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="572e8-151">Sınıf modeller klasörü listeleme 2'de ekleyin.</span><span class="sxs-lookup"><span data-stu-id="572e8-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="572e8-152">**2 - Models\Movie.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="572e8-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="572e8-153">Listeleme 2 sınıfında içeren bildirim *kısmi* değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="572e8-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="572e8-154">Herhangi bir yöntemi veya bu sınıfa ekleyin özellikleri Entity Framework tarafından oluşturulan film sınıfı bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="572e8-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="572e8-155">OnChanging ve OnChanged kısmi yöntemler ekleme</span><span class="sxs-lookup"><span data-stu-id="572e8-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="572e8-156">Entity Framework kısmi yöntemler sınıfa Entity Framework bir varlık sınıfı oluşturduğunda, otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="572e8-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="572e8-157">Entity Framework sınıfın her bir özellik için karşılık gelen OnChanging ve OnChanged kısmi yöntemler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="572e8-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="572e8-158">Film sınıfı söz konusu olduğunda, aşağıdaki yöntemlerden Entity Framework oluşturur:</span><span class="sxs-lookup"><span data-stu-id="572e8-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="572e8-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="572e8-159">OnIdChanging</span></span>
- <span data-ttu-id="572e8-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="572e8-160">OnIdChanged</span></span>
- <span data-ttu-id="572e8-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="572e8-161">OnTitleChanging</span></span>
- <span data-ttu-id="572e8-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="572e8-162">OnTitleChanged</span></span>
- <span data-ttu-id="572e8-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="572e8-163">OnDirectorChanging</span></span>
- <span data-ttu-id="572e8-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="572e8-164">OnDirectorChanged</span></span>
- <span data-ttu-id="572e8-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="572e8-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="572e8-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="572e8-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="572e8-167">Karşılık gelen özelliği değiştirilmeden önce OnChanging yöntemi sağ çağrılır.</span><span class="sxs-lookup"><span data-stu-id="572e8-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="572e8-168">Özellik değiştirildikten sonra OnChanged yöntemi sağ çağrılır.</span><span class="sxs-lookup"><span data-stu-id="572e8-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="572e8-169">Doğrulama mantığını film sınıfı eklemek için bu kısmi yöntemlerin avantajından yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="572e8-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="572e8-170">Güncelleştirme film sınıfı listeleme 3 başlık ve Director özellikleri boş olmayan değerler atanır doğrular.</span><span class="sxs-lookup"><span data-stu-id="572e8-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="572e8-171">Kısmi bir yöntem uygulamak için gerekli olmayan bir sınıf içinde tanımlanan bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="572e8-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="572e8-172">Kısmi bir yöntem uygularsanız yok derleyici yöntem imzası kaldırır ve tüm çağrıları yöntemi yani vardır kısmi yöntemiyle ilişkili çalışma zamanı maliyetleri olmadan aynıdır.</span><span class="sxs-lookup"><span data-stu-id="572e8-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="572e8-173">Visual Studio Kod Düzenleyicisi'nde anahtar sözcüğü yazarak kısmi bir yöntem ekleyebilirsiniz *kısmi* uygulamak için kısmi bir listesini görüntülemek için bir boşluk bırakarak.</span><span class="sxs-lookup"><span data-stu-id="572e8-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="572e8-174">**3 - Models\Movie.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="572e8-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="572e8-175">Başlık özellik boş bir dize atama çalışırsanız, örneğin, daha sonra bir hata iletisi adlı bir sözlük atanır \_hataları.</span><span class="sxs-lookup"><span data-stu-id="572e8-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="572e8-176">Bu noktada, hiçbir şey gerçekte başlık özelliği boş bir dize atamak ve bir hata özel eklendiğinde gerçekleşir \_hataları alan.</span><span class="sxs-lookup"><span data-stu-id="572e8-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="572e8-177">ASP.NET MVC çerçevesi bu doğrulama hatalarını kullanıma sunmak için IDataErrorInfo arabirimi uygulamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="572e8-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="572e8-178">IDataErrorInfo arabirimi uygulama</span><span class="sxs-lookup"><span data-stu-id="572e8-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="572e8-179">IDataErrorInfo arabirimi ilk sürümünden bu yana .NET framework'ün bir parçası olmuştur.</span><span class="sxs-lookup"><span data-stu-id="572e8-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="572e8-180">Bu arabirim çok basit bir arabirimdir:</span><span class="sxs-lookup"><span data-stu-id="572e8-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="572e8-181">Bir sınıf IDataErrorInfo arabirimi uygularsa, ASP.NET MVC çerçevesi sınıfının bir örneğini oluştururken bu arabirimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="572e8-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="572e8-182">Örneğin, ev denetleyicisi Create() eylem film sınıfının bir örneği kabul eder:</span><span class="sxs-lookup"><span data-stu-id="572e8-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="572e8-183">ASP.NET MVC çerçevesi model bağlayıcı (DefaultModelBinder) kullanarak Create() eyleme geçirilen film örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="572e8-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="572e8-184">Model bağlayıcı Film nesnesini örneğine HTML form alanlarını bağlayarak film nesnesinin örneğini oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="572e8-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="572e8-185">Bir sınıf IDataErrorInfo arabirimi uygulayan olup olmadığına bakılmaksızın DefaultModelBinder algılar.</span><span class="sxs-lookup"><span data-stu-id="572e8-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="572e8-186">Bir sınıfı bu arabirimi uygular, model bağlayıcı sınıfın her bir özellik için IDataErrorInfo.this dizin oluşturucuyu çağırır.</span><span class="sxs-lookup"><span data-stu-id="572e8-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="572e8-187">Dizin Oluşturucu bir hata iletisi döndürürse, model bağlayıcı durumu otomatik olarak modellemek için bu hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="572e8-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="572e8-188">DefaultModelBinder de IDataErrorInfo.Error özelliğini denetler.</span><span class="sxs-lookup"><span data-stu-id="572e8-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="572e8-189">Bu özellik ile ilişkili özelliği olmayan belirli doğrulama hatalarını temsil etmek üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="572e8-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="572e8-190">Örneğin, film sınıfın birden çok özelliklerin değerlerine bağlıdır bir doğrulama kuralı zorlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="572e8-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="572e8-191">Bu durumda, hata özelliğinden bir doğrulama hata döndürecektir.</span><span class="sxs-lookup"><span data-stu-id="572e8-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="572e8-192">Listeleme 4 güncelleştirilmiş film sınıfında IDataErrorInfo arabirimi uygular.</span><span class="sxs-lookup"><span data-stu-id="572e8-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="572e8-193">**4 - Models\Movie.vb (IDataErrorInfo uygulayan) listeleme**</span><span class="sxs-lookup"><span data-stu-id="572e8-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="572e8-194">Dizin Oluşturucu özelliği listeleme 4'te denetler \_özellik adına karşılık gelen bir anahtarı içerip içermediğini görmek için hatalar koleksiyonuna geçirilen için dizin oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="572e8-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="572e8-195">Özellik ile ilişkilendirilmiş doğrulama hata yoksa boş bir dize döndürülür.</span><span class="sxs-lookup"><span data-stu-id="572e8-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="572e8-196">Giriş denetleyicisi değiştirilmiş film sınıfını kullanmak için herhangi bir şekilde değiştirmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="572e8-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="572e8-197">Şekil 3'te görüntülenen sayfa başlığının ya da Director form alanları için herhangi bir değer girdiğinizde ne olacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="572e8-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="572e8-198">[![Eylem yöntemleri otomatik olarak oluşturma](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="572e8-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="572e8-199">**Şekil 03**: eksik değerleri bir formla ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="572e8-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>


<span data-ttu-id="572e8-200">DateReleased değer otomatik olarak doğrulanır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="572e8-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="572e8-201">Bir değere sahip olmadığı durumlarda DefaultModelBinder, bu özellik için bir doğrulama hatası DateReleased özelliği NULL değerleri kabul etmiyor olduğundan, otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="572e8-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="572e8-202">DateReleased özelliği için hata iletisini değiştirmek istiyorsanız özel bir model bağlayıcısını oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="572e8-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="572e8-203">Özet</span><span class="sxs-lookup"><span data-stu-id="572e8-203">Summary</span></span>

<span data-ttu-id="572e8-204">Bu öğreticide, IDataErrorInfo arabirimi doğrulama hata iletileri oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="572e8-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="572e8-205">İlk olarak, Entity Framework tarafından oluşturulan kısmi film sınıf işlevselliğini genişleten bir kısmi film sınıfı oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="572e8-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="572e8-206">Ardından, doğrulama mantığını film sınıfı OnTitleChanging() ve OnDirectorChanging() kısmi yöntemlerine eklediğimiz.</span><span class="sxs-lookup"><span data-stu-id="572e8-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="572e8-207">Son olarak, biz bu doğrulama iletileri ASP.NET MVC çerçevesi için kullanıma sunmak için IDataErrorInfo arabirimi uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="572e8-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="572e8-208">[Önceki](performing-simple-validation-vb.md)
[sonraki](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="572e8-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
