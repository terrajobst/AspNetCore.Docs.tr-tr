## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>İstek bilgilerini bir ara sunucu veya yük dengeleyici ile ilet

Uygulama bir ara sunucu veya yük dengeleyici arkasında dağıtılırsa, bazı özgün istek bilgileri, istek üst bilgilerinde uygulamaya iletilebilir. Bu bilgiler genellikle güvenli istek düzenini (`https`), Konağı ve istemci IP adresini içerir. Uygulamalar, özgün istek bilgilerini bulacak ve kullanacak şekilde bu istek üst bilgilerini otomatik olarak okumayın.

Düzen, dış sağlayıcılarla kimlik doğrulama akışını etkileyen bağlantı oluşturma sırasında kullanılır. Güvenli düzenin kaybolması (`https`) uygulamanın hatalı güvenli olmayan yeniden yönlendirme URL 'Leri üretmesine neden olur.

İstek işleme için özgün istek bilgilerini uygulama için kullanılabilir hale getirmek üzere Iletilen üstbilgiler ara yazılımını kullanın.

Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.
