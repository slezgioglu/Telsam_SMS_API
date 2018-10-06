TELSAM sms xmlApi RestApi

ÖNEMLİ :
Bu api için gerekli olan Token (API token anahtarınızı) ve Keycode (API Anahtar kodunuzu) Web panelinde Ayarlar Menüsü altındaki RestApi Bilgileri sayfasında bulabilirsiniz.

Api Methodları
•	Sms Gönderimi (Tek mesajı bir veya daha fazla alıcıya gönderim)
Sms mesajını, aynı anda bir veya daha fazla kullanıcıya bu methodu kullanarak gönderim yapabilirsiniz.
•	İletim Raporları
Göndermiş olduğunuz sms mesajının id si ile sms durumunun raporlarını bu method ile alabilirsiniz.
•	Çoklu Sms gönderimi
Tek seferde birden fazla birbirinden farklı ve bağımsız Sms mesajını ve her mesajda birden fazla alıcıya gönderimi bu method ile yapabilirsiniz.
•	Çoklu SMS Gönderimi için İletim Raporları
Çoklu Sms gönderimlerinin raporlarını bu method ile alabilirsiniz.
•	Gruba Sms gönderimi
Hesabınızda kayıtlı olan gruptaki tüm kişilere mesaj gönderimini bu method ile yapabilirsiniz.
•	Kullanıcı Bilgisi
Hesabınıza ait tüm bilgileri bu method ile alabilirsiniz.
•	Başlık Originator Talebinde Bulunma
Bu method aracılığı ile kullanmak istediğiniz başlığı sisteme ekleyebilirsiniz. Onaylanma işleminden sonra kullanıma başlayabilirsiniz.
•	Rehberde Yeni Bir Grup Oluşturma
Bu method aracılığı ile rehberinize yeni bir grup ekleyebilirsiniz.
•	Rehberdeki bir Gruba Yeni Kişi Veya Kişiler Ekleme
Rehberinizde bulunan bir gruba yeni kişi veya kişileri bu method aracılığı ile ekleyebilirsiniz.

SMS Gönderimi (Tek mesajı bir veya daha fazla alıcıya gönderim)
Açıklama
Sms gönderimi; HTTP POST methodu kullanılarak gönderim yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/sendsms
XML Yapısı
<?xml version="1.0"?>
<SMS>
  <authentication> 
    <token></token > 
    <keycode></keycode > 
  </authentication> 
  <message>
    <originator></originator> 
    <text></text> 
    <unicode></unicode>
    <international></international> 
    <canceltext></canceltext>
  </message>
  <receivers>
    <receiver></receiver>
    <receiver id=”custom_id”></receiver>
  </receivers> 
</SMS>

token: API token anahtarınız
keycode: API anahtar kodunuz
originator: SMS Başlığı
text: SMS mesajı. Maksimum limit 1080 karakter olabilir. 
unicode: Unicode (örn:Türkçe karakter) mesaj gönderimi için değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.
international: Uluslar arası sms gönderimi için bu değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.
canceltext: Mesajın sonuna iptal bilgisi eklemek için bu değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.
receivers: Alıcı GMS numaralarının tutulduğu parametre
receiver: Alıcı GMS numarası. GSM numarası 0 ile başlamamalıdır.
receiver->id: Eğer kendi receiver id değerinizi kullanırsanız, receiver id parametresini kullanabilirsiniz. Kullanmaz iseniz sistem kendi id leri ile döndürür.
XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/sendsms
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<SMS>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
  <message>
    <originator>originator</originator> 
    <text>text</text> 
    <unicode></unicode> 
    <international></international>
    <canceltext></canceltext>
  </message>
  <receivers>
    <receiver>5320000000</receiver>
    <receiver>5330000000</receiver>
    <receiver>5340000000</receiver>
    <receiver>340000000</receiver>
  </receivers> 
</SMS>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
  <message_id>546544454</message_id>
  <total_valid_receivers>3</total_valid_receivers>
  <total_invalid_receivers>1</total_invalid_receivers>
  <receivers>
    <receiver id=”1034666571” gsmno=”5320000000”>OK</receiver>
    <receiver id=”1034666572” gsmno=”5330000000”>OK</receiver>
    <receiver id=”1034666573” gsmno=”5340000000”>OK</receiver>
    <receiver id=”1034666574” gsmno=”340000000”>INVALID</receiver>
  </receivers>
  <sms_count>1</sms_count>
  <amount>0.0640</amount>
  <credit>122.2600</credit>
