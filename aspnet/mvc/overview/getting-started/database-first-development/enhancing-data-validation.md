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
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>EF veritabanıyla ilk ASP.NET MVC: veri doğrulama geliştirme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir. Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.
> 
> Bu serinin parçası doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri modeli için veri ek açıklamaları ekleme konusunda odaklanır. Bu temel görüşler açıklamalar bölümünde kullanıcılardan geliştirildi.


## <a name="add-data-annotations"></a>Veri ek açıklamaları ekleme

Önceki bir konuda anlatıldığı gibi bazı veri doğrulama kuralları otomatik olarak kullanıcı girişi için uygulanır. Örneğin, sınıf özelliği için yalnızca bir sayı sağlayabilir. Daha fazla veri doğrulama kurallarını belirtmek için veri ek açıklamaları model sınıfınıza ekleyebilirsiniz. Bu ek açıklamalar, belirtilen özellik için web uygulamanızın boyunca uygulanır. Ayrıca, özellikleri görüntülenme biçimini değiştirme biçimlendirme öznitelikleri uygulayabilirsiniz; gibi metin etiketleri için kullanılan değer değiştiriliyor.

Bu öğreticide FirstName, LastName ve MiddleName özellikleri için sağlanan değerler uzunluğu kısıtlamak için veri ek açıklamaları ekleyeceksiniz. Veritabanında bu değerleri 50 karakterle sınırlıdır; Ancak, web uygulamanızda bu karakter sınırı şu anda zorlanmaz. Bir kullanıcı bu değerlerden biri için 50'den fazla karakter sağlıyorsa sayfa değeri veritabanına kaydetmek çalışırken kilitleniyor. Değerleri 0 ile 4 arasında düzeyde de kısıtlar.

Açık **Student.cs** dosyasını **modelleri** klasör. Aşağıdaki vurgulanmış kodu sınıfına ekleyin.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Enrollment.cs içinde aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Çözümü oluşturun.

Düzenleme veya öğrencinin oluşturmak için bir sayfasına göz atın. 50'den fazla karakter girin çalışırsanız, bir hata iletisi görüntülenir.

![hata iletisini göster](enhancing-data-validation/_static/image1.png)

Kayıtları düzenleme sayfasına gidin ve bir düzeyde 4 yukarıda sağlama girişiminde.

![sınıf aralık hatası](enhancing-data-validation/_static/image2.png)

Veri doğrulama ek açıklamaları özellikleri ve sınıfları için uygulayabileceğiniz tam bir listesi için bkz: [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Meta veri sınıfları ekleme

Değiştirmek için veritabanı beklemediğiniz doğrulama öznitelikleri doğrudan model sınıfı ekleme çalışır; Ancak, model sınıfı yeniden oluşturmak veritabanı değişikliklerinizi ve gerekirse, tüm model sınıfına uygulanan özniteliklerini kaybedersiniz. Bu yaklaşım, çok verimsiz ve önemli doğrulama kuralları kaybetme yatkın olabilir.

Bu sorunu önlemek için özniteliklerini içeren bir meta veri sınıfının ekleyebilirsiniz. Meta veri sınıfının modeli sınıfına ilişkilendir özniteliklerle modele uygulanır. Bu yaklaşımda, model sınıfı tüm meta verileri sınıfına uygulanan öznitelikleri kaybetmeden yeniden oluşturulabilir.

İçinde **modelleri** klasörünü adlı bir sınıf ekleyin **Metadata.cs**.

![meta veri sınıfı ekleme](enhancing-data-validation/_static/image3.png)

Metadata.cs kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Bu meta veri sınıfları tüm model sınıfları, daha önce uyguladığınız doğrulama özniteliklerini içerir. **Görüntü** özniteliği metin etiketleri için kullanılan değerini değiştirmek için kullanılır.

Şimdi, modeli sınıfları ile meta veri sınıfları ilişkilendirmeniz gerekir.

İçinde **modelleri** klasörünü adlı bir sınıf ekleyin **PartialClasses.cs**.

Dosya içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Her sınıf olarak işaretlenmiş bildirimi bir `partial` sınıfı ve her otomatik olarak oluşturulan sınıf olarak eşleşen adını ve ad alanı. Meta veri özniteliği parçalı sınıfa uygulayarak, veri doğrulama öznitelikleri otomatik olarak oluşturulan sınıfa uygulanır emin olun. Meta veri özniteliği değil yeniden kısmi sınıflarda uygulandığından modeli sınıfları yeniden başlatıldığında bu öznitelikler kayıp olmaz.

Otomatik olarak oluşturulan sınıflar yeniden oluşturmak için ContosoModel.edmx dosyasını açın. Bir kez daha, tasarım yüzeyi ve select sağ **güncelleştirme modeli veritabanından**. Veritabanı değişmemiştir olsa da, bu işlem sınıfları yeniden oluşturur. İçinde **yenileme** sekmesine **tabloları** ve **son**.

![yenileme tabloları](enhancing-data-validation/_static/image4.png)

Değişiklikleri uygulamak için ContosoModel.edmx dosyasını kaydedin.

Student.cs veya Enrollment.cs dosyasını açın ve daha önce uygulanan veri doğrulama öznitelikleri artık dosyasında olduğuna dikkat edin. Ancak, uygulamayı çalıştırın ve veri girdiğinizde doğrulama kuralları yine uygulanır dikkat edin.

> [!div class="step-by-step"]
> [Önceki](customizing-a-view.md)
> [sonraki](publish-to-azure.md)
