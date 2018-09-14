---
title: ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama
author: rick-anderson
description: Bu makalede, bir ASP.NET Core uygulaması Entity Framework 6 kullanmayı gösterir.
ms.author: tdykstra
ms.date: 02/24/2017
uid: data/entity-framework-6
ms.openlocfilehash: ba3ee137bfefd53cf2b7be2f328651ad38104ab5
ms.sourcegitcommit: 70fb7c9d5f2ddfcf4747382a9f7159feca7a6aa7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45601749"
---
# <a name="get-started-with-aspnet-core-and-entity-framework-6"></a>ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama

Tarafından [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), ve [Tom Dykstra](https://github.com/tdykstra)

Bu makalede, bir ASP.NET Core uygulaması Entity Framework 6 kullanmayı gösterir.

## <a name="overview"></a>Genel Bakış

Entity Framework 6 .NET Core desteklemediğinden Entity Framework 6 kullanmak için projeniz .NET Framework karşı derleme gerekir. Platformlar arası özelliklerine ihtiyacınız varsa, yükseltme gerekecektir [Entity Framework Core](https://docs.microsoft.com/ef/).

Entity Framework 6 içinde ASP.NET Core uygulamasını kullanmak için önerilen yöntem EF6 bağlam eklemektir ve model sınıfları bir sınıf kitaplığı'nda, Framework'ün tamamını hedefleyen proje. Sınıf kitaplığı ASP.NET Core projesi bir başvuru ekleyin. Örnek görmek [EF6 ve ASP.NET Core projeleri Visual Studio çözümüyle](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

.NET Core projeleri EF6 gibi komutlar işlevlerin desteklenmediği bir ASP.NET Core projesinde EF6 bağlam konulamıyor *etkinleştir geçişleri* gerektirir.

EF6 Bağlamınızı bulmanıza, proje türü ne olursa olsun yalnızca EF6 komut satırı araçlarını bir EF6 bağlamı ile çalışır. Örneğin, `Scaffold-DbContext` yalnızca Entity Framework Core içinde kullanılabilir. Bir EF6 modele tersine mühendislik, bir veritabanının gerekiyorsa bkz [var olan bir veritabanına ilk kod](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Tam başvuru framework ve ASP.NET Core projesinde EF6

ASP.NET Core projeniz .NET framework ve EF6 başvurmalıdır. Örneğin, *.csproj* ASP.NET Core proje dosyası aşağıdaki örneğe benzer görünür (yalnızca ilgili bölümlerin dosyanın gösterilir).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Yeni bir proje oluştururken **ASP.NET Core Web uygulaması (.NET Framework)** şablonu.

## <a name="handle-connection-strings"></a>Bağlantı dizeleri işleme

EF6 sınıf kitaplığı projesinde kullanacağınız EF6 komut satırı araçlarını bir varsayılan oluşturucu gerektirdiğinden, bağlam örneğini oluşturabilir. Ancak, içerik oluşturucu durumda ASP.NET Core projesinde kullanılacak bağlantı dizesini bağlantı dizesine geçmenizi sağlayan bir parametreye sahip olmalıdır belirtmek büyük olasılıkla istersiniz. Bir örnek aşağıda verilmiştir.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

EF6 Bağlamınızı parametresiz bir oluşturucu olmadığı bir uygulamasını sağlamak üzere EF6 projenizi sahip [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). EF6 komut satırı araçlarını bulun ve bağlam örneğini oluşturabilir, böylece bu uygulamayı kullanın. Bir örnek aşağıda verilmiştir.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

Bu örnek kodda `IDbContextFactory` uygulaması bir sabit kodlanmış bağlantı dizesine geçirir. Bu komut satırı araçlarını kullanan bağlantı dizesidir. Sınıf Kitaplığı arama uygulamanın kullandığı aynı bağlantı dizesine kullanmasını sağlamak için bir strateji yürütmelidir isteyebilirsiniz. Örneğin, bir ortam değişkeninden hem projelerde değer alabilir.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>ASP.NET Core projesi bağımlılık ekleme ayarlama

Temel projenin *Startup.cs* EF6 bağlamı için bağımlılık ekleme (dı) kümesinde dosya `ConfigureServices`. EF bağlam nesneleri için bir istek başına ömrü kapsamında olmalıdır.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Ardından bağlam örneğini, DI kullanarak denetleyicilerinizi alabilirsiniz. Kod, bir EF Core bağlamının ne yazmalısınız için benzer:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Örnek uygulama

Çalışan bir örnek uygulama için bkz. [örnek Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) bu makalede, eşlik.

Bu örnek, aşağıdaki adımlarda Visual Studio tarafından sıfırdan oluşturulabilir:

* Bir çözüm oluşturun.

* **Ekleme** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**
  * Proje şablonu seçimi iletişim kutusunda, açılır menüde API ve .NET Framework'ü seçin

* **Ekleme** > **yeni proje** > **Windows Masaüstü** > **sınıf kitaplığı (.NET Framework)**

* İçinde **Paket Yöneticisi Konsolu** (PMC) iki proje için komutu çalıştırmak `Install-Package Entityframework`.

* Sınıf kitaplığı projesinde veri modeli sınıfları ve bağlam sınıfını ve uygulaması oluşturma `IDbContextFactory`.

* Sınıf kitaplığı projesi için PMC'de komutları çalıştırmak `Enable-Migrations` ve `Add-Migration Initial`. ASP.NET Core projesi başlangıç projesi olarak ayarladıysanız, ekleme `-StartupProjectName EF6` bu komutların.

* Core projesinde sınıf kitaplığı projesine bir proje başvurusu ekleyin.

* Core projesinde içinde *Startup.cs*, bağlam DI için kaydolun.

* Core projesinde içinde *appsettings.json*, bağlantı dizesi ekleyin.

* Bir denetleyici ve okuma ve yazma veri doğrulamak için görünümleri çekirdek proje ekleyin. (Unutmayın. ASP.NET Core MVC yapı iskelesi Sınıf Kitaplığı'ndan başvurulan EF6 bağlamı ile çalışmaz.)

## <a name="summary"></a>Özet

Bu makalede, bir ASP.NET Core uygulaması'nda Entity Framework 6 kullanma için temel kılavuz hazırladı.

## <a name="additional-resources"></a>Ek kaynaklar

* [Entity Framework - kod tabanlı yapılandırma](https://msdn.microsoft.com/data/jj680699.aspx)
