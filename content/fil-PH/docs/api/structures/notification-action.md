# Mga bagay ng NotificationAction

* `type` String - Ang uri ng aksyon, ay maaaring `button`.
* `text` String - (opsyonal) Ang tanda para sa ibinigay na aksyon.

## Platform / Suporta ng Aksyon

| Uri ng Aksyon | Suporta ng Platform | Kagamitan ng `text`                | Default `text` | Mga limitasyon                                                                                                                                                                          |
| ------------- | ------------------- | ---------------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `pindutan`    | macOS               | Ginagamit bilang tanda ng pindutan | "Ipakita"      | Pinakamarami ay isang pindutan, kung marami ang inilagay ang huli ang dapat gamitin. Ang aksyon na ito ay hindi tugma pag may `hasReply` at hindi papansinin kung `hasReply` ay `true`. |

### Suporta ng pindutan sa macOS

Ang kaayusan para gumana ang pindutan ng karagdagang abiso sa macOS ang iyong app ay kailangang matugunan ang sumusunod na pamantayan.

* Nalagdaan na ang app
* Ang app ay mayroon ng `NSUserNotificationAlertStyle` na itinakda sa`alert` sa mga `info.plist`.

Kung alinman sa mga kinakailangan ay hindi natagpuan ang pindutan ay hindi lilitaw.