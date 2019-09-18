---
title: Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine dayalı makaleler
author: rick-anderson
description: Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine göre makaleleri bulun.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080698"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine dayalı makaleler

ASP.NET Core kimlik, Visual Studio 'daki proje şablonlarına "bireysel kullanıcı hesapları" seçeneği ile dahildir.

Kimlik doğrulama şablonları ile `-au Individual`.NET Core CLI kullanılabilir:

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Web API kimlik doğrulaması için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore/issues/5833) bakın.

<a name="no"></a>

## <a name="no-authentication"></a>Kimlik doğrulaması yok

Kimlik doğrulaması, .NET Core CLI `-au` seçeneğiyle belirtilir. Visual Studio 'da **kimlik doğrulaması Değiştir** iletişim kutusu yeni Web uygulamaları için kullanılabilir. Visual Studio 'da yeni Web uygulamaları için varsayılan değer **kimlik doğrulaması**değildir.

Kimlik doğrulaması olmadan oluşturulan projeler:

* Oturum açmak ve oturumu kapatmak için Web sayfalarını ve Kullanıcı arabirimini bulundurmayın.
* Kimlik doğrulama kodu içermiyor.

<a name="win"></a>

## <a name="windows-authentication"></a>Windows Kimlik Doğrulaması

.NET Core CLI yeni Web uygulamaları için Windows kimlik doğrulaması, `-au Windows` seçeneğiyle belirtilir. Visual Studio 'da **kimlik doğrulaması Değiştir** Iletişim kutusu **Windows kimlik doğrulama** seçeneklerini sağlar.

Windows kimlik doğrulaması seçilirse, uygulama [Windows kimlik doğrulama IIS modülünü](xref:host-and-deploy/iis/modules)kullanacak şekilde yapılandırılır. Windows kimlik doğrulaması, Intranet web sitelerine yöneliktir.

## <a name="additional-resources"></a>Ek kaynaklar

Aşağıdaki makalelerde, bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarda oluşturulan kodun nasıl kullanılacağı gösterilmektedir:

* [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
* [ASP.NET Core hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm)
* [Yetkilendirme ile korunan kullanıcı verileriyle ASP.NET Core uygulama oluşturma](xref:security/authorization/secure-data)
