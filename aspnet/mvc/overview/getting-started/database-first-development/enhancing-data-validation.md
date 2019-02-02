---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Öğretici: Veri doğrulama EF veritabanı ilk için ASP.NET MVC uygulaması ile geliştirin'
description: Bu öğreticide, doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri modeline veri ek açıklamaları ekleme odaklanır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667628"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Öğretici: Veri doğrulama EF veritabanı ilk için ASP.NET MVC uygulaması ile geliştirin

MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.

Bu öğreticide, doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri modeline veri ek açıklamaları ekleme odaklanır. Bu temel kullanıcıların yorumlar bölümünde geri bildirim üzerinde geliştirildi.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Veri ek açıklama Ekle
> * Meta veri sınıfları ekleme

## <a name="prerequisites"></a>Önkoşullar

* [Bir görünümü özelleştirme](customizing-a-view.md)

## <a name="add-data-annotations"></a>Veri ek açıklama Ekle

Bir önceki konu başlığında gördüğünüz gibi bazı veri doğrulama kuralları, kullanıcı girişi için otomatik olarak uygulanır. Örneğin, sınıf özelliği için yalnızca bir sayı sağlayabilir. Daha fazla veri doğrulama kurallarını belirtmek için veri ek açıklamaları model sınıfınızın ekleyebilirsiniz. Bu ek açıklamalar, belirtilen özellik için web uygulamanızda uygulanır. Ayrıca, özelliklerini nasıl görüntüleneceğini Değiştir biçimlendirme öznitelikleri de uygulayabilirsiniz; metin etiketlerini için kullanılan değer gibi değiştiriliyor.

Bu öğreticide, FirstName ve LastName MiddleName özellikleri için sağlanan değerler uzunluğunu kısıtlamak için veri ek açıklamalarını ekler. Veritabanında, bu değerleri 50 karakterle sınırlıdır; Ancak, web uygulamanızda bu karakter sınırı şu anda zorlanmaz. Bir kullanıcı bu değerlerden biri 50 karakterden uzun sağlıyorsa sayfası değeri veritabanına kaydedilmeye çalışıldığında kilitlenir. Ayrıca, değerleri 0 ile 4 arasında sınıf kısıtlar.

Seçin **modelleri** > **ContosoModel.edmx** > **ContosoModel.tt** açın *Student.cs* dosya. Sınıfına aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Açık *Enrollment.cs* ve aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Çözümü oluşturun.

Tıklayın **Öğrenciler listesi** seçip **Düzenle**. 50'den fazla karakter girin çalışırsanız, bir hata iletisi görüntülenir.

![hata iletisi göster](enhancing-data-validation/_static/image1.png)

Giriş sayfasına geri dönün. Tıklayın **kayıtları listesi** seçip **Düzenle**. Bir sınıf 4 yukarıda sağlamak çalışır. Bu hatayı alırsınız: *Alanı, sınıf 0 ile 4 arasında olması gerekir.*

## <a name="add-metadata-classes"></a>Meta veri sınıfları ekleme

Değiştirmek üzere veritabanını beklemiyoruz doğrudan model sınıfı için doğrulama öznitelikleri ekleyerek çalışır; Ancak, veritabanı değişikliklerinizi ve model sınıfı yeniden oluşturmanız gerekiyorsa, tüm model sınıfa uygulanmış özniteliklerini kaybedersiniz. Bu yaklaşım çok verimsiz ve önemli doğrulama kuralları kaybetme potansiyeli olabilir.

Bu sorunu önlemek için özniteliklerini içeren bir meta veri sınıfının ekleyebilirsiniz. Model sınıfı için meta veri sınıfının ilişkilendirdiğinizde, bu öznitelikleri modele uygulanır. Bu yaklaşımda, tüm meta verileri sınıfına uygulanan öznitelikleri kaybetmeden model sınıfı üretilebilir.

İçinde **modelleri** klasör adında bir sınıf ekleyin *Metadata.cs*.

Değiştirin *Metadata.cs* aşağıdaki kod ile.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Bu meta veri sınıfları tüm model sınıfları, daha önce uyguladığınız doğrulama özniteliklerini içerir. **Görünen** özniteliği metin etiketleri için kullanılan değeri değiştirmek için kullanılır.

Şimdi, meta veri sınıfları ile model sınıfları ilişkilendirmeniz gerekir.

İçinde **modelleri** klasör adında bir sınıf ekleyin *PartialClasses.cs*.

Dosyanın içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Her sınıf olarak işaretlenmiş olduğuna dikkat edin. bir `partial` sınıfı ve her otomatik olarak oluşturulan sınıf olarak eşleşen ad alanı ve adını. Kısmi sınıfa meta veri özniteliği uygulayarak veri doğrulama özniteliklerinin otomatik olarak oluşturulan sınıfa uygulanan emin olun. Meta veri özniteliği değil yeniden kısmi sınıflar uygulandığından model sınıfları yeniden oluştururken bu öznitelikler kaybolmaz.

Otomatik olarak oluşturulan sınıflar yeniden oluşturmak için açık *ContosoModel.edmx* dosya. Bir kez daha, tasarım yüzeyi ve select sağ **veritabanından bir güncelleştirme modeli**. Bu işlem, veritabanı değişmemiştir. olsa da, sınıfları yeniden oluşturulacak. İçinde **Yenile** sekmesinde **tabloları** ve **son**.

Kaydet *ContosoModel.edmx* değişiklikleri uygulamak için dosya.

Açık *Student.cs* dosya veya *Enrollment.cs* dosya ve daha önce uyguladığınız veri doğrulama öznitelikleri artık dosyasındadır dikkat edin. Ancak, uygulamayı çalıştırmak ve veri girdiğinizde doğrulama kuralları hala uygulandığına dikkat edin.

## <a name="conclusion"></a>Sonuç

Bu seri, düzenleyebilir, güncelleştirebilir, oluşturabilir ve verileri silmek kullanıcıların sağlayan varolan bir veritabanından kodu oluşturmak nasıl basit bir örnek sağlanır. Projeyi oluşturmak için ASP.NET MVC 5, Entity Framework ve ASP.NET iskeleti oluşturma kullanılır. 

Code First geliştirmeye giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md). 

Daha gelişmiş bir örnek için bkz: [ASP.NET MVC 4 uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Veritabanı ilk verilerle çalışmak için kullandığınız DbContext API Code First verilerle çalışmak için kullandığınız API ile aynı olduğunu unutmayın. Veritabanı ilk kullanmak istediğinize olsa bile, vb. kod ilk öğreticide öğesinden eşzamanlılık çakışmalarını işleme okuma ve ilgili verileri güncelleştirme gibi daha karmaşık senaryolarda işlemek nasıl öğrenebilirsiniz. Tek fark, nasıl veritabanı bağlamı sınıfının ve varlık sınıfları oluşturulur ' dir.

## <a name="additional-resources"></a>Ek kaynaklar

Veri doğrulama ek özellikleri ve sınıflarına uygulayabilirsiniz tam bir listesi için bkz. [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Eklenen veri ek açıklamaları
> * Ek meta veri sınıfları

Bir web uygulaması ve SQL veritabanı, Azure App Service'e dağıtma konusunda bilgi almak için bu öğreticiye bakın:
> [!div class="nextstepaction"]
> [Azure App Service'e .NET uygulaması dağıtma](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
