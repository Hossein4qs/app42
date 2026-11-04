# فهرست ابزارها و APIها (پیش‌نویس)

> **توجه:** این فایل فقط از روی نام متدهای خروجی `grep` ساخته شده، نه از روی خود کد.
> ستون «فایل» را خودت با جست‌وجوی نام متد در `src` پر کن (مثلاً با Ctrl+Shift+F در VS Code).
> علامت ✅ یعنی تأیید شد، ❓ یعنی هنوز بررسی نشده.

## ساختار پوشه‌های src

| پوشه | احتمالاً شامل |
|---|---|
| `src/methods` | منطق هر ابزار ❓ |
| `src/lib/methods` | توابع کمکی مشترک برای تلگرام ❓ |
| `src/pages` | صفحه‌ی هر ابزار ❓ |
| `src/modals` | پنجره‌های پاپ‌آپ ❓ |
| `src/images_generator` | ساخت تصویر خروجی (آمار پیام‌ها، آمار تماس، تاریخ ثبت‌نام) |
| `src/languages` | ترجمه‌ها |
| `src/errors` | خطاهای سفارشی |

## نگاشت ابزارها به فایل (۲۳ ابزار)

> کاربردها از روی **نام فایل** حدس زده شده‌اند، نه از روی کد. ستون «متدهای احتمالی» را با باز کردن هر فایل تأیید کن.
> `src/methods/AbstractMethod.tsx` کلاس پایه‌ی همه‌ی ابزارهاست و `src/lib/methods/dialogs.ts` و `messages.ts` توابع مشترک دریافت چت‌ها و پیام‌ها هستند.

### ابزارهای تلگرام

| فایل | کاربرد احتمالی | متدهای احتمالی | عملیات طولانی؟ |
|---|---|---|---|
| `Administered.tsx` | کانال‌ها و گروه‌هایی که ادمینشان هستی | `messages.GetDialogs`، `channels.GetChannels` | بله |
| `AnimatedMessages.tsx` | ساخت پیام‌های متحرک | `messages.SendMessage`، `EditMessage` | بله |
| `Birthdays.tsx` | تولد مخاطبین | `contacts.GetContacts`، `users.GetFullUser` | بله |
| `BoostLevels.tsx` | سطح بوست کانال‌ها | `channels.GetChannels` ❓ | خیر |
| `CallsStat.tsx` | آمار تماس‌ها | `messages.GetHistory`، `messages.Search` | بله |
| `ChannelsRegistration.tsx` | تاریخ ثبت کانال‌ها | `channels.GetChannels` ❓ | خیر |
| `ClearBlacklist.tsx` | پاک‌کردن لیست بلاک | `contacts.GetBlocked`، `contacts.Unblock` | بله (دسته‌ای) |
| `ClearDialogMembers.tsx` | حذف اعضای یک گروه | `channels.GetParticipants`، `EditBanned`، `messages.DeleteChatUser` | بله (دسته‌ای) |
| `CommonChatsTop.tsx` | برترین گروه‌های مشترک | `messages.GetCommonChats` | بله |
| `ContactsAnalysis.tsx` | تحلیل مخاطبین | `contacts.GetContacts`، `users.GetFullUser` | بله |
| `DialogJoined.tsx` | تاریخ عضویت در چت‌ها | `messages.GetDialogs`، `channels.GetParticipants` ❓ | بله |
| `GetId.tsx` | یافتن شناسه‌ی کاربر یا چت | `contacts.ResolveUsername` | خیر |
| `ImportMessages.tsx` | ایمپورت تاریخچه‌ی چت | `messages.*HistoryImport`، `UploadImportedMedia` | بله (آپلود) |
| `InactiveChannels.tsx` | کانال‌های بی‌فعالیت | `channels.GetInactiveChannels`، `LeaveChannel` | بله (خروج دسته‌ای) |
| `MessagesStat.tsx` | آمار پیام‌ها | `messages.GetHistory`، `messages.Search` | بله |
| `MonetizationList.tsx` | درآمد و Stars | `payments.GetStarsRevenueStats` ❓ | خیر |
| `OwnChannels.tsx` | کانال‌های خودت | `channels.GetChannels`، `messages.GetDialogs` | بله |
| `RecordsStat.tsx` | آمار ویس و ویدیو‌پیام | `messages.Search` | بله |
| `StoriesStat.tsx` | آمار استوری‌ها | `stories.*` | بله |

### ابزارهای TON

| فایل | کاربرد احتمالی |
|---|---|
| `TonContactsWithNFT.tsx` | مخاطبینی که NFT دارند |
| `TonJettonsAnalysis.tsx` | تحلیل جتون‌ها |
| `TonTransactions.tsx` | تراکنش‌ها |

## ۱. احراز هویت و نشست (مهم برای فاز ۱)

| متد | کاربرد |
|---|---|
| `auth.SendCode` | ارسال کد ورود |
| `auth.SignIn` | ورود با کد |
| `account.GetPassword` و `auth.CheckPassword` | تأیید رمز دوم (2FA) |
| `auth.LogOut` | خروج |
| `help.GetNearestDc` | پیداکردن نزدیک‌ترین دیتاسنتر |
| `help.GetCountriesList` | فهرست کشورها برای شماره‌ی تلفن |

**فایل:** ❓
**نکته برای فاز ۱:** رمز 2FA و Session در همین مسیر دست‌به‌دست می‌شوند. بررسی کن Session کجا و چطور ذخیره می‌شود.

## ۲. پیام‌ها و تحلیل چت‌ها

