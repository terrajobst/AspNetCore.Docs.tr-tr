---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF veritabanıyla ilk ASP.NET MVC: veri doğrulama geliştirme | Microsoft Docs'
author: tfitzmac
description: ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879614"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="8ca5f-104">EF veritabanıyla ilk ASP.NET MVC: veri doğrulama geliştirme</span><span class="sxs-lookup"><span data-stu-id="8ca5f-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="8ca5f-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8ca5f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8ca5f-106">ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8ca5f-107">Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8ca5f-108">Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="8ca5f-109">Bu serinin parçası doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri modeli için veri ek açıklamaları ekleme konusunda odaklanır.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="8ca5f-110">Bu temel görüşler açıklamalar bölümünde kullanıcılardan geliştirildi.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="8ca5f-111">Veri ek açıklamaları ekleme</span><span class="sxs-lookup"><span data-stu-id="8ca5f-111">Add data annotations</span></span>

<span data-ttu-id="8ca5f-112">Önceki bir konuda anlatıldığı gibi bazı veri doğrulama kuralları otomatik olarak kullanıcı girişi için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="8ca5f-113">Örneğin, sınıf özelliği için yalnızca bir sayı sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="8ca5f-114">Daha fazla veri doğrulama kurallarını belirtmek için veri ek açıklamaları model sınıfınıza ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="8ca5f-115">Bu ek açıklamalar, belirtilen özellik için web uygulamanızın boyunca uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="8ca5f-116">Ayrıca, özellikleri görüntülenme biçimini değiştirme biçimlendirme öznitelikleri uygulayabilirsiniz; gibi metin etiketleri için kullanılan değer değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="8ca5f-117">Bu öğreticide FirstName, LastName ve MiddleName özellikleri için sağlanan değerler uzunluğu kısıtlamak için veri ek açıklamaları ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="8ca5f-118">Veritabanında bu değerleri 50 karakterle sınırlıdır; Ancak, web uygulamanızda bu karakter sınırı şu anda zorlanmaz.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="8ca5f-119">Bir kullanıcı bu değerlerden biri için 50'den fazla karakter sağlıyorsa sayfa değeri veritabanına kaydetmek çalışırken kilitleniyor.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="8ca5f-120">Değerleri 0 ile 4 arasında düzeyde de kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="8ca5f-121">Açık **Student.cs** dosyasını **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="8ca5f-122">Aşağıdaki vurgulanmış kodu sınıfına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="8ca5f-123">Enrollment.cs içinde aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="8ca5f-124">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-124">Build the solution.</span></span>

<span data-ttu-id="8ca5f-125">Düzenleme veya öğrencinin oluşturmak için bir sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="8ca5f-126">50'den fazla karakter girin çalışırsanız, bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![hata iletisini göster](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="8ca5f-128">Kayıtları düzenleme sayfasına gidin ve bir düzeyde 4 yukarıda sağlama girişiminde.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![sınıf aralık hatası](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="8ca5f-130">Veri doğrulama ek açıklamaları özellikleri ve sınıfları için uygulayabileceğiniz tam bir listesi için bkz: [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ca5f-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="8ca5f-131">Meta veri sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="8ca5f-131">Add metadata classes</span></span>

<span data-ttu-id="8ca5f-132">Değiştirmek için veritabanı beklemediğiniz doğrulama öznitelikleri doğrudan model sınıfı ekleme çalışır; Ancak, model sınıfı yeniden oluşturmak veritabanı değişikliklerinizi ve gerekirse, tüm model sınıfına uygulanan özniteliklerini kaybedersiniz.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="8ca5f-133">Bu yaklaşım, çok verimsiz ve önemli doğrulama kuralları kaybetme yatkın olabilir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="8ca5f-134">Bu sorunu önlemek için özniteliklerini içeren bir meta veri sınıfının ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="8ca5f-135">Meta veri sınıfının modeli sınıfına ilişkilendir özniteliklerle modele uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="8ca5f-136">Bu yaklaşımda, model sınıfı tüm meta verileri sınıfına uygulanan öznitelikleri kaybetmeden yeniden oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="8ca5f-137">İçinde **modelleri** klasörünü adlı bir sınıf ekleyin **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![meta veri sınıfı ekleme](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="8ca5f-139">Metadata.cs kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="8ca5f-140">Bu meta veri sınıfları tüm model sınıfları, daha önce uyguladığınız doğrulama özniteliklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="8ca5f-141">**Görüntü** özniteliği metin etiketleri için kullanılan değerini değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="8ca5f-142">Şimdi, modeli sınıfları ile meta veri sınıfları ilişkilendirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="8ca5f-143">İçinde **modelleri** klasörünü adlı bir sınıf ekleyin **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="8ca5f-144">Dosya içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="8ca5f-145">Her sınıf olarak işaretlenmiş bildirimi bir `partial` sınıfı ve her otomatik olarak oluşturulan sınıf olarak eşleşen adını ve ad alanı.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="8ca5f-146">Meta veri özniteliği parçalı sınıfa uygulayarak, veri doğrulama öznitelikleri otomatik olarak oluşturulan sınıfa uygulanır emin olun.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="8ca5f-147">Meta veri özniteliği değil yeniden kısmi sınıflarda uygulandığından modeli sınıfları yeniden başlatıldığında bu öznitelikler kayıp olmaz.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="8ca5f-148">Otomatik olarak oluşturulan sınıflar yeniden oluşturmak için ContosoModel.edmx dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="8ca5f-149">Bir kez daha, tasarım yüzeyi ve select sağ **güncelleştirme modeli veritabanından**.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="8ca5f-150">Veritabanı değişmemiştir olsa da, bu işlem sınıfları yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="8ca5f-151">İçinde **yenileme** sekmesine **tabloları** ve **son**.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![yenileme tabloları](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="8ca5f-153">Değişiklikleri uygulamak için ContosoModel.edmx dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="8ca5f-154">Student.cs veya Enrollment.cs dosyasını açın ve daha önce uygulanan veri doğrulama öznitelikleri artık dosyasında olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="8ca5f-155">Ancak, uygulamayı çalıştırın ve veri girdiğinizde doğrulama kuralları yine uygulanır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8ca5f-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ca5f-156">[Önceki](customizing-a-view.md)
> [sonraki](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="8ca5f-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
