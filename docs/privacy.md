<a id="lang-en"></a>
🌐 **Languages / Языки:** [English](#lang-en) · [Русский](#lang-ru)

# Privacy Policy

*Draft — last updated 2026-09-19. Applies to the FortGram Android app (`org.fgram.app`) and the website fgram.org.*

## The short version

FortGram is a Telegram client. Everything you send and receive goes to Telegram's servers under [Telegram's Privacy Policy](https://telegram.org/privacy), exactly as with the official apps. FortGram itself has no servers, no accounts of its own, no analytics and no advertising. The parental settings and the parental PIN never leave the device.

## What the app stores on the device

All of this lives in the app's private storage on the phone and is deleted when the app is uninstalled:

- **The Telegram session** — the same data any Telegram client keeps: the account's authorisation keys, cached chats, messages and media. Managed by TDLib, Telegram's official client library.
- **Parental settings** — the whitelist switches, the lists of allowed users, groups and sticker packs. Stored as identifiers, not as message content.
- **The parental PIN** — stored only as a salted SHA-256 hash. The PIN itself is not recoverable from the device.
- **App preferences** — theme, language, notification settings, as in Telegram X.

The app does not read the phone's contacts, call log, location or files unless the user explicitly uses a Telegram feature that needs them (contact sync, sharing a location or a file), in which case Android asks for the corresponding permission and the data goes to Telegram.

## What leaves the device, and where to

| Recipient | What | Why |
|---|---|---|
| **Telegram** | Messages, media, contacts, account data — everything a Telegram client sends | Providing the messaging service. Governed by Telegram's own terms and privacy policy. |
| **Google (Firebase Cloud Messaging)** | A push-notification token for this installation; push messages sent by Telegram to wake the app | Delivering notifications while the app is closed. Telegram encrypts the payload; Google can see that a push was sent, not its content. Every Android Telegram client uses this channel. |
| **Google (Play services, on device)** | Nothing sent | QR-code scanning uses Google's on-device barcode library. Map previews for shared locations are loaded from the map provider Telegram X uses. |

Nothing is sent to fgram.org or to the FortGram developers. There is no telemetry, no crash reporting service, no analytics SDK and no advertising SDK in the app. Google Analytics is disabled in the Firebase project used for push delivery.

## Exported settings files

*Export settings* creates a file (or QR code) with the parental rules: identifiers, usernames and display names of allowed users, group titles and sticker pack names. It contains no messages and never contains the PIN. You decide where that file goes; treat it like any document with names of your child's contacts.

## Parents, children and accounts

FortGram is meant to be installed by a parent or legal guardian on a child's device. The Telegram account is created and controlled by the parent; the parent decides who may contact the child. FortGram does not collect personal data from children and has no way of knowing a user's age. Telegram's minimum-age requirements continue to apply — see the [Disclaimer](./DISCLAIMER.md).

The parent's own session of the child's account (on the parent's phone) sees the same chats the account has; that is a Telegram feature, not something FortGram adds or transmits.

## The website

fgram.org is a static site hosted on GitHub Pages. It counts page views with [Cloudflare Web Analytics](https://www.cloudflare.com/web-analytics/), which works without cookies, without fingerprinting and without identifying individual visitors — we see how many people open which pages and from which countries, nothing more. GitHub may log connection data (such as IP addresses) according to [GitHub's privacy statement](https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement). The app never contacts fgram.org.

## Your choices

- Uninstalling FortGram removes everything it stored on the device. The Telegram account itself, its chats and its other sessions are unaffected — manage those from Telegram's settings or from the parent's session.
- Push notifications can be switched off in the app's notification settings; the app then relies on being opened.
- The source code is published under the GPL-3.0 licence at [github.com/fortgram/fortgram](https://github.com/fortgram/fortgram), so everything above can be verified in the code.

## Changes and contact

This policy will change when the app does; the date at the top tells you the current version. Questions: open an issue in the project repository or write to the address on fgram.org.

---

<a id="lang-ru"></a>
🌐 **Languages / Языки:** [English](#lang-en) · [Русский](#lang-ru)

# Политика конфиденциальности

*Черновик — обновлено 2026-09-19. Относится к Android-приложению FortGram (`org.fgram.app`) и сайту fgram.org.*

## Коротко

FortGram — это клиент Telegram. Всё, что вы отправляете и получаете, уходит на серверы Telegram по [Политике конфиденциальности Telegram](https://telegram.org/privacy) — ровно как в официальных приложениях. У самого FortGram нет серверов, нет собственных аккаунтов, нет аналитики и рекламы. Родительские настройки и PIN никогда не покидают устройство.

## Что приложение хранит на устройстве

Всё это лежит в закрытом хранилище приложения на телефоне и удаляется вместе с приложением:

- **Сессия Telegram** — те же данные, что хранит любой клиент Telegram: ключи авторизации аккаунта, кэш чатов, сообщений и медиа. Ими управляет TDLib, официальная клиентская библиотека Telegram.
- **Родительские настройки** — переключатели белого списка, списки разрешённых пользователей, групп и стикерпаков. Хранятся как идентификаторы, а не как содержимое сообщений.
- **Родительский PIN** — только в виде SHA-256-хеша с солью. Сам PIN восстановить из устройства нельзя.
- **Настройки приложения** — тема, язык, уведомления, как в Telegram X.

Приложение не читает контакты телефона, журнал звонков, местоположение или файлы, пока пользователь явно не воспользуется функцией Telegram, которой это нужно (синхронизация контактов, отправка геопозиции или файла) — тогда Android запрашивает соответствующее разрешение, а данные уходят в Telegram.

## Что покидает устройство и куда

| Получатель | Что | Зачем |
|---|---|---|
| **Telegram** | Сообщения, медиа, контакты, данные аккаунта — всё, что отправляет любой клиент Telegram | Работа мессенджера. Регулируется условиями и политикой конфиденциальности Telegram. |
| **Google (Firebase Cloud Messaging)** | Push-токен этой установки; push-сообщения, которыми Telegram будит приложение | Доставка уведомлений при закрытом приложении. Содержимое шифрует Telegram; Google видит факт отправки, а не текст. Этим каналом пользуется каждый Android-клиент Telegram. |
| **Google (сервисы Play, на устройстве)** | Ничего не отправляется | Сканирование QR-кодов делает библиотека Google на самом устройстве. Превью карт для отправленных геопозиций загружаются у картографического провайдера, которого использует Telegram X. |

На fgram.org и разработчикам FortGram не отправляется ничего. В приложении нет телеметрии, сервисов отчётов о сбоях, аналитических и рекламных SDK. В Firebase-проекте, который используется для доставки push, Google Analytics отключён.

## Файлы экспорта настроек

*Экспорт настроек* создаёт файл (или QR-код) с родительскими правилами: идентификаторы, юзернеймы и имена разрешённых пользователей, названия групп и стикерпаков. В нём нет сообщений и никогда нет PIN. Куда попадёт этот файл, решаете вы; относитесь к нему как к любому документу с именами контактов ребёнка.

## Родители, дети и аккаунты

FortGram предназначен для установки родителем или законным опекуном на устройство ребёнка. Аккаунт Telegram создаёт и контролирует родитель; он же решает, кто может связаться с ребёнком. FortGram не собирает персональные данные детей и не может знать возраст пользователя. Возрастные требования Telegram продолжают действовать — см. [Дисклеймер](./DISCLAIMER.md).

Родительская сессия детского аккаунта (на телефоне родителя) видит те же чаты, что есть в аккаунте; это возможность Telegram, а не что-то, что FortGram добавляет или передаёт.

## Сайт

fgram.org — статический сайт на GitHub Pages. Просмотры страниц считаются через [Cloudflare Web Analytics](https://www.cloudflare.com/web-analytics/) — без cookies, без отпечатков браузера и без идентификации отдельных посетителей: мы видим, сколько людей открывают какие страницы и из каких стран, и ничего больше. GitHub может вести журнал подключений (например, IP-адреса) согласно [политике конфиденциальности GitHub](https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement). Приложение к fgram.org никогда не обращается.

## Ваш выбор

- Удаление FortGram стирает всё, что он хранил на устройстве. Сам аккаунт Telegram, его чаты и другие сессии не затрагиваются — ими управляют из настроек Telegram или из родительской сессии.
- Push-уведомления можно выключить в настройках уведомлений приложения; тогда приложение узнаёт о новых сообщениях при открытии.
- Исходный код опубликован под лицензией GPL-3.0 в [github.com/fortgram/fortgram](https://github.com/fortgram/fortgram), так что всё написанное выше можно проверить по коду.

## Изменения и контакт

Политика будет меняться вместе с приложением; дата вверху показывает актуальную версию. Вопросы: откройте issue в репозитории проекта или напишите на адрес, указанный на fgram.org.
