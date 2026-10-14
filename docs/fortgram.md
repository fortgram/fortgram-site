<a id="lang-en"></a>
🌐 **Languages / Языки:** [English](#lang-en) · [Русский](#lang-ru)

# FortGram — How It Works

FortGram is an Android Telegram client based on a fork of [Telegram X](https://github.com/TGX-Android/Telegram-X), with a parental-control layer built into the app. It is intended for a child's device that is already managed by a device-level tool such as Google Family Link.

## Architecture: two layers

FortGram does not introduce a separate "child mode" or a special account type. Protection comes from two layers that complement each other:

1. **Telegram's own account model.** The child uses a regular Telegram account. The parent keeps a second session of that account on their own phone and manages contacts, groups and privacy settings from there. Whatever the parent puts into the account is what the child gets.
2. **The client-side whitelist inside FortGram.** Even when a stranger writes first, adds the child to a group or sends a sticker pack, FortGram decides what the child actually sees and can answer. The rules live on the device behind a parental PIN and can be exported, imported and moved between phones.

The first layer is what Telegram already offers; the second is what FortGram adds. Together they close the gaps each has alone: Telegram's privacy settings cannot hide an existing chat, and a client alone cannot stop the child from logging in elsewhere — that is why the [account preparation](#child-account-preparation) below matters.

## What FortGram changes vs. Telegram X

| Area | What it does |
|---|---|
| **Parental PIN** | 4-digit PIN, separate from the app passcode, stored on the device as a hash. Opening *Settings* asks for it, so the child cannot change account or privacy settings, sign out or add another account. |
| **Whitelist mode** | When on, the child can chat only with phone contacts, users the parent allowed explicitly, and groups the parent allowed. Everything else — unknown users, other groups, channels, secret chats, bots — is hidden from the chat list, search and notifications, and cannot be opened by link. Creating groups and channels, starting secret chats and joining by invite link are blocked. |
| **Allowed contacts** | Users who are not in the phone book but may talk to the child, added by `@username`. |
| **Allowed groups** | The list of groups the child is a member of, each with a switch. Only switched-on groups are visible and writable. |
| **Adding contacts** | A separate switch decides whether the child may add new contacts on their own (adding a contact would otherwise whitelist that person). |
| **Sticker packs** | Optional restriction: only official Telegram packs plus packs the parent allowed. Other stickers — incoming ones too — are drawn as their emoji instead of the image. Packs are picked from the installed list, found by name, or added by link. |
| **Notifications** | Push notifications work as in Telegram X, but nothing from hidden chats reaches the notification shade. |
| **Backup of the rules** | Every parental setting except the PIN can be exported and imported — as a file, via the clipboard, or as a QR code. See [Backup, transfer and QR](#backup-transfer-and-qr). |
| **Background work and calls** | A settings screen with shortcuts to the phone-maker's switches (battery, autostart, pop-up windows) that decide whether calls and messages arrive while the app is closed. |
| **Languages** | The parental screens are available in English, Russian and Ukrainian; the rest of the app uses Telegram's translations. |
| **No in-app updater** | Updates come from [fgram.org](https://fgram.org) / [GitHub Releases](https://github.com/fortgram/fortgram/releases), never from the Telegram X channel. |

When the whitelist mode is off, FortGram behaves exactly like Telegram X.

## Privacy and infrastructure

> **FortGram does not use any third-party infrastructure.**

FortGram does not run its own backend, does not proxy messages, does not collect telemetry and does not store anything outside of Telegram. All traffic goes through Telegram's servers exactly as with any other client. The parental PIN and the whitelist are stored only on the device. Push notifications are delivered through Firebase Cloud Messaging, the same channel every Telegram client on Android uses; Google Analytics is not enabled in the project.

Consequently:
- **Telegram's Terms of Service and privacy policy apply unchanged.**
- **Responsibility for what the child does on Telegram lies with Telegram and with the parent**, not with any third party. FortGram is a UI client.

### For the technically minded: what is and is not in the build

- **No Google Analytics, no Crashlytics, no ad or attribution SDKs.** The Firebase project behind FortGram has Analytics disabled at project level, and the app is built without the `firebase-analytics` / `firebase-core` libraries (Telegram X excludes them explicitly); `firebase_analytics_collection_deactivated` is set in the manifest as a second guard.
- **Firebase is used for one thing: Cloud Messaging (push).** The app registers an FCM token with Telegram, and Telegram's servers wake the app through Google's push service, exactly as every Android Telegram client does. The push payload is encrypted by Telegram; Google sees only that a message exists.
- **No crash reports leave the device.** Crash details can be copied manually from *Settings* and sent by the parent if they choose to.
- **The app never contacts fgram.org.** The site hosts documentation and downloads; it counts page views without cookies (see the Privacy Policy).
- The client is open source, so all of the above can be checked in the code rather than taken on trust.

## Child account preparation

All of this happens through normal Telegram features; nothing FortGram-specific yet.

### 1. Create or designate a Telegram account for the child

Register the account on a phone number you (the parent) control long-term — a separate SIM, an eSIM or a virtual number. The parent must keep access to that number for account recovery.

### 2. Set a cloud password (Two-Step Verification)

Enable Two-Step Verification on the child's account.

- Only the parent knows the password; the recovery e-mail is the parent's.
- Without it the child can sign in to standard Telegram on any device with an SMS code and bypass FortGram entirely. **This is the single most important step.**

### 3. Add the child's account to the parent's Telegram

On the parent's phone add the child's account as a second account in any client (Telegram, Telegram X or FortGram). You need the SMS code and the cloud password. Both parents or guardians can keep their own session.

### 4. Configure privacy and the address book

From the parent's session of the child's account:

- **Who can find me by phone number** → My Contacts
- **Who can send me messages** → My Contacts
- **Who can add me to groups and channels** → My Contacts (or Nobody)
- **Who can call me** → My Contacts (or Nobody)
- **Phone number / last seen / profile photo** → restrict as appropriate

Then add the contacts the child may talk to and join the groups the child may be in.

### 5. Sign in to FortGram on the child's device

Install FortGram (see [Installation](#installation)) and sign in with the child's account — SMS code and cloud password as usual.

### 6. Set the parental PIN and switch the whitelist on

*Settings → Parental Controls*: set the PIN, then enable **Whitelist mode**. The app asks to restart so the change applies to existing chats. Then:

- open **Allowed groups** and switch on the groups the child may use;
- add **Allowed contacts** by `@username` if some people are not in the phone book;
- decide whether **Allow adding contacts** stays on;
- optionally enable **Official sticker packs only** and pick the extra packs under **Allowed sticker packs**.

Finally, **Export settings** (see below) so the rules can be restored on a new phone.

### Ongoing management

Day-to-day — new contacts, new groups, reviewing chats — happens on the parent's phone in the parent's session of the child's account. A group the parent adds the child to stays hidden until it is switched on in *Allowed groups* on the child's device (behind the PIN). Remote switching from the parent's phone is on the roadmap.

## Backup, transfer and QR

*Settings → Parental Controls → Backup.*

**Export settings** produces a small text document with all the rules: the three switches (whitelist mode, adding contacts, official stickers only), allowed users (with id, `@username` and name), allowed groups (id and title) and allowed sticker packs (id, short name, title). The PIN is never exported.

Export targets:

- **Show QR code** — the rules compressed into a QR code, to be scanned by another FortGram (a second child's phone, a replacement phone). If there are too many entries for one code, the app says so — use a file instead.
- **Save to file** — the system "Save as" dialog, no storage permission needed.
- **Copy to clipboard** / **Share…** — as plain text.

**Import settings** reads the same document from a **QR code** (built-in scanner), **a file**, **the clipboard** or **pasted text**. Before anything is applied, a preview shows every switch and every entry with a checkbox — untick what you do not want, then **Apply**. Users and packs without numeric ids are resolved on the device by `@username` and pack name, so a document can be written by hand:

```json
{"fortgram_parental": 1,
 "whitelist": {"enabled": true, "allow_add_contact": false,
   "users": [{"username": "grandma_anna"}], "groups": []},
 "stickers": {"official_only": true, "sets": [{"name": "HotCherry"}]}}
```

Importing replaces the current rules. If the whitelist switch changes, the app asks to restart.

## Background work and calls

Messages arrive through push notifications even when FortGram is closed. Incoming calls additionally need the phone to let the app show the call screen from the background. Many manufacturers (Xiaomi/Redmi, realme, Huawei, Samsung) block this by default.

*Settings → Background work and calls* lists the switches and opens the right screen for each:

- **Battery: no restrictions** — removes battery optimisation for FortGram.
- **Display over other apps** — needed for the incoming-call screen.
- On Xiaomi/Redmi (MIUI): **Pop-up windows in background** and **Show on lock screen** in the app's "Other permissions", plus **Autostart**.

If calls ring only while the app is open, go through this screen first.

## Installation

FortGram is distributed as an APK from [fgram.org](https://fgram.org) and [GitHub Releases](https://github.com/fortgram/fortgram/releases); it is not in Google Play yet (see the roadmap). Package name: `org.fgram.app`.

1. On the child's device allow installation from the browser or file manager you use (Family Link can require the parent's approval).
2. Download the APK and open it. Updates are installed the same way over the existing app — the account, PIN and rules stay.
3. Every release is signed with the project key. To verify a downloaded APK: `apksigner verify --print-certs FortGram-*.apk` must show certificate SHA-256 `54:DA:5E:95:C6:83:2D:DE:0A:89:35:48:2D:5D:EE:C7:A3:FA:C0:A1:9B:10:AF:58:E1:E1:70:F7:1C:17:5F:2B`. An APK with a different fingerprint is not from this project.

Requirements: Android 7.0 or newer, 64-bit ARM device (arm64-v8a) — that is practically every phone sold since 2017.

## Known limitations

- **The whitelist hides, it does not leave.** The child stays a member of hidden groups and keeps hidden chats in the account; they are simply invisible on the child's device. Leaving groups or deleting chats is done from the parent's session.
- **Content is not filtered.** Inside allowed chats FortGram does not read or censor messages, photos or links. It controls *who*, not *what*. Sticker restriction is the one exception, and it covers sticker packs only — not GIFs or custom emoji.
- **Only this device is protected.** With the SMS code and the cloud password the account can be opened in any Telegram client; the cloud password known only to the parent is what prevents that. Telegram Web and Telegram Desktop are covered by the same rule.
- **Group members are visible.** In an allowed group the child sees all participants and their messages, including people who are not whitelisted; opening a private chat with them is blocked.
- **Notifications from hidden chats are dropped on the device**, but Telegram still counts them as unread in the parent's session.
- **Screenshots, forwarding and copying** inside allowed chats work as in any Telegram client.

### Moving to a new phone

1. On the old phone: *Parental Controls → Export settings → Show QR code* (or *Save to file*).
2. On the new phone: install FortGram, sign in with the child's account, set a PIN.
3. *Parental Controls → Import settings → Scan QR code* (or *From file*), check the preview, **Apply**.

---

<a id="lang-ru"></a>
🌐 **Languages / Языки:** [English](#lang-en) · [Русский](#lang-ru)

# FortGram — Как это работает

FortGram — Android-клиент Telegram на основе форка [Telegram X](https://github.com/TGX-Android/Telegram-X) со встроенным слоем родительского контроля. Предназначен для устройства ребёнка, которое уже управляется на уровне системы — например, через Google Family Link.

## Архитектура: два уровня

FortGram не вводит отдельный «детский режим» и не создаёт особый тип аккаунта. Защита строится из двух уровней, которые дополняют друг друга:

1. **Собственная модель аккаунта Telegram.** Ребёнок пользуется обычным Telegram-аккаунтом. У родителя на своём телефоне есть вторая сессия этого аккаунта, откуда он управляет контактами, группами и приватностью. Что родитель добавляет в аккаунт — то ребёнок и получает.
2. **Белый список внутри FortGram.** Даже если незнакомец напишет первым, добавит ребёнка в группу или пришлёт стикерпак, именно FortGram решает, что ребёнок реально увидит и на что сможет ответить. Правила хранятся на устройстве за родительским PIN, их можно экспортировать, импортировать и переносить между телефонами.

Первый уровень даёт сам Telegram, второй добавляет FortGram. Вместе они закрывают дыры друг друга: настройки приватности Telegram не могут скрыть уже существующий чат, а клиент сам по себе не помешает ребёнку войти в аккаунт в другом месте — поэтому важна [подготовка аккаунта](#подготовка-детского-аккаунта) ниже.

## Что FortGram меняет по сравнению с Telegram X

| Область | Что делает |
|---|---|
| **Родительский PIN** | 4-значный PIN, отдельный от пароля приложения, хранится на устройстве в виде хеша. Вход в *Настройки* требует его ввода, поэтому ребёнок не может изменить настройки аккаунта и приватности, выйти из аккаунта или добавить другой. |
| **Режим белого списка** | Когда включён, ребёнок может общаться только с контактами из телефонной книги, с явно разрешёнными пользователями и в разрешённых группах. Всё остальное — незнакомые пользователи, другие группы, каналы, секретные чаты, боты — скрыто из списка чатов, поиска и уведомлений и не открывается по ссылке. Создание групп и каналов, секретные чаты и вступление по приглашению заблокированы. |
| **Разрешённые контакты** | Люди не из телефонной книги, с которыми ребёнку можно общаться; добавляются по `@username`. |
| **Разрешённые группы** | Список групп, в которых состоит ребёнок, с переключателем у каждой. Видны и доступны для сообщений только включённые. |
| **Добавление контактов** | Отдельный переключатель: может ли ребёнок сам добавлять новые контакты (иначе добавленный контакт автоматически попадал бы в белый список). |
| **Стикерпаки** | Необязательное ограничение: только официальные наборы Telegram плюс разрешённые родителем. Остальные стикеры — в том числе входящие — рисуются как эмодзи вместо картинки. Наборы выбираются из установленных, ищутся по названию или добавляются по ссылке. |
| **Уведомления** | Push-уведомления работают как в Telegram X, но из скрытых чатов в шторку ничего не попадает. |
| **Резервная копия правил** | Все родительские настройки, кроме PIN, экспортируются и импортируются — файлом, через буфер обмена или QR-кодом. См. [Резервная копия, перенос и QR](#резервная-копия-перенос-и-qr). |
| **Фоновая работа и звонки** | Экран настроек с переходами к переключателям производителя (батарея, автозапуск, всплывающие окна), от которых зависит, приходят ли звонки и сообщения при закрытом приложении. |
| **Языки** | Родительские экраны доступны на английском, русском и украинском; остальное приложение использует переводы Telegram. |
| **Без встроенного обновления** | Обновления берутся с [fgram.org](https://fgram.org) / [GitHub Releases](https://github.com/fortgram/fortgram/releases), а не из канала Telegram X. |

Когда режим белого списка выключен, FortGram ведёт себя ровно как Telegram X.

## Приватность и инфраструктура

> **FortGram не использует стороннюю инфраструктуру.**

FortGram не запускает собственный бэкенд, не проксирует сообщения, не собирает телеметрию и не хранит ничего за пределами Telegram. Весь трафик идёт через серверы Telegram, как у любого другого клиента. Родительский PIN и белый список хранятся только на устройстве. Push-уведомления доставляются через Firebase Cloud Messaging — тот же канал, которым пользуется каждый Telegram-клиент на Android; Google Analytics в проекте не включён.

Из этого следует:
- **Условия использования и политика конфиденциальности Telegram применяются без изменений.**
- **Ответственность за то, что ребёнок делает в Telegram, лежит на Telegram и на родителях**, а не на третьей стороне. FortGram — это клиент.

### Для специалистов: что есть и чего нет в сборке

- **Нет Google Analytics, Crashlytics, рекламных и трекинговых SDK.** В Firebase-проекте FortGram Analytics отключён на уровне проекта, а приложение собирается без библиотек `firebase-analytics` / `firebase-core` (Telegram X исключает их явно); в манифесте дополнительно стоит `firebase_analytics_collection_deactivated`.
- **Firebase используется ровно для одного: Cloud Messaging (push).** Приложение регистрирует FCM-токен в Telegram, и серверы Telegram будят приложение через push-сервис Google — так работает каждый Android-клиент Telegram. Содержимое пуша шифрует Telegram; Google видит лишь факт наличия сообщения.
- **Отчёты о сбоях не покидают устройство.** Детали сбоя можно скопировать вручную из *Настроек* и отправить самостоятельно, если родитель захочет.
- **Приложение к fgram.org не обращается.** Сайт раздаёт документацию и файлы; просмотры страниц считаются без cookies (см. Политику конфиденциальности).
- Клиент с открытым исходным кодом, так что всё перечисленное проверяется по коду, а не принимается на веру.

## Подготовка детского аккаунта

Всё делается штатными средствами Telegram, ничего специфичного для FortGram пока не требуется.

### 1. Создать или выделить Telegram-аккаунт для ребёнка

Зарегистрируйте аккаунт на номер, который контролируете вы (родитель) и сохраните надолго — отдельная SIM, eSIM или виртуальный номер. Доступ к номеру нужен для восстановления аккаунта.

### 2. Установить облачный пароль (двухэтапная проверка)

Включите двухэтапную проверку на детском аккаунте.

- Пароль знает только родитель; резервная почта — родительская.
- Без него ребёнок войдёт в обычный Telegram на любом устройстве по SMS-коду и обойдёт FortGram целиком. **Это самый важный шаг.**

### 3. Добавить детский аккаунт на телефон родителя

На своём телефоне добавьте детский аккаунт как второй в любой клиент (Telegram, Telegram X или FortGram). Понадобятся SMS-код и облачный пароль. Оба родителя или опекуна могут держать по своей сессии.

### 4. Настроить приватность и адресную книгу

Из родительской сессии детского аккаунта:

- **Кто может найти меня по номеру телефона** → Мои контакты
- **Кто может писать мне** → Мои контакты
- **Кто может добавлять меня в группы и каналы** → Мои контакты (или Никто)
- **Кто может мне звонить** → Мои контакты (или Никто)
- **Номер / время захода / фото профиля** → ограничьте по своему усмотрению

Затем добавьте контакты, с которыми ребёнку можно общаться, и вступите в группы, где ему можно состоять.

### 5. Войти в FortGram на устройстве ребёнка

Установите FortGram (см. [Установка](#установка)) и войдите под детским аккаунтом — SMS-код и облачный пароль как обычно.

### 6. Задать родительский PIN и включить белый список

*Настройки → Родительский контроль*: задайте PIN, затем включите **Режим белого списка**. Приложение попросит перезапуск, чтобы правила применились к существующим чатам. Дальше:

- откройте **Разрешённые группы** и включите те, которыми ребёнку можно пользоваться;
- добавьте **Разрешённые контакты** по `@username`, если кого-то нет в телефонной книге;
- решите, оставлять ли включённым **Разрешить добавление контактов**;
- при желании включите **Только официальные стикерпаки** и отметьте дополнительные наборы в **Разрешённых стикерпаках**.

В конце сделайте **Экспорт настроек** (см. ниже), чтобы правила можно было восстановить на новом телефоне.

### Текущее управление

Повседневное — новые контакты, новые группы, просмотр переписки — происходит на телефоне родителя в его сессии детского аккаунта. Группа, куда родитель добавил ребёнка, остаётся скрытой, пока её не включить в *Разрешённых группах* на устройстве ребёнка (за PIN). Удалённое включение с телефона родителя — в дорожной карте.

## Резервная копия, перенос и QR

*Настройки → Родительский контроль → Резервная копия.*

**Экспорт настроек** создаёт небольшой текстовый документ со всеми правилами: три переключателя (белый список, добавление контактов, только официальные стикеры), разрешённые пользователи (id, `@username`, имя), разрешённые группы (id, название) и разрешённые стикерпаки (id, короткое имя, название). PIN не экспортируется никогда.

Куда экспортировать:

- **Показать QR-код** — правила, сжатые в QR-код, для сканирования другим FortGram (телефон второго ребёнка, новый телефон). Если записей слишком много для одного кода, приложение сообщит об этом — тогда сохраняйте в файл.
- **Сохранить в файл** — системный диалог «Сохранить как», без прав на хранилище.
- **Скопировать в буфер обмена** / **Поделиться…** — обычным текстом.

**Импорт настроек** читает тот же документ из **QR-кода** (встроенный сканер), **файла**, **буфера обмена** или **вставленного текста**. Перед применением открывается предпросмотр: все переключатели и все записи с галочками — снимите лишнее и нажмите **Применить**. Пользователи и наборы без числовых id распознаются на устройстве по `@username` и имени набора, так что документ можно написать и руками:

```json
{"fortgram_parental": 1,
 "whitelist": {"enabled": true, "allow_add_contact": false,
   "users": [{"username": "grandma_anna"}], "groups": []},
 "stickers": {"official_only": true, "sets": [{"name": "HotCherry"}]}}
```

Импорт заменяет текущие правила. Если меняется переключатель белого списка, приложение попросит перезапуск.

## Фоновая работа и звонки

Сообщения приходят через push-уведомления даже при закрытом FortGram. Входящим звонкам дополнительно нужно, чтобы телефон разрешил приложению показать экран звонка из фона. Многие производители (Xiaomi/Redmi, realme, Huawei, Samsung) по умолчанию это запрещают.

*Настройки → Фоновая работа и звонки* перечисляет нужные переключатели и открывает для каждого нужный экран:

- **Батарея: без ограничений** — снимает оптимизацию батареи для FortGram.
- **Поверх других приложений** — нужно для экрана входящего звонка.
- На Xiaomi/Redmi (MIUI): **Всплывающие окна в фоновом режиме** и **Показывать на экране блокировки** в «Других разрешениях» приложения, а также **Автозапуск**.

Если звонки приходят только при открытом приложении — сначала пройдите этот экран.

## Установка

FortGram распространяется как APK с [fgram.org](https://fgram.org) и через [GitHub Releases](https://github.com/fortgram/fortgram/releases); в Google Play его пока нет (см. дорожную карту). Имя пакета: `org.fgram.app`.

1. На устройстве ребёнка разрешите установку из браузера или файлового менеджера, которым пользуетесь (Family Link может потребовать одобрение родителя).
2. Скачайте APK и откройте его. Обновления ставятся так же, поверх установленного приложения — аккаунт, PIN и правила сохраняются.
3. Каждый выпуск подписан ключом проекта. Проверить скачанный APK: `apksigner verify --print-certs FortGram-*.apk` должен показать сертификат SHA-256 `54:DA:5E:95:C6:83:2D:DE:0A:89:35:48:2D:5D:EE:C7:A3:FA:C0:A1:9B:10:AF:58:E1:E1:70:F7:1C:17:5F:2B`. APK с другим отпечатком — не от этого проекта.

Требования: Android 7.0 и новее, 64-битное ARM-устройство (arm64-v8a) — практически любой телефон, выпущенный с 2017 года.

## Известные ограничения

- **Белый список скрывает, а не удаляет.** Ребёнок остаётся участником скрытых групп, а скрытые чаты остаются в аккаунте — они просто не видны на его устройстве. Выход из групп и удаление чатов делаются из родительской сессии.
- **Содержимое не фильтруется.** Внутри разрешённых чатов FortGram не читает и не цензурирует сообщения, фото и ссылки. Он управляет тем, *с кем*, а не *о чём*. Единственное исключение — ограничение стикеров, и оно касается только стикерпаков, не GIF и не кастомных эмодзи.
- **Защищено только это устройство.** По SMS-коду и облачному паролю аккаунт открывается в любом клиенте Telegram; именно облачный пароль, известный только родителю, этому мешает. Telegram Web и Telegram Desktop подпадают под то же правило.
- **Участники групп видны.** В разрешённой группе ребёнок видит всех участников и их сообщения, включая людей не из белого списка; открыть с ними личный чат нельзя.
- **Уведомления из скрытых чатов отбрасываются на устройстве**, но в родительской сессии Telegram по-прежнему считает их непрочитанными.
- **Скриншоты, пересылка и копирование** внутри разрешённых чатов работают как в любом клиенте Telegram.

### Перенос на новый телефон

1. На старом телефоне: *Родительский контроль → Экспорт настроек → Показать QR-код* (или *Сохранить в файл*).
2. На новом: установите FortGram, войдите под детским аккаунтом, задайте PIN.
3. *Родительский контроль → Импорт настроек → Сканировать QR-код* (или *Из файла*), проверьте предпросмотр, **Применить**.