| متد | کاربرد احتمالی | عملیات طولانی؟ |
|---|---|---|
| `messages.GetHistory` | خواندن تاریخچه‌ی پیام‌ها | بله (صفحه‌به‌صفحه) |
| `messages.Search` | جست‌وجو در پیام‌ها | بله |
| `messages.GetDialogs` | فهرست چت‌ها | بله |
| `messages.GetFullChat` و `users.GetFullUser` | اطلاعات کامل چت یا کاربر | خیر |
| `messages.GetCommonChats` | گروه‌های مشترک | خیر |
| `contacts.ResolveUsername` و `contacts.Search` | یافتن کاربر یا چت | خیر |
| `users.GetUsers` | اطلاعات کاربران | خیر |

**فایل:** ❓
**نکته برای فاز ۴:** این‌ها پرتکرارترین و سنگین‌ترین مسیرها هستند. اگر هر ابزار حلقه‌ی جدا برای صفحه‌بندی دارد، یک تابع مشترک با مدیریت `FLOOD_WAIT` و قابلیت لغو بساز.

## ۳. مخاطبین و بلاک‌شده‌ها

| متد | کاربرد احتمالی |
|---|---|
| `contacts.GetContacts` | فهرست مخاطبین |
| `contacts.GetBlocked` | فهرست بلاک‌شده‌ها |
| `contacts.Unblock` | رفع بلاک |

**فایل:** ❓

## ۴. کانال‌ها و گروه‌ها

| متد | کاربرد احتمالی | عملیات طولانی؟ |
|---|---|---|
| `channels.GetInactiveChannels` | یافتن کانال‌های بی‌فعالیت | خیر |
| `channels.LeaveChannel` | خروج از کانال | بله (اگر دسته‌ای باشد) |
| `channels.GetParticipants` | اعضای گروه یا کانال | بله |
| `channels.EditBanned` | بن یا محدودسازی کاربر | بله (اگر دسته‌ای باشد) |
| `messages.DeleteChatUser` | حذف کاربر از گروه | بله (اگر دسته‌ای باشد) |
| `channels.GetChannels` | اطلاعات کانال | خیر |
| `channels.GetForumTopics` | تاپیک‌های فروم | خیر |

**فایل:** ❓
**نکته:** ابزار «چت‌های بی‌فعالیت» که در roadmap پیشنهاد دادم احتمالاً **از قبل وجود دارد** (`GetInactiveChannels`). قبل از ساختنش، ابزار موجود را بررسی و بهتر کن.

## ۵. استوری‌ها

| متد | کاربرد احتمالی |
|---|---|
| `stories.CanSendStory` و `stories.SendStory` | ارسال استوری |
| `stories.GetStoriesArchive` | آرشیو استوری‌ها |
| `stories.GetStoriesByID` | دریافت استوری با شناسه |
| `stories.GetStoryViewsList` | بازدیدکنندگان استوری |

**فایل:** ❓

## ۶. ایمپورت تاریخچه‌ی چت

| متد | کاربرد |
|---|---|
| `messages.CheckHistoryImport` | بررسی فایل پیش از ایمپورت |
| `messages.InitHistoryImport` | شروع ایمپورت |
| `messages.UploadImportedMedia` | آپلود مدیا |
| `messages.StartHistoryImport` | تکمیل ایمپورت |

**فایل:** ❓
**عملیات طولانی؟** بله (آپلود فایل‌ها). این مسیر کاندیدای اصلی لغو و ادامه است.

## ۷. سایر

| متد | کاربرد احتمالی |
|---|---|
| `fragment.GetCollectibleInfo` | اطلاعات یوزرنیم یا شماره‌ی collectible |
| `payments.GetStarsRevenueStats` | آمار درآمد Stars |
| `messages.SendMessage` / `EditMessage` / `SendMedia` | ارسال نتیجه یا پیام پیشرفت |

**فایل:** ❓

## ۸. APIهای TON (از تلگرام نیستند)

این‌ها متدهای تلگرام نیستند. از یک کلاینت جدا (به‌احتمال زیاد TonAPI) می‌آیند و باید جدا از تلگرام بررسی شوند.

| متد | کاربرد احتمالی |
|---|---|
| `accounts.getAccount` / `getAccountEvents` / `getAccountNftItems` / `getAccountJettonsBalances` / `addressParse` | اطلاعات کیف پول |
| `jettons.getJettonHolders` | دارندگان یک جتون |
| `events.getEvent` | جزئیات یک تراکنش |
| `rates.getRates` / `getChartRates` | نرخ و نمودار قیمت |
| `nft.getNftHistoryById` | تاریخچه‌ی NFT |

**فایل:** ❓
**نکته:** بررسی کن کلید API این سرویس کجای کد نوشته شده. اگر داخل کد کلاینت باشد، برای همه قابل‌مشاهده است.

## نکته درباره‌ی خروجی grep

موارد زیر **درخواست** نیستند، بلکه نوع پاسخ هستند و در بررسی نوع (`instanceof`) استفاده می‌شوند. در فهرست ابزارها نادیده‌شان بگیر:

`MessagesSlice`، `Contacts`، `Messages`، `Dialogs`، `DialogsSlice`، `ChannelMessages`، `Chats`، `ChatFull`، `SentCode`، `Password`، `BlockedSlice`، `ChannelParticipants`، `ChannelParticipantsNotModified`، `MessagesNotModified`، `TypeMessages`، `TypeChannelParticipants`، `TypeCollectibleInfo`، `CountriesList`

## کارهای باقی‌مانده

- [ ] ستون «فایل» هر بخش را پر کن
- [ ] مشخص کن هر صفحه در `src/pages` به کدام بخش بالا وصل است
- [ ] علامت ❓ را در بخش‌هایی که بررسی کردی به ✅ تبدیل کن
- [ ] محل ذخیره‌ی Session و کلید TonAPI را پیدا و یادداشت کن
