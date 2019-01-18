---
ms.openlocfilehash: a7be92adffc06ac0f25b84ea15d1a8c4896f4f9d
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54397081"
---
<!--Don't update this for 2.2, use the 2.2 version --> Çağrı [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) varsayılan düzen ayarlarını yapılandırır. [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) kümeleri aşırı [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) özelliği. [AddAuthentication (Eylem&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) aşırı yükleme, farklı amaçlar için varsayılan kimlik doğrulama düzeni ' ayarlamak için kullanılan kimlik doğrulama seçeneklerini yapılandırma sağlar. Yapılan sonraki çağrılar `AddAuthentication` önceden yapılandırılmış bir geçersiz kılma [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) özellikleri.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) bir kimlik doğrulama işleyicisi kaydetme genişletme yöntemleri yalnızca çağrılabilir kez kimlik doğrulama düzeni. Aşırı yüklemeler yapılandırma düzeni özellikleri, Düzen adı verin ve görünen adı yok.
