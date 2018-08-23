---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: '6. Bölüm: Model doğrulama için veri ek açıklamalarını kullanma | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 6. bölüm veri ek açıklamaları Model için V incelemektedir...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b666c3cef0b09c6d68cee581a3c27c08e3357cca
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752551"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="3641b-104">6. Bölüm: Kullanmak için veri ek açıklamaları Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="3641b-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="3641b-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3641b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3641b-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="3641b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3641b-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="3641b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="3641b-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3641b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3641b-109">6. bölüm veri ek açıklamaları Model doğrulama için incelemektedir.</span><span class="sxs-lookup"><span data-stu-id="3641b-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="3641b-110">Önemli bir sorun bizim oluşturma ve düzenleme formlarla sahibiz: herhangi bir doğrulama yaptıkları değil.</span><span class="sxs-lookup"><span data-stu-id="3641b-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="3641b-111">Fiyat alanında harflerini yazın ya da gerekli alanları boş bırakın gibi işlemler yapabileceğimiz ve görüyoruz ilk veritabanından hatadır.</span><span class="sxs-lookup"><span data-stu-id="3641b-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="3641b-112">Kolayca doğrulama uygulamamıza bizim model sınıfları için veri ek açıklamaları ekleyerek ekleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="3641b-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="3641b-113">Veri ek açıklamaları tanımlamak sunduğumuz modeli özelliklerine uygulanan istiyoruz kuralları olanak tanır ve ASP.NET MVC uygulamadan ve kullanıcılarımız için uygun iletileri görüntülemeyi ilgileniriz.</span><span class="sxs-lookup"><span data-stu-id="3641b-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="3641b-114">Bizim albüm formları için doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="3641b-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="3641b-115">Aşağıdaki veri ek açıklama öznitelikleri kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="3641b-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="3641b-116">**Gerekli** – gerekli bir alan özelliği belirtir</span><span class="sxs-lookup"><span data-stu-id="3641b-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="3641b-117">**DisplayName** – form alanlarını ve doğrulama iletilerinin kullanılan istiyoruz metni tanımlar</span><span class="sxs-lookup"><span data-stu-id="3641b-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="3641b-118">**StringLength** – tanımlayan bir dize alanı için en fazla uzunluk</span><span class="sxs-lookup"><span data-stu-id="3641b-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="3641b-119">**Aralık** – maksimum ve minimum değer için sayısal bir alan sağlar.</span><span class="sxs-lookup"><span data-stu-id="3641b-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="3641b-120">**Bağlama** – listeler hariç tutma veya parametresi veya form değerleri, model özellikleri bağlanırken eklemek için alanlar</span><span class="sxs-lookup"><span data-stu-id="3641b-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="3641b-121">**ScaffoldColumn** – Düzenleyicisi formlarından alanları gizleme sağlar</span><span class="sxs-lookup"><span data-stu-id="3641b-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="3641b-122">*Not: veri ek açıklama öznitelikleri kullanarak Model doğrulama hakkında daha fazla bilgi için MSDN belgelerine bakın.*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="3641b-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="3641b-123">Albüm sınıfını açın ve aşağıdakileri ekleyin *kullanarak* en.</span><span class="sxs-lookup"><span data-stu-id="3641b-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="3641b-124">Ardından, aşağıda gösterildiği gibi görünen ve doğrulama öznitelikleri eklemek için özelliklerini güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="3641b-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="3641b-125">Biz var. kullanmadığınız esnada özelliğin ayrıca tarz ve sanatçı sanal özelliklerine değiştirdik.</span><span class="sxs-lookup"><span data-stu-id="3641b-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="3641b-126">Bu, yavaş bunları gerektiği şekilde yük Entity Framework sağlar.</span><span class="sxs-lookup"><span data-stu-id="3641b-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="3641b-127">Bu öznitelikler, albüm modelimizi eklenen sonra bizim oluşturma ve düzenleme ekranı hemen başlamak alanları doğrulama ve görünen adlarını kullanarak biz (örneğin albüm resim URL'si yerine AlbumArtUrl) seçtiniz.</span><span class="sxs-lookup"><span data-stu-id="3641b-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="3641b-128">Uygulamayı çalıştırmak ve /StoreManager/Create için göz atın.</span><span class="sxs-lookup"><span data-stu-id="3641b-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="3641b-129">Ardından, biz bazı doğrulama kuralı ihlal.</span><span class="sxs-lookup"><span data-stu-id="3641b-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="3641b-130">Bir fiyat 0 girin ve başlık boş bırakın.</span><span class="sxs-lookup"><span data-stu-id="3641b-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="3641b-131">Biz Oluştur düğmesine tıkladığınızda, formun tanımladığımız hangi alanların doğrulama kuralları karşılamayan gösteren bir doğrulama hata iletisi ile görüntülenen göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="3641b-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="3641b-132">İstemci tarafı doğrulama sınaması</span><span class="sxs-lookup"><span data-stu-id="3641b-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="3641b-133">Sunucu tarafı doğrulama bir uygulama açısından oldukça önemli çünkü kullanıcılar istemci tarafı doğrulama atlayabilir.</span><span class="sxs-lookup"><span data-stu-id="3641b-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="3641b-134">Ancak, yalnızca sunucu tarafı doğrulama uygulayan Web forms üç önemli sorunlar sergiler.</span><span class="sxs-lookup"><span data-stu-id="3641b-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="3641b-135">Kullanıcının formun gönderilen, sunucu üzerinde doğrulanır ve kendi tarayıcıya gönderilecek yanıt için beklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="3641b-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="3641b-136">Artık doğrulama kuralları geçirir, böylece bir alan düzeltirken kullanıcıya anında geri bildirim elde edemez.</span><span class="sxs-lookup"><span data-stu-id="3641b-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="3641b-137">Biz, kullanıcının tarayıcı yararlanarak yerine doğrulama mantığını gerçekleştirmesi için sunucu kaynağı israfına yol.</span><span class="sxs-lookup"><span data-stu-id="3641b-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="3641b-138">Neyse ki, ASP.NET MVC 3 iskele şablonlarında yerleşik istemci tarafı doğrulama olmadan hiçbir ek iş gerek vardır.</span><span class="sxs-lookup"><span data-stu-id="3641b-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="3641b-139">Doğrulama iletisini hemen kaldırılır başlık alanında tek bir harf yazarak doğrulama gereksinimlerini karşılar.</span><span class="sxs-lookup"><span data-stu-id="3641b-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="3641b-140">[Önceki](mvc-music-store-part-5.md)
> [İleri](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="3641b-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
