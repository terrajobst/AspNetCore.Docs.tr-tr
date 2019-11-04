---
title: Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine dayalı makaleler
author: rick-anderson
description: Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine göre makaleleri bulun.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463040"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine dayalı makaleler

ASP.NET Core kimlik, Visual Studio 'daki proje şablonlarına "bireysel kullanıcı hesapları" seçeneği ile dahildir.

Kimlik doğrulama şablonları `-au Individual`ile .NET Core CLI kullanılabilir:

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

Kimlik doğrulaması, .NET Core CLI `-au` seçeneği ile belirtilir. Visual Studio 'da **kimlik doğrulaması Değiştir** iletişim kutusu yeni Web uygulamaları için kullanılabilir. Visual Studio 'da yeni Web uygulamaları için varsayılan değer **kimlik doğrulaması**değildir.

Kimlik doğrulaması olmadan oluşturulan projeler:

* Oturum açmak ve oturumu kapatmak için Web sayfalarını ve Kullanıcı arabirimini bulundurmayın.
* Kimlik doğrulama kodu içermiyor.

<a name="win"></a>

## <a name="windows-authentication"></a>Windows Kimlik Doğrulaması

`-au Windows` seçeneğiyle .NET Core CLI yeni Web uygulamaları için Windows kimlik doğrulaması belirtilir. Visual Studio 'da **kimlik doğrulaması Değiştir** Iletişim kutusu **Windows kimlik doğrulama** seçeneklerini sağlar.

Windows kimlik doğrulaması seçilirse, uygulama [Windows kimlik doğrulama IIS modülünü](xref:host-and-deploy/iis/modules)kullanacak şekilde yapılandırılır. Windows kimlik doğrulaması, Intranet web sitelerine yöneliktir.

## <a name="additional-resources"></a>Ek kaynaklar

Aşağıdaki makalelerde, bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarda oluşturulan kodun nasıl kullanılacağı gösterilmektedir:

* [ASP.NET Core hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm)
* [Yetkilendirme ile korunan kullanıcı verileriyle ASP.NET Core uygulama oluşturma](xref:security/authorization/secure-data)
