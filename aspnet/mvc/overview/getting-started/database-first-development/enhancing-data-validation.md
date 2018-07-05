---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'İlk ASP.NET MVC ile EF veritabanında: veri doğrulamayı geliştirme | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: a328aa8aec2c512d77ddabec31b3785b8742e018
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376720"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="d9371-104">İlk ASP.NET MVC ile EF veritabanında: veri doğrulamayı geliştirme</span><span class="sxs-lookup"><span data-stu-id="d9371-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="d9371-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d9371-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d9371-106">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9371-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="d9371-107">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="d9371-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="d9371-108">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d9371-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="d9371-109">Bu serinin doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri modeline veri ek açıklamaları ekleme odaklanır.</span><span class="sxs-lookup"><span data-stu-id="d9371-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="d9371-110">Bu temel kullanıcıların yorumlar bölümünde geri bildirim üzerinde geliştirildi.</span><span class="sxs-lookup"><span data-stu-id="d9371-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="d9371-111">Veri ek açıklama Ekle</span><span class="sxs-lookup"><span data-stu-id="d9371-111">Add data annotations</span></span>

<span data-ttu-id="d9371-112">Bir önceki konu başlığında gördüğünüz gibi bazı veri doğrulama kuralları, kullanıcı girişi için otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d9371-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="d9371-113">Örneğin, sınıf özelliği için yalnızca bir sayı sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="d9371-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="d9371-114">Daha fazla veri doğrulama kurallarını belirtmek için veri ek açıklamaları model sınıfınızın ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9371-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="d9371-115">Bu ek açıklamalar, belirtilen özellik için web uygulamanızda uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d9371-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="d9371-116">Ayrıca, özelliklerini nasıl görüntüleneceğini Değiştir biçimlendirme öznitelikleri de uygulayabilirsiniz; metin etiketlerini için kullanılan değer gibi değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="d9371-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="d9371-117">Bu öğreticide, FirstName ve LastName MiddleName özellikleri için sağlanan değerler uzunluğunu kısıtlamak için veri ek açıklamalarını ekler.</span><span class="sxs-lookup"><span data-stu-id="d9371-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="d9371-118">Veritabanında, bu değerleri 50 karakterle sınırlıdır; Ancak, web uygulamanızda bu karakter sınırı şu anda zorlanmaz.</span><span class="sxs-lookup"><span data-stu-id="d9371-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="d9371-119">Bir kullanıcı bu değerlerden biri 50 karakterden uzun sağlıyorsa sayfası değeri veritabanına kaydedilmeye çalışıldığında kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="d9371-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="d9371-120">Ayrıca, değerleri 0 ile 4 arasında sınıf kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="d9371-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="d9371-121">Açık **Student.cs** dosyası **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="d9371-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="d9371-122">Sınıfına aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d9371-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="d9371-123">Enrollment.cs içinde aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d9371-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="d9371-124">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d9371-124">Build the solution.</span></span>

<span data-ttu-id="d9371-125">Bir öğrenci oluşturma veya düzenleme sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="d9371-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="d9371-126">50'den fazla karakter girin çalışırsanız, bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d9371-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![hata iletisi göster](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="d9371-128">Kayıtları düzenleme sayfasına gidin ve bir sınıf 4 yukarıda sağlamak dener.</span><span class="sxs-lookup"><span data-stu-id="d9371-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![sınıf aralık hatası](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="d9371-130">Veri doğrulama ek özellikleri ve sınıflarına uygulayabilirsiniz tam bir listesi için bkz. [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="d9371-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="d9371-131">Meta veri sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="d9371-131">Add metadata classes</span></span>

<span data-ttu-id="d9371-132">Değiştirmek üzere veritabanını beklemiyoruz doğrudan model sınıfı için doğrulama öznitelikleri ekleyerek çalışır; Ancak, veritabanı değişikliklerinizi ve model sınıfı yeniden oluşturmanız gerekiyorsa, tüm model sınıfa uygulanmış özniteliklerini kaybedersiniz.</span><span class="sxs-lookup"><span data-stu-id="d9371-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="d9371-133">Bu yaklaşım çok verimsiz ve önemli doğrulama kuralları kaybetme potansiyeli olabilir.</span><span class="sxs-lookup"><span data-stu-id="d9371-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="d9371-134">Bu sorunu önlemek için özniteliklerini içeren bir meta veri sınıfının ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9371-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="d9371-135">Model sınıfı için meta veri sınıfının ilişkilendirdiğinizde, bu öznitelikleri modele uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d9371-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="d9371-136">Bu yaklaşımda, tüm meta verileri sınıfına uygulanan öznitelikleri kaybetmeden model sınıfı üretilebilir.</span><span class="sxs-lookup"><span data-stu-id="d9371-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="d9371-137">İçinde **modelleri** klasör adında bir sınıf ekleyin **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="d9371-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![meta veri sınıfı Ekle](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="d9371-139">Metadata.cs kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d9371-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="d9371-140">Bu meta veri sınıfları tüm model sınıfları, daha önce uyguladığınız doğrulama özniteliklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="d9371-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="d9371-141">**Görünen** özniteliği metin etiketleri için kullanılan değeri değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9371-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="d9371-142">Şimdi, meta veri sınıfları ile model sınıfları ilişkilendirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9371-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="d9371-143">İçinde **modelleri** klasör adında bir sınıf ekleyin **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="d9371-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="d9371-144">Dosyanın içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d9371-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="d9371-145">Her sınıf olarak işaretlenmiş olduğuna dikkat edin. bir `partial` sınıfı ve her otomatik olarak oluşturulan sınıf olarak eşleşen ad alanı ve adını.</span><span class="sxs-lookup"><span data-stu-id="d9371-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="d9371-146">Kısmi sınıfa meta veri özniteliği uygulayarak veri doğrulama özniteliklerinin otomatik olarak oluşturulan sınıfa uygulanan emin olun.</span><span class="sxs-lookup"><span data-stu-id="d9371-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="d9371-147">Meta veri özniteliği değil yeniden kısmi sınıflar uygulandığından model sınıfları yeniden oluştururken bu öznitelikler kaybolmaz.</span><span class="sxs-lookup"><span data-stu-id="d9371-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="d9371-148">Otomatik olarak oluşturulan sınıfları yeniden üret için ContosoModel.edmx dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="d9371-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="d9371-149">Bir kez daha, tasarım yüzeyi ve select sağ **veritabanından bir güncelleştirme modeli**.</span><span class="sxs-lookup"><span data-stu-id="d9371-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="d9371-150">Bu işlem, veritabanı değişmemiştir. olsa da, sınıfları yeniden oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="d9371-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="d9371-151">İçinde **Yenile** sekmesinde **tabloları** ve **son**.</span><span class="sxs-lookup"><span data-stu-id="d9371-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![tabloları Yenile](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="d9371-153">Değişiklikleri uygulamak için ContosoModel.edmx dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d9371-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="d9371-154">Student.cs veya Enrollment.cs dosyasını açın ve daha önce uyguladığınız veri doğrulama öznitelikleri artık dosyasında olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d9371-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="d9371-155">Ancak, uygulamayı çalıştırmak ve veri girdiğinizde doğrulama kuralları hala uygulandığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d9371-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d9371-156">[Önceki](customizing-a-view.md)
> [İleri](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="d9371-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
