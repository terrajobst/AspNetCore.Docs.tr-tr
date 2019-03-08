---
ms.openlocfilehash: 2fe12027e7a5233cf01e6c412f7ee536d479facd
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665784"
---
<!--Don't update this for 2.2, use the 2.2 version --> Çağrı [AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) varsayılan düzen ayarlarını yapılandırır. [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) kümeleri aşırı [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) özelliği. [AddAuthentication (Eylem&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) aşırı yükleme, farklı amaçlar için varsayılan kimlik doğrulama düzeni ' ayarlamak için kullanılan kimlik doğrulama seçeneklerini yapılandırma sağlar. Yapılan sonraki çağrılar `AddAuthentication` önceden yapılandırılmış bir geçersiz kılma [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) özellikleri.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) bir kimlik doğrulama işleyicisi kaydetme genişletme yöntemleri yalnızca çağrılabilir kez kimlik doğrulama düzeni. Aşırı yüklemeler yapılandırma düzeni özellikleri, Düzen adı verin ve görünen adı yok.
