# Контакты — проектная работа спринта 2

Рефакторинг двух фильтров приложения на Kotlin и Jetpack Compose с использованием общего дженерик-компонента.

Исходный проект: https://github.com/interactivetextbook/android-contacts, коммит `b6744fa`.

## Что изменено

- `CountryCodeBottomSheet` использует `CommonBottomSheet<CountryCode>`.
- `MessengersBottomSheet` использует `CommonBottomSheet<MessagingApp>`.
- В общий компонент передаются заголовок, список вариантов, выбранное множество, обработчик обновления и обработчик закрытия.
- Отображение вариантов осталось в `CountryCodeOption` и `MessengerOption`: сохранены подписи, флажки и обработчики множественного выбора через `Set`.
- Удалены повторяющиеся контейнеры, заголовки, списки, обработчики нажатия на строку и неиспользуемые импорты.

Сигнатуры публичных функций и общий компонент `CommonBottomSheet` сохранены. Сортировка и логика фильтрации остались в исходном проекте. Итоговый diff двух файлов: 30 добавленных и 156 удалённых строк.

## Открытие и сборка

Откройте **эту папку целиком** в Android Studio и дождитесь синхронизации Gradle. Для исходной конфигурации проекта нужны JDK 21 и Android SDK Platform 35. Версии Gradle, Android Gradle Plugin, Kotlin и библиотек сохранены из исходного проекта; Gradle Wrapper входит в комплект.

Сборка в PowerShell при настроенных `JAVA_HOME` и `ANDROID_HOME`:

```powershell
.\gradlew.bat :app:assembleDebug
```

APK появится в `app/build/outputs/apk/debug/app-debug.apk`.

## Самопроверка

Проверены параметры обоих вызовов `CommonBottomSheet`, типы выбранных множеств, сохранение обработчиков флажков и закрытия, а также отсутствие ошибок форматирования в `git diff --check`.

Сборка `:app:assembleDebug` выполнена успешно, подпись полученного APK проверена через `apksigner`. APK находится в `app/build/outputs/apk/debug/app-debug.apk` (около 17,3 МБ). Подробности — в `VERIFICATION.md`. Запуск на устройстве или эмуляторе пока не проверен.

На этом компьютере JDK 21 доступен в `C:\Program Files\JetBrains\PyCharm2025.3\jbr`, а SDK и кэш Gradle находятся рядом с проектом, в `../.tools`. Для повторной сборки из папки проекта:

```powershell
$env:JAVA_HOME = 'C:\Program Files\JetBrains\PyCharm2025.3\jbr'
$toolsPath = (Resolve-Path '..\.tools').Path
$env:ANDROID_HOME = Join-Path $toolsPath 'android-sdk'
$env:ANDROID_USER_HOME = Join-Path $toolsPath 'android-user'
$env:GRADLE_USER_HOME = Join-Path $toolsPath 'gradle'
.\gradlew.bat --no-daemon --offline --max-workers=2 :app:assembleDebug
```

Для ручной проверки после запуска:

1. Открыть страны, выбрать несколько кодов, закрыть и повторно открыть список: выбранные пункты должны сохраниться. Исходный экран закрывает фильтр после каждого выбора; для выбора следующего пункта нужно открыть его снова.
2. Снять один из флажков и убедиться, что остальные остаются выбранными.
3. Повторить проверку с несколькими мессенджерами.
4. Совместить оба фильтра и проверить список контактов.
5. Проверить сортировку, закрытие по крестику, свайпу и нажатию вне листа.

Задание Практикума: https://practicum.yandex.ru/trainer/ap-android-st/lesson/c3c5b05b-3e14-4622-88af-c7eb8bbcb1ed/

Чек-лист: https://practicum.yandex.ru/trainer/ap-android-st/lesson/774bae6c-6c98-44fa-81b7-0324865f1267/
