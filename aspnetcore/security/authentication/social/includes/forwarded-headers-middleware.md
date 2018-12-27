## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>İleri bir proxy ile ilgili bilgi isteyin veya yük dengeleyici

Uygulamayı bir ara sunucu veya yük dengeleyici arkasında dağıtılırsa, özgün istek bilgilerden bazılarını istek üst bilgileri uygulamada iletilmesi. Bu bilgiler genellikle güvenli istek düzeni içerir (`https`), konak ve istemci IP adresi. Uygulamaları otomatik olarak bulmak ve özgün istek bilgileri kullanmak için bu istek üst bilgilerini okuma yok.

Düzeni, dış sağlayıcıları ile kimlik doğrulaması akışı etkiler bağlantı oluşturmada kullanılır. Güvenli düzeni kaybetme (`https`) yanlış güvenli olmayan yeniden yönlendirme URL'ler oluşturulurken uygulamada sonuçlanır.

İletilen üstbilgileri ara yazılım, özgün istek bilgileri uygulama isteği işleme için kullanılabilir hale getirmek için kullanın.

Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.
