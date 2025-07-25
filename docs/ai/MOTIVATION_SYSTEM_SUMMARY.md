# Мотивационная система и обратная связь в Telegram Wine Bot

## 1. Персонализация мотивационных уведомлений и AI-ответов

- Для каждого пользователя формируется персональный промпт с учётом его имени (или username), который добавляется в начало мотивационного сообщения или AI-ответа.
- Имя пользователя берётся из Telegram (first_name, last_name, username) и сохраняется/обновляется при каждом взаимодействии с ботом.
- Это позволяет делать мотивационные сообщения и AI-ответы более адресными и вовлекающими.
- Имя пользователя добавляется как в массовую рассылку мотивации (cron), так и во все индивидуальные AI-ответы (например, через WebApp или команду /ai).

## 2. Логика обратной связи и возврата в главное меню

- После любого варианта обратной связи (лайк, сложно, легко, комментарий) бот:
  - сохраняет отзыв в базу данных;
  - отправляет благодарность;
  - **автоматически возвращает пользователя в главное меню обучения** (вызывает startLearning).
- Это реализовано как для кнопок, так и для текстовых комментариев.
- Такой UX делает сценарий замкнутым: пользователь всегда возвращается к обучению после фидбека.

## 3. Технические детали

- Для массовой рассылки используется cron-триггер в wrangler.toml (обычно 09:00 и 22:00 по Москве).
- Все промпты для AI-генерации мотивации и консультаций формируются с учётом имени пользователя.
- Имя пользователя добавляется в промпт на уровне backend (src/index.js и src/handlers/ai.js).
- Для обратной связи используются callback query и текстовые сообщения, после которых всегда вызывается главное меню обучения.

## 4. Пример UX

1. Пользователь выбирает "Обратная связь" и отправляет комментарий или выбирает вариант.
2. Бот отвечает благодарностью и сразу показывает главное меню обучения.
3. В мотивационных уведомлениях и AI-ответах используется имя пользователя:
   - "Анна, уровень: 2, прогресс: 40%..."

---

_Документ обновлён: 2025-07-23_ 