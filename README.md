# Android Notification

Notyfikacja to wiadomość wyświetlana poza UI aplikacji. Notyfikacja występuje w czterech miejscach ( i formatach ):
- jako ikona na status barze
- jako kafelek w szufladzie notyfikacji lub wyskakujący ("heads up")
- jako badge na ikonie
- jako pasek na ekranie blokady

## Budowa kafelka z notyfikacją

![not](https://user-images.githubusercontent.com/51796385/200135419-f17bcda2-cfdc-48f9-b2b8-22a1122fb91a.png)\

## Notyfikation importance

Określa jak bardzo notyfikacja powinna "przeszkadzać" użytkownikowi by zwrócić na siebię jego uwagę. Poziomy to:

- `URGENT` - ikona na statusbarze + dźwięk + heads up ( kafelek wyskakuje u góry ekranu )
- `HIGH` - ikona na statusbarze + dźwięk
- `MEDIUM` - ikona na statsubarze 
- `LOW` - nic z powyższych

## Tworzenie notyfikacji

```
val notificationBuilder = NotificationCompat.Builder(this, "channel-1")
    .setContentTitle("Tytuł")
    .setContentText("Zawartość")
    .setSmallIcon(R.drawable.ic_launcher_foreground)
    .setPriority(NotificationCompat.PRIORITY_DEFAULT)
    
val notificationManager = NotificationManagerCompat.from(this)

val notificationChannel = NotificationChannel(
    "channel-1", // musi być takie samo ID co w builderze!
    "Main Channel", // nazwa kanału wyświetlana w ustawieniach aplikacji
    NotificationManager.IMPORTANCE_DEFAULT
)

notificationManager.createNotificationChannel(notificationChannel)

notificationManager.notify(1, notificationBuilder.build())
```

## Update notyfikacji

```
// id określa którą notyfikację aktualizujemy
notificationManager.notify(1, notificationBuilder.setContentTitle(newTitle).build()) 
```

## Usunięcie notyfikacji
```
// id określa którą notyfikację odwołujemy
notificationManager.cancel(1)
```

## Notyfikacja na ekranie blokady

Wygląd notyfikacji na ekranie blokady określa wywołanie metody `setVisibility(visibility: Int)` na builderze. Dostępne opcje to:
- `VISIBILITY_PUBLIC` - zostanie pokazana cała zawartość kafelka
- `VISIBILITY_PRIVATE` - zostaną pokazane tylko tytuł i ikona
- `VISIBILITY_SECRET` - notyfikacja nie zostanie pokazana w ogóle

Wybierając `VISIBILITY_PRIVATE` możemy zdefiniować własny wygląd kafelka przez użycie `setPublicVersion` na builderze:

```
.setPriority(NotificationCompat.PRIORITY_DEFAULT)
.setPublicVersion(
    NotificationCompat.Builder(this, "channel-1")
        .setContentTitle("Ukryte")
        .setContentText("Odblokuj aby przeczytać notyfikację")
        .build()
)
```

