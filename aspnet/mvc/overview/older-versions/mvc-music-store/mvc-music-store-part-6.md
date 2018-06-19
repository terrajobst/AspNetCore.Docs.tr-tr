---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Bölüm 6: Veri ek açıklamaları Model doğrulama için kullanarak | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 6 V modeli için veri ek açıklamaları kullanılarak kapsayan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872422"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="a4202-104">Bölüm 6: Kullanmak için veri ek açıklamaları Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="a4202-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="a4202-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a4202-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a4202-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="a4202-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a4202-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="a4202-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a4202-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="a4202-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a4202-109">Bölüm 6 veri ek açıklamaları Model doğrulama için kullanmayı ele alır.</span><span class="sxs-lookup"><span data-stu-id="a4202-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="a4202-110">Bizim oluşturma ve düzenleme formlarla önemli sorun sunuyoruz: tüm doğrulama yaptıkları değil.</span><span class="sxs-lookup"><span data-stu-id="a4202-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="a4202-111">Biz gerekli alanları boş veya harflerini yazın fiyat alanında bırakın gibi şeyler yapabilir ve göreceğiz ilk veritabanından hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="a4202-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="a4202-112">Biz kolayca doğrulama, bizim model sınıflarına veri ek açıklamaları ekleyerek uygulamamız için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4202-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="a4202-113">ASP.NET MVC koymadan ve kullanıcılarımızın uygun iletilerin görüntüleme ilgilenebilmek ve veri ek açıklamaları bize bizim modeli özelliklerine uygulanan istiyoruz kurallar tanımlamak izin verin.</span><span class="sxs-lookup"><span data-stu-id="a4202-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="a4202-114">Bizim albüm formlara doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="a4202-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="a4202-115">Aşağıdaki veri ek açıklamasını öznitelikleri kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="a4202-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="a4202-116">**Gerekli** – özelliği gerekli bir alan olduğunu gösterir</span><span class="sxs-lookup"><span data-stu-id="a4202-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="a4202-117">**DisplayName** – form alanları ve doğrulama ileti üzerinde kullanılan istiyoruz metni tanımlar</span><span class="sxs-lookup"><span data-stu-id="a4202-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="a4202-118">**StringLength** – bir dize alanı için en fazla uzunluk tanımlar</span><span class="sxs-lookup"><span data-stu-id="a4202-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="a4202-119">**Aralık** – sayısal bir alan için bir maksimum ve minimum değeri verir</span><span class="sxs-lookup"><span data-stu-id="a4202-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="a4202-120">**Bağlama** – listeler hariç tutma veya parametresi ya da form değerleri model özelliklerine bağlanırken eklemek için alanları</span><span class="sxs-lookup"><span data-stu-id="a4202-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="a4202-121">**ScaffoldColumn** – Düzenleyicisi form alanlardan gizleme sağlar</span><span class="sxs-lookup"><span data-stu-id="a4202-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="a4202-122">*Not: veri ek açıklamasını özniteliklerini kullanarak Model doğrulama hakkında daha fazla bilgi için MSDN belgelerine bakın.*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="a4202-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="a4202-123">Albüm sınıfı açın ve aşağıdakileri ekleyin *kullanarak* deyimleri dön.</span><span class="sxs-lookup"><span data-stu-id="a4202-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="a4202-124">Ardından, aşağıda gösterildiği gibi görünen ve doğrulama öznitelikleri eklemek için özelliklerini güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a4202-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="a4202-125">Var ki, ancak biz de tarz ve sanatçı sanal özellikleri değiştirdiniz.</span><span class="sxs-lookup"><span data-stu-id="a4202-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="a4202-126">Bu, yavaş bunları gerektiği şekilde yük Entity Framework sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4202-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="a4202-127">Bu öznitelikler albüm modelimizi eklenmiş sonra bizim oluşturma ve düzenleme ekran hemen başlamak alanları doğrulama ve görünen adları kullanarak biz (örneğin albüm resim URL'si AlbumArtUrl yerine) seçtiniz.</span><span class="sxs-lookup"><span data-stu-id="a4202-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="a4202-128">Uygulamayı çalıştırın ve /StoreManager/Create için göz atın.</span><span class="sxs-lookup"><span data-stu-id="a4202-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="a4202-129">Ardından, biz bazı doğrulama kuralları bölün.</span><span class="sxs-lookup"><span data-stu-id="a4202-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="a4202-130">Bir fiyat 0 girin ve başlık boş bırakın.</span><span class="sxs-lookup"><span data-stu-id="a4202-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="a4202-131">Biz Oluştur düğmesine tıkladığınızda, biz tanımladığınız hangi alanların doğrulama kuralları karşılamayan gösteren bir doğrulama hata iletisi ile görüntülenen form göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="a4202-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="a4202-132">İstemci tarafı doğrulama test etme</span><span class="sxs-lookup"><span data-stu-id="a4202-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="a4202-133">Kullanıcıların istemci tarafı doğrulama atlayabilir için sunucu tarafında doğrulama bir uygulama açısından çok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="a4202-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="a4202-134">Ancak, sunucu tarafında doğrulama yalnızca uygulayan Web forms üç önemli sorunlar sergiler.</span><span class="sxs-lookup"><span data-stu-id="a4202-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="a4202-135">Formun gönderilen, sunucuda doğrulanması ve bunların tarayıcıya gönderilecek yanıt için beklemek kullanıcının vardır.</span><span class="sxs-lookup"><span data-stu-id="a4202-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="a4202-136">Doğrulama kuralları şimdi sağlayacak şekilde bir alan düzeltirken kullanıcı anında geri bildirim alma değil.</span><span class="sxs-lookup"><span data-stu-id="a4202-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="a4202-137">Biz, kullanıcının tarayıcısının yararlanarak yerine doğrulama mantığını gerçekleştirmek için sunucu kaynaklarını israfı.</span><span class="sxs-lookup"><span data-stu-id="a4202-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="a4202-138">Neyse ki, ASP.NET MVC 3 iskele şablonları, yerleşik istemci tarafı doğrulama doğabilecek hiçbir ek iş gerek vardır.</span><span class="sxs-lookup"><span data-stu-id="a4202-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="a4202-139">Doğrulama ileti hemen kaldırılır başlık alanında tek bir harf yazarak doğrulama gereksinimlerini karşılar.</span><span class="sxs-lookup"><span data-stu-id="a4202-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="a4202-140">[Önceki](mvc-music-store-part-5.md)
> [sonraki](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="a4202-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
