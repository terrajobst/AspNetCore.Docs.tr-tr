---
title: ASP.NET Core yetkilendirme için giriş
author: rick-anderson
description: ASP.NET Core uygulamalarda yetkilendirme ve yetkilendirme ile ilgili temel bilgileri öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: b5e60b3c256941fff5e54e1a02e077c34c535902
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660715"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>ASP.NET Core yetkilendirme için giriş

<a name="security-authorization-introduction"></a>

Yetkilendirme, bir kullanıcının ne yapabileceğini belirleyen işlemi ifade eder. Örneğin, bir yönetici kullanıcının belge kitaplığı oluşturmalarına, belge eklemesine, belge düzenlemesine ve bunları silmesine izin verilir. Kitaplıkla çalışan yönetici olmayan bir Kullanıcı yalnızca belgeleri okuma yetkisine sahiptir.

Yetkilendirme, kimlik doğrulamasından dik ve bağımsızdır. Ancak, yetkilendirme bir kimlik doğrulama mekanizması gerektirir. Kimlik doğrulaması, kullanıcı kim olduğunu belirlememe işlemidir. Kimlik doğrulaması geçerli kullanıcı için bir veya daha fazla kimlik oluşturabilir.

ASP.NET Core kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/index>.

## <a name="authorization-types"></a>Yetkilendirme türleri

ASP.NET Core yetkilendirme basit, bildirime dayalı bir [rol](xref:security/authorization/roles) ve zengin bir [ilke tabanlı](xref:security/authorization/policies) model sağlar. Yetkilendirme, gereksinimlerde ifade edilir ve işleyiciler bir kullanıcının gereksinimlerine göre taleplerini değerlendirir. Zorunlu denetimler, kullanıcının erişmeye çalıştığı kaynağın Kullanıcı kimliğini ve özelliklerini değerlendiren basit ilkeleri veya ilkeleri temel alabilir.

## <a name="namespaces"></a>Ad Alanları

`AuthorizeAttribute` ve `AllowAnonymousAttribute` öznitelikleri dahil olmak üzere yetkilendirme bileşenleri `Microsoft.AspNetCore.Authorization` ad alanında bulunur.

[Basit yetkilendirme](xref:security/authorization/simple)ile ilgili belgelere başvurun.
