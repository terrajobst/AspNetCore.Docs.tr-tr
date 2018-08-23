---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'İlk ASP.NET MVC ile EF veritabanında: veri doğrulamayı geliştirme | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: dbff33a1c4f47474adda82e796d9c292392142f2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752564"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>İlk ASP.NET MVC ile EF veritabanında: veri doğrulamayı geliştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.
> 
> Bu serinin doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri modeline veri ek açıklamaları ekleme odaklanır. Bu temel kullanıcıların yorumlar bölümünde geri bildirim üzerinde geliştirildi.


## <a name="add-data-annotations"></a>Veri ek açıklama Ekle

Bir önceki konu başlığında gördüğünüz gibi bazı veri doğrulama kuralları, kullanıcı girişi için otomatik olarak uygulanır. Örneğin, sınıf özelliği için yalnızca bir sayı sağlayabilir. Daha fazla veri doğrulama kurallarını belirtmek için veri ek açıklamaları model sınıfınızın ekleyebilirsiniz. Bu ek açıklamalar, belirtilen özellik için web uygulamanızda uygulanır. Ayrıca, özelliklerini nasıl görüntüleneceğini Değiştir biçimlendirme öznitelikleri de uygulayabilirsiniz; metin etiketlerini için kullanılan değer gibi değiştiriliyor.

Bu öğreticide, FirstName ve LastName MiddleName özellikleri için sağlanan değerler uzunluğunu kısıtlamak için veri ek açıklamalarını ekler. Veritabanında, bu değerleri 50 karakterle sınırlıdır; Ancak, web uygulamanızda bu karakter sınırı şu anda zorlanmaz. Bir kullanıcı bu değerlerden biri 50 karakterden uzun sağlıyorsa sayfası değeri veritabanına kaydedilmeye çalışıldığında kilitlenir. Ayrıca, değerleri 0 ile 4 arasında sınıf kısıtlar.

Açık **Student.cs** dosyası **modelleri** klasör. Sınıfına aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Enrollment.cs içinde aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Çözümü oluşturun.

Bir öğrenci oluşturma veya düzenleme sayfasına göz atın. 50'den fazla karakter girin çalışırsanız, bir hata iletisi görüntülenir.

![hata iletisi göster](enhancing-data-validation/_static/image1.png)

Kayıtları düzenleme sayfasına gidin ve bir sınıf 4 yukarıda sağlamak dener.

![sınıf aralık hatası](enhancing-data-validation/_static/image2.png)

Veri doğrulama ek özellikleri ve sınıflarına uygulayabilirsiniz tam bir listesi için bkz. [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Meta veri sınıfları ekleme

Değiştirmek üzere veritabanını beklemiyoruz doğrudan model sınıfı için doğrulama öznitelikleri ekleyerek çalışır; Ancak, veritabanı değişikliklerinizi ve model sınıfı yeniden oluşturmanız gerekiyorsa, tüm model sınıfa uygulanmış özniteliklerini kaybedersiniz. Bu yaklaşım çok verimsiz ve önemli doğrulama kuralları kaybetme potansiyeli olabilir.

Bu sorunu önlemek için özniteliklerini içeren bir meta veri sınıfının ekleyebilirsiniz. Model sınıfı için meta veri sınıfının ilişkilendirdiğinizde, bu öznitelikleri modele uygulanır. Bu yaklaşımda, tüm meta verileri sınıfına uygulanan öznitelikleri kaybetmeden model sınıfı üretilebilir.

İçinde **modelleri** klasör adında bir sınıf ekleyin **Metadata.cs**.

![meta veri sınıfı Ekle](enhancing-data-validation/_static/image3.png)

Metadata.cs kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Bu meta veri sınıfları tüm model sınıfları, daha önce uyguladığınız doğrulama özniteliklerini içerir. **Görünen** özniteliği metin etiketleri için kullanılan değeri değiştirmek için kullanılır.

Şimdi, meta veri sınıfları ile model sınıfları ilişkilendirmeniz gerekir.

İçinde **modelleri** klasör adında bir sınıf ekleyin **PartialClasses.cs**.

Dosyanın içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Her sınıf olarak işaretlenmiş olduğuna dikkat edin. bir `partial` sınıfı ve her otomatik olarak oluşturulan sınıf olarak eşleşen ad alanı ve adını. Kısmi sınıfa meta veri özniteliği uygulayarak veri doğrulama özniteliklerinin otomatik olarak oluşturulan sınıfa uygulanan emin olun. Meta veri özniteliği değil yeniden kısmi sınıflar uygulandığından model sınıfları yeniden oluştururken bu öznitelikler kaybolmaz.

Otomatik olarak oluşturulan sınıfları yeniden üret için ContosoModel.edmx dosyasını açın. Bir kez daha, tasarım yüzeyi ve select sağ **veritabanından bir güncelleştirme modeli**. Bu işlem, veritabanı değişmemiştir. olsa da, sınıfları yeniden oluşturulacak. İçinde **Yenile** sekmesinde **tabloları** ve **son**.

![tabloları Yenile](enhancing-data-validation/_static/image4.png)

Değişiklikleri uygulamak için ContosoModel.edmx dosyayı kaydedin.

Student.cs veya Enrollment.cs dosyasını açın ve daha önce uyguladığınız veri doğrulama öznitelikleri artık dosyasında olduğuna dikkat edin. Ancak, uygulamayı çalıştırmak ve veri girdiğinizde doğrulama kuralları hala uygulandığına dikkat edin.

> [!div class="step-by-step"]
> [Önceki](customizing-a-view.md)
> [İleri](publish-to-azure.md)
