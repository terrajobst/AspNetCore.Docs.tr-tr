---
title: "ASP.NET Çekirdeği ve Entity Framework 6 ile çalışmaya başlama"
author: tdykstra
description: "Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmayı gösterir."
manager: wpickett
ms.author: tdykstra
ms.date: 02/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: data/entity-framework-6
ms.openlocfilehash: 7407fe8a976978d7d5077d5e5ac6cc264565621d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama

Tarafından [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), ve [zel Dykstra](https://github.com/tdykstra)

Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmayı gösterir.

## <a name="overview"></a>Genel Bakış

Entity Framework 6 .NET Core desteklemiyor Entity Framework 6 kullanmak için projeniz .NET Framework karşı derlemek aynıdır. Platformlar arası özelliklerine gereksinim duyarsanız için yükseltmeniz gerekmektedir [Entity Framework Çekirdek](https://docs.microsoft.com/ef/).

EF6 içerik yerleştirmek için Entity Framework 6 ASP.NET Core uygulamada kullanmak için önerilen yöntem olduğundan ve bir sınıf kitaplığı'nda modeli sınıfları hedefleyen tam framework projesi. ASP.NET Core projeden sınıf kitaplığına bir başvuru ekleyin. Örnek bkz [EF6 ve ASP.NET Core projeleri ile Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

.NET Core projeleri tüm EF6 gibi komutlar işlevselliğini desteklenmediği, ASP.NET Core projesinde EF6 bağlam içine *Enable-Migrations* gerektirir.

EF6 içeriğiniz bulun proje türü ne olursa olsun yalnızca EF6 komut satırı araçları EF6 bağlamı ile çalışır. Örneğin, `Scaffold-DbContext` yalnızca Entity Framework Çekirdek kullanılabilir. Bir EF6 modeline tersine mühendislik bir veritabanı, ihtiyacınız varsa, bkz: [varolan bir veritabanına ilk kod](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Başvuru tam framework ve ASP.NET Core projesinde EF6

ASP.NET Core projeniz .NET framework ve EF6 başvurmalıdır. Örneğin, *.csproj* ASP.NET Core projenizin dosyasını aşağıdaki örneğe benzer görünür (dosya, yalnızca ilgili bölümlerini gösterilir).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Yeni bir proje oluştururken **ASP.NET çekirdek Web uygulaması (.NET Framework)** şablonu.

## <a name="handle-connection-strings"></a>Bağlantı dizelerini işleme

EF6 sınıf kitaplığı projesinde kullanacağınız EF6 komut satırı araçlarını varsayılan bir oluşturucu gerektirdiğinden, bağlam örneğini oluşturabilirsiniz. Ancak belirtin, bağlam Oluşturucusu ASP.NET Core projede; bu durumda kullanılacak bağlantı dizesini bağlantı dizesinde geçirmenize olanak sağlayan bir parametresi olmalıdır istersiniz. Bir örnek verilmiştir.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

EF6 içeriğiniz bir parametresiz oluşturucuya sahip olmadığından, bir uygulama sunmak amacıyla EF6 projenizi sahip [Idbcontextfactory](https://msdn.microsoft.com/library/hh506876). EF6 komut satırı araçları bulun ve bu uygulama kullandığından, bağlam örneğini oluşturabilirsiniz. Bir örnek verilmiştir.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

Bu örnek kodda `IDbContextFactory` uygulama sabit kodlanmış bağlantı dizesinde geçirir. Bu komut satırı araçlarını kullanacağı bağlantı dizesidir. Sınıf kitaplığı çağrı yapan uygulamanın kullandığı aynı bağlantı dizesini kullandığından emin olmak üzere bir strateji uygulamak isteyeceksiniz. Örneğin, bir ortam değişkeni hem projelerinde gelen değeri alabilir.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>ASP.NET Core projesinde bağımlılık ekleme ayarlama

Çekirdek projenin *haline* bağımlılık ekleme (dı) EF6 bağlamı kümesinde dosya `ConfigureServices`. EF bağlam nesneleri için bir istek başına ömrü kapsamlı.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

DI kullanarak denetleyicileriniz ardından bağlam örneği elde edebilirsiniz. Kod EF çekirdek bağlamı için ne yazarsınız için benzer:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Örnek uygulama

Çalışan bir örnek uygulama için bkz: [örnek Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) bu makalede eşlik.

Bu örnek, aşağıdaki adımlarda Visual Studio tarafından sıfırdan oluşturulabilir:

* Bir çözüm oluşturun.

* **Yeni Proje Ekle > Web > ASP.NET Core Web uygulaması (.NET Framework)**

* **Yeni Proje Ekle > Windows Klasik Masaüstü > sınıf kitaplığı (.NET Framework)**

* İçinde **Paket Yöneticisi Konsolu** (PMC) hem projeleri için komutu çalıştırmak `Install-Package Entityframework`.

* Sınıf kitaplığı projesinde veri modeli sınıfları ve bağlam sınıfını ve uygulaması oluşturma `IDbContextFactory`.

* Sınıf kitaplığı proje için PMC komutları çalıştırmanız `Enable-Migrations` ve `Add-Migration Initial`. Başlangıç projesi olarak ASP.NET Core proje ayarlarsanız eklemek `-StartupProjectName EF6` bu komutları.

* Çekirdek projesinde sınıf kitaplığı proje proje başvurusu ekleyin.

* Çekirdek projesinde içinde *haline*, bağlam için dı kaydedin.

* Çekirdek projesinde içinde *appsettings.json*, bağlantı dizesi ekleyin.

* Çekirdek projesinde, denetleyici ve okuma ve veri yazma doğrulamak için görünümler ekleyin. (ASP.NET Core MVC yapı iskelesi Sınıf Kitaplığı'ndan başvurulan EF6 bağlamına sahip çalışmaz unutmayın.)

## <a name="summary"></a>Özet

Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmaya yönelik temel kılavuz sağlamıştır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Entity Framework - kod tabanlı yapılandırma](https://msdn.microsoft.com/data/jj680699.aspx)