</result>
Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürülen XML yanıttır.
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
BANNED_ORIGINATOR: Başlık kullanılamaz.
ORIGINATOR_INVALID: Başlık sistemde bulunamadı.
ORIGINATOR_UNCONFIRMED: Başlık onay bekliyor.
NO_RECEIVERS: Hiç alıcı numara bulunamadı.
NO_VALID_RECEIVERS: Hiç geçerli alıcı numara bulunamadı.
NO_TEXT: Sms mesajı boş.
BADWORDS_INTEXT: Sms mesajı içeriğinde uygunsuz kelimeler tespit edildi.
NOT_ENOUGH_CREDITS: Kredi yetersiz.


İletim Raporları
Açıklama
İletim Raporları; HTTP POST methodu kullanılarak istek yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/delivery_report
XML Yapısı
<?xml version="1.0"?>
<REPORT>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
  <message_id></message_id>
  <receivers>
    <id></id>
    <id></id>
  </receivers> 
</REPORT>

token: API token anahtarınız
keycode: API anahtar kodunuz
message_id: Raporunu almak istediğiniz mesajın id değeri. Bu değer sms gönderildiğinde sunucu tarafından size dönen değerdir.
receivers -> id: Durumunu öğrenmek istediğiniz alıcının id değeri. 
XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/delivery_report
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<REPORT>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
  <message_id>546544454</message_id>
  <receivers>
    <id>45654788</id>
    <id>45654789</id>
  </receivers> 
</REPORT>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
  <message_id>546544454</message_id>
  <receivers>
    <receiver>
      <id>45654788</id>
      <sent_date>2013-11-18 12:02:34</sent_date>
      <done_date></done_date>
      <status>PENDING</status>
    </receiver>
    <receiver>
      <id>45654789</id>
      <sent_date>2013-11-18 12:02:34</sent_date>
      <done_date>2013-11-18 12:02:46</done_date>
      <status>DELIVERED</status>
    </receiver>
  </receivers> 
</result>

receiver -> status: receiver status dönüş değerleri aşağıda açıklanmıştır.
PENDING: Mesaj alıcı numaraya gönderildi, fakat Alıcı numaranın operatöründen durum bilgisi bekleniyor.
DELIVERED: Mesaj alıcı numaraya başarılı olarak iletildi.
NOT_DELIVERED: Mesaj alıcı numaraya iletilemedi.
INVALID_DESTINATION_ADDRESS: Alıcı numara geçersiz veya hatalı.
Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürilen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
NO_MESSAGEID: Geçersiz mesaj id değeri. Not found in system.
NO_RECEIVERIDS: Hiç alıcı id si bulunamadı.

Çoklu SMS Gönderimi
Açıklama
Çoklu Sms gönderimi; Tek seferde birden fazla Sms mesajı gönderimini bu parametre ile yapabilirsiniz. HTTP POST methodu kullanılarak gönderim yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/sendmultisms
XML Yapısı
<?xml version="1.0"?>
<SMS>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
  <group_message></group_message>
  <message>
    <originator></originator> 
    <text></text> 
    <unicode></unicode>
    <international></international> 
    <receivers>
       <receiver></receiver>
       <receiver id=”custom_id”></receiver>
    </receivers>
  </message>
  <message>
     ……..
  </message>
  <message>
     ……..
  </message>
</SMS>

token: API token anahtarınız
keycode: API anahtar kodunuz
group_message: Eğer sonuç olarak tek id numarası dönmesini istiyorsanız bu değeri 1 yapın.
originator: SMS Başlığı
text: SMS mesajı. Maksimum limit 1080 karakter olabilir. 
unicode: Unicode (örn:Türkçe karakter) mesaj gönderimi için değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.
international: Uluslar arası sms gönderimi için bu değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.
receivers: Alıcı GMS numaralarının tutulduğu parametre
receiver: Alıcı GMS numarası. GSM numarası 0 ile başlamamalıdır.
receiver->id: Eğer kendi receiver id değerinizi kullanırsanız, receiver id parametresini kullanabilirsiniz. Kullanmaz iseniz sistem kendi id leri ile döndürür.

XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/sendmultisms
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<SMS>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
  <group_message>1</group_message>
  <message>
    <originator>BAŞLIK 1</originator> 
    <text>MESAJ İÇERİĞİ 1</text> 
    <unicode></unicode>
    <international></international> 
    <receivers>
       <receiver>5320000000</receiver>
       <receiver>5330000000</receiver>
       <receiver>5340000000</receiver>
    </receivers>
  </message>
  <message>
    <originator>BAŞLIK 2</originator> 
    <text>MESAJ İÇERİĞİ 2</text> 
    <unicode></unicode>
    <international></international> 
    <receivers>
       <receiver>5320000000</receiver>
       <receiver>5330000000</receiver>
       <receiver>5340000000</receiver>
    </receivers>
  </message>
</SMS>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
  <message_ids>546544454,546544455</message_ids>
  <credit>122.2600</credit>
</result>

Eğer gönderim yaparken, [group_message] parametresini 1 olarak gönderdiyseniz, tek id [message_id] parametresi içerisinde döner.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
  <message_id>546544454</message_id>
  <credit>122.2600</credit>
</result>

Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürülen XML yanıttır.
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
BANNED_ORIGINATOR: Başlık kullanılamaz.
ORIGINATOR_INVALID: Başlık sistemde bulunamadı.
ORIGINATOR_UNCONFIRMED: Başlık onay bekliyor.
NO_RECEIVERS: Hiç alıcı numara bulunamadı.
NO_VALID_RECEIVERS: Hiç geçerli alıcı numara bulunamadı.
NO_TEXT: Sms mesajı boş.
BADWORDS_INTEXT: Sms mesajı içeriğinde uygunsuz kelimeler tespit edildi.
NOT_ENOUGH_CREDITS: Kredi yetersiz.


Çoklu Sms Gönderimi için İletim Raporları
Açıklama
Çoklu SMS Gönderimi için İletim Raporları; Çoklu Sms gönderimlerinin raporlarını bu api modülü ile alabilirsiniz. HTTP POST methodu kullanılarak istek yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/delivery_report_for_multisms
XML Yapısı
<?xml version="1.0"?>
<REPORT>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
  <group_message></group_message>
  <message_id></message_id> 
</REPORT>

token: API token anahtarınız
keycode: API anahtar kodunuz
group_message: Eğer gönderiminizi grup mesaj olarak yapıp tek id aldıysanız, bu değeri 1 yapın.
message_id: Raporunu almak istediğiniz mesajın id değeri. Bu değer sms gönderildiğinde sunucu tarafından size dönen değerdir.

XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/delivery_report_for_multisms
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<REPORT>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication>
  <group_message>1</group_message> 
  <message_id>546544454</message_id>
</REPORT>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
  <message_id>546544454</message_id>
  <receivers>
    <receiver>
      <id>45654788</id>
      <sent_date>2013-11-18 12:02:34</sent_date>
      <done_date></done_date>
      <status>PENDING</status>
    </receiver>
    <receiver>
      <id>45654789</id>
      <sent_date>2013-11-18 12:02:34</sent_date>
      <done_date>2013-11-18 12:02:46</done_date>
      <status>DELIVERED</status>
    </receiver>
  </receivers> 
</result>

receiver -> status: receiver status dönüş değerleri aşağıda açıklanmıştır.
PENDING: Mesaj alıcı numaraya gönderildi, fakat Alıcı numaranın operatöründen durum bilgisi bekleniyor.
DELIVERED: Mesaj alıcı numaraya başarılı olarak iletildi.
NOT_DELIVERED: Mesaj alıcı numaraya iletilemedi.
INVALID_DESTINATION_ADDRESS: Alıcı numara geçersiz veya hatalı.
Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürilen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
NO_MESSAGEID: Geçersiz mesaj id değeri. Not found in system.
NO_RECEIVERIDS: Hiç alıcı id si bulunamadı.


Gruba SMS Gönderimi
Açıklama
Gruba Sms gönderimi; HTTP POST methodu kullanılarak gönderim yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/sendgroupsms
XML Yapısı
<?xml version="1.0"?>
<SMS>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
  <message>
    <groupname></groupname>
    <originator></originator> 
    <text></text> 
    <unicode></unicode>
    <international></international> 
    <canceltext></canceltext>
  </message> 
</SMS>

token: API token anahtarınız
keycode: API anahtar kodunuz
groupname: Gönderimin yapılacağı Grubun İsmi
originator: SMS Başlığı
text: SMS mesajı. Maksimum limit 1080 karakter olabilir. 
unicode: Unicode (örn:Türkçe karakter) mesaj gönderimi için değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.
international: Uluslar arası sms gönderimi için bu değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.
canceltext: Mesajın sonuna iptal bilgisi eklemek için bu değer 1 olmalıdır. Aksi halde boş bırakılmalıdır.

XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/sendgroupsms
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<SMS>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
  <message>
    <groupname>GRUP 1</groupname>
    <originator>originator</originator> 
    <text>text</text> 
    <unicode></unicode> 
    <international></international>
    <canceltext></canceltext>
  </message>
</SMS>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
  <message_id>546544454</message_id>
  <total_valid_receivers>3</total_valid_receivers>
  <total_invalid_receivers>1</total_invalid_receivers>
  <receivers>
    <receiver id=”1034666571” gsmno=”5320000000”>OK</receiver>
    <receiver id=”1034666572” gsmno=”5330000000”>OK</receiver>
    <receiver id=”1034666573” gsmno=”5340000000”>OK</receiver>
    <receiver id=”1034666574” gsmno=”340000000”>INVALID</receiver>
  </receivers>
  <sms_count>1</sms_count>
  <amount>0.0640</amount>
  <credit>122.2600</credit>
</result>
Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürülen XML yanıttır.
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
BANNED_ORIGINATOR: Başlık kullanılamaz.
ORIGINATOR_INVALID: Başlık sistemde bulunamadı.
ORIGINATOR_UNCONFIRMED: Başlık onay bekliyor.
NO_GROUP: Bu isimde bir grup mevcut değil.
NO_RECEIVERS: Gruba kayıtlı Hiç alıcı numara bulunamadı.
NO_VALID_RECEIVERS: Gruba kayıtlı Hiç geçerli alıcı numara bulunamadı.
NO_TEXT: Sms mesajı boş.
BADWORDS_INTEXT: Sms mesajı içeriğinde uygunsuz kelimeler tespit edildi.
NOT_ENOUGH_CREDITS: Kredi yetersiz.
Kullanıcı Bilgisi
Açıklama
Kullanıcı bilgisi; HTTP POST methodu kullanılarak istek yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/userinfo
XML Yapısı
<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
</INFO>

token: API token anahtarınız
keycode: API anahtar kodunuz
XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/user_info
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
</INFO>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
  <user>
    <credit>122.2600</credit>
    <alphanumeric_price>0.0250</alphanumeric_price>
    <numeric_price>0.0180</numeric_price>
    <international_price>0.2200</international_price>
    <originators>
      <originator>
        <title>originator1</title>
	 <status>OK</status>
      </originator>
      <originator>
	 <title>originator2</title>
	 <status>WAITING_FORM</status>
      </originator>
    </originators>
  </user>
</result>
Hatalı Dönüş Cevabı
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.

Başlık Originator Ekleme
Açıklama
Başlık Talebinde Bulunma; HTTP POST methodu kullanılarak işlem yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/addoriginator
XML Yapısı
<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
  <originator></originator> 
</INFO>

token: API token anahtarınız
keycode: API anahtar kodunuz
originator: Eklemek istediğiniz SMS Başlığı
XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/addoriginator
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
  <originator>originator</originator> 
</INFO>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
</result>
Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürülen XML yanıttır.
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo, addoriginator.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
EMPTY_ORIGINATOR: originator parametresi boş.
BANNED_ORIGINATOR: Başlık kullanılamaz.
WAITING_ORIGINATOR: Başlık zaten onay bekliyor.
ORIGINATOR_USED: Başlık zaten kullanımda.


Rehberde Yeni Bir Grup Oluşturma
Açıklama
Rehberde Yeni Bir Grup Oluşturma; HTTP POST methodu kullanılarak işlem yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/addgroup
XML Yapısı
<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
  <group_name></group_name> 
</INFO>

token: API token anahtarınız
keycode: API anahtar kodunuz
group_name: Eklemek istediğiniz Grubun İsmi
XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/addgroup
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
  <group_name>grup ismi</group_name> 
</INFO>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
</result>
Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürülen XML yanıttır.
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo, addoriginator.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
EMPTY_GROUPNAME: group_name parametresi boş veya geçersiz.
GROUPNAME_USED: Bu isimde grup zaten mevcut.

Rehberde Gruba Yeni Kişi Veya Kişiler Ekleme
Açıklama
Rehberde Gruba Yeni Kişi Veya Kişiler Ekleme; HTTP POST methodu kullanılarak işlem yapılabilir. Parametre açıklamaları ve gönderim şekli aşağıda açıklanmıştır.
API Web Adresi
http://websms.telsam.com.tr/xmlapiV4/addcontacts
XML Yapısı
<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token></token> 
    <keycode></keycode> 
  </authentication> 
  <contacts>
     <group_name></group_name>
     <contact>
        <fullname></fullname>
        <gsmno></gsmno>
        <pstnno></pstnno>
        <email></email>
        <blood></blood>
     </contact>
     <contact>
        ……….
     </contact>
     <contact>
        ……….
     </contact>
  </contacts> 
</INFO>

token: API token anahtarınız
keycode: API anahtar kodunuz
group_name: Kişileri eklemek istediğiniz Grubun İsmi (Grup ismi boş bırakılır ise kişi veya kişiler GRUPSUZLARA eklenecektir.
fullname: Eklenecek kişinin Adı Soyadı
gsmno: Eklenecek kişinin Gsm Numarası
pstnno: Eklenecek kişinin Sabit Telefon Numarası
email: Eklenecek kişinin E-posta adresi
blood: Eklenecek kişinin kan Grubu (Bu parametrede Özel bir bilgiyide kayıt edebilirsiniz.)

XML İstek Örneği
POST http://websms.telsam.com.tr/xmlapiV4/addcontacts
Host: websms.telsam.com.tr
Content-Type: application/xml
Accept: */*

<?xml version="1.0"?>
<INFO>
  <authentication> 
    <token>token</token> 
    <keycode>keycode</keycode> 
  </authentication> 
  <contacts>
     <group_name>GRUP 1</group_name>
     <contact>
        <fullname>Ali Veli</fullname>
        <gsmno>5320000000</gsmno>
        <pstnno>3120000000</pstnno>
        <email>test1@test.com</email>
        <blood>Özel bilgi 1</blood>
     </contact>
     <contact>
        <fullname>Mustafa Veli</fullname>
        <gsmno>5330000000</gsmno>
        <pstnno>2120000000</pstnno>
        <email>test2@test.com</email>
        <blood>Özel bilgi 2</blood>
     </contact>
  </contacts>
</INFO>
Başarılı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi başarılı bir prosedür döndürülen XML yanıttır.
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <status>OK</status>
</result>
Hatalı Dönüş Cevabı
API yanıtı XML formatında döner. Aşağıdaki gibi hatalı bir prosedür döndürülen XML yanıttır.
<result>
  <status>ERROR</status>
  <error_code>AUTH_FAILED</error_code>
  <error_description>Invalid token or keycode</error_description>
</result>

Error Codes
GENERAL_ERROR: Bilinmeyen API komutu. Mevcut komutlar: sendsms, delivery_report, userinfo, addoriginator.
XML_ERROR: XML Post parametresi boş veya geçersiz.
AUTH_FAILED: Hatalı token veya keycode.
USER_DENIED: Erişim engellendi.
GROUP_NOT_FOUND: Bu isimde Grup bulunamadı.
CONTACT_ERROR: Eklenecek kişi veya kişilerde <fullname> veya <gsmno> parametresi boş veya geçersiz olanlar mevcut.




PHP Örnek Sms Gönderim Kodu
Açıklama
Aşağıdaki PHP ile yapılmış örnekten yola çıkarak API entegrasyonunuzu yapabilirsiniz.

<?php
$postUrl = 'http://websms.telsam.com.tr/xmlapiV4/sendsms';
$token = ‘XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX;   //API token anahtarınız
$keycode = ‘XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX;    //API anahtar kodunuz
$originator = 'BASLIK'; //Buraya Başlık gireceksiniz
$text = 'Test Mesaj';

$receivers = array('5320000000', '5420000000');

foreach ( $receivers AS $receiver ){  
	$receiversStr .= '<receiver>' . $receiver . '</receiver>';
}

$xmlStr = '
<?xml version="1.0"?>
<SMS>
  <authentication> 
    <token>' . $token . '</token> 
    <keycode>' . $keycode . '</keycode> 
  </authentication> 
  <message>
    <originator>' . $originator . '</originator> 
    <text>' . $text . '</text> 
    <unicode></unicode> 
  </message>
  <receivers>' . $receiversStr . '</receivers> 
</SMS>';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $postUrl); 
curl_setopt($ch, CURLOPT_POST, 1); 
curl_setopt($ch, CURLOPT_POSTFIELDS, $xmlStr);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

$response = curl_exec($ch);

echo $response;
