# Як користуватися GitHub і організацією KSE-codebase

Універсальна інструкція для **людей, які не працювали з Git/GitHub**, і для **AI-агентів** (Claude Code, Codex). Усі команди — копі-пейст. Основна ОС — **macOS**; для Windows є примітки.

> Якщо ти AI-агент, який читає цей файл, — гортай одразу до розділу **[10. Шпаргалка для агента](#10-шпаргалка-для-агента)**.

---

## 0. Логіка за 30 секунд

- **Git** — програма, що зберігає історію змін у папці (локально, у тебе на комп'ютері).
- **GitHub** — сайт, куди ці папки (репозиторії) заливаються, щоб ділитися й зберігати в хмарі.
- **Організація `KSE-codebase`** — спільний простір KSE. Усередині — **репозиторії**, по одному на кожну автоматизацію/проєкт.
- **Юніт** (President Office, Business School…) = **Team** + префікс у назві репо (`po-`, `bs-`…). Папок усередині GitHub немає — групуємо префіксом і командами.
- **Ти працюєш так:** копіюєш репо до себе (`clone`) → міняєш файли → зберігаєш зміни (`commit`) → відправляєш назад на GitHub (`push`). Або: маєш готову папку → створюєш з неї нове репо в організації одним рухом.

Повні правила іменування й доступів — у [`STRUCTURE.md`](STRUCTURE.md).

---

## 1. Відкрити термінал

**macOS:** натисни `Cmd + Пробіл`, набери `Terminal`, `Enter`. Відкриється чорне/біле вікно — це термінал.

**Windows:** `Пуск` → набери `PowerShell` → `Enter`. (Команди `git`/`gh` однакові.)

Термінал виконує команди, які ти вставляєш і натискаєш `Enter`. Вставити: `Cmd + V` (Mac) / `Ctrl + V` (Windows).

---

## 2. Встановити інструменти (один раз)

Скопіюй увесь блок, встав у термінал, `Enter`. На macOS спершу ставимо **Homebrew** (менеджер програм), потім усе інше.

```bash
# 1) Homebrew (якщо ще нема). Пропусти, якщо команда `brew` вже працює.
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2) git (система контролю версій) + gh (офіційний клієнт GitHub)
brew install git gh

# 3) Node.js (потрібен для Claude Code)
brew install node
```

Установити **Claude Code** і/або **Codex** (AI-агенти в терміналі):

```bash
# Claude Code
npm install -g @anthropic-ai/claude-code

# Codex (OpenAI) — за бажанням
npm install -g @openai/codex
```

Перевір, що все стало:

```bash
git --version && gh --version && node --version
```

Якщо кожна команда вивела номер версії — усе готово.

---

## 3. Авторизуватися в GitHub через термінал (один раз)

```bash
gh auth login
```

Далі відповідай на питання стрілками/`Enter`:

1. **What account do you want to log into?** → `GitHub.com`
2. **What is your preferred protocol?** → `HTTPS`
3. **Authenticate Git with your GitHub credentials?** → `Yes`
4. **How would you like to authenticate?** → `Login with a web browser`
5. Термінал покаже **one-time code** (напр. `A1B2-C3D4`) — запам'ятай його, натисни `Enter`, у браузері встав код і підтверди.

> ⚠️ **Важливо для заливки автоматизацій (GitHub Actions).** Одразу додай розширений доступ, інакше файли `.github/workflows/*.yml` не заллються:
> ```bash
> gh auth refresh -h github.com -s workflow -s admin:org
> ```

Перевір, що ти залогінений:

```bash
gh auth status
```

---

## 4. Скопіювати (клонувати) репозиторій організації

```bash
# перейти в теку, де тримаєш проєкти (створюємо, якщо нема)
mkdir -p ~/kse && cd ~/kse

# клонувати конкретне репо (заміни назву на потрібну)
gh repo clone KSE-codebase/po-moodle-to-notion

# зайти в нього
cd po-moodle-to-notion
```

Побачити список усіх репо організації:

```bash
gh repo list KSE-codebase --limit 100
```

---

## 5. Базовий цикл: змінив → зберіг → залив

Усередині репо, після того як щось поміняв у файлах:

```bash
git add -A                       # додати всі зміни
git commit -m "Опис що я змінив" # зберегти їх з підписом
git push                         # відправити на GitHub
```

Підтягнути свіжі зміни інших людей **перед** роботою:

```bash
git pull
```

Подивитися, що змінилося / у якому стані репо:

```bash
git status
```

---

## 6. Залити ГОТОВУ локальну папку як нове репо в організацію

Найчастіший сценарій: у тебе є папка з проєктом/промтами, і ти хочеш зробити з неї репо в `KSE-codebase`.

```bash
# 1) зайти в свою папку
cd ~/шлях/до/твоєї/папки

# 2) ініціалізувати git (один раз на папку)
git init -b main
git add -A
git commit -m "Initial import"

# 3) створити репо в організації і одразу залити
#    назва = префікс юніту + що це (напр. po-my-automation)
#    --private = закрите; --public = відкрите
gh repo create KSE-codebase/po-my-automation --private --source=. --push
```

Готово — репо створене й залите. Посилання термінал виведе сам.

> **Секрети (паролі, токени) НЕ комітимо.** Створи файл `.gitignore` і додай туди `.env`, `*credentials*.json`, `token.json`. Значення секретів клади в **GitHub → Settings → Secrets and variables → Actions** (для Actions) або в Script Properties (для Apps Script).

### Якщо є `.github/workflows/*.yml` (автоматизація за розкладом)
Це заллється, **лише якщо** ти виконав крок з `-s workflow` у розділі 3. Якщо забув — дороби:
```bash
gh auth refresh -h github.com -s workflow
git push
```

---

## 7. Найпростіший шлях: доручити все агенту (Claude Code / Codex)

Замість того щоб писати команди самому, можна попросити агента. Запусти його **всередині потрібної папки**:

```bash
cd ~/шлях/до/папки
claude          # запустити Claude Code   (або:  codex)
```

Далі просто напиши агенту завдання людською мовою. Готові промти для копіювання:

**Залити поточну папку новим репо:**
> «Залий цю папку новим **приватним** репозиторієм у GitHub-організацію **KSE-codebase** з назвою **po-НАЗВА**. Спочатку створи короткий README українською з описом що це і як користуватися, додай `.gitignore` для секретів, потім `git init`, commit і `gh repo create ... --source=. --push`. Якщо в папці є `.github/workflows`, а токен без scope `workflow` — поклади workflow тимчасово в `workflow/` і напиши мені, що треба зробити.»

**Оновити наявне репо:**
> «Внеси такі зміни: … Потім зроби `git add -A`, `commit` зі змістовним описом і `git push`.»

**Розібратися з чужим репо:**
> «Проаналізуй це репо, поясни українською що воно робить, які секрети йому потрібні і як його запустити.»

Агент виконає команди сам і покаже результат. Ти лише підтверджуєш дії.

---

## 8. Увімкнути автоматизацію (GitHub Actions) після заливки

Якщо репо містить `.github/workflows/*.yml`:

1. Відкрий репо на GitHub → вкладка **Actions**.
2. Якщо просить — натисни **I understand my workflows, enable them**.
3. Вибери потрібний workflow зліва → **Run workflow** (запуск вручну для тесту).
4. Секрети додаються в **Settings → Secrets and variables → Actions → New repository secret** (назви бери з README репо / файлу `.env.example`).

> Якщо workflow лежить у теці `workflow/` (а не `.github/workflows/`) — його поклали туди, бо заливали без scope `workflow`. Перенеси: на GitHub *Add file → Create new file*, назва `.github/workflows/sync.yml`, встав вміст із `workflow/sync.yml`, commit.

---

## 9. Часті проблеми

| Симптом | Причина / рішення |
|---|---|
| `command not found: git` (чи `gh`, `brew`) | Не встановлено — див. розділ 2. |
| `refusing to allow an OAuth App to create ... workflow without workflow scope` | Токен без scope. `gh auth refresh -h github.com -s workflow`, тоді `git push`. |
| `Permission denied` / `not found` при push в KSE-codebase | Тебе ще не додали в організацію, або нема прав. Напиши адміну org. |
| `Authentication failed` | `gh auth login` заново (розділ 3). |
| `updates were rejected ... fetch first` | Хтось залив зміни раніше. Зроби `git pull`, потім `git push`. |
| Випадково закомітив пароль/токен | Негайно **відклич** цей токен у сервісі, заміни на новий, і напиши адміну — історію треба чистити. |
| Не знаю назву юніт-префікса | Дивись [`STRUCTURE.md`](STRUCTURE.md). |

---

## 10. Шпаргалка для агента

Стислий контракт для AI-агента, який заливає проєкт у цю організацію:

- **Організація:** `KSE-codebase`. Одне репо = одна автоматизація/проєкт.
- **Назва репо:** `<префікс-юніту>-<що-робить>`, lowercase, через дефіс (President Office = `po-`). Див. `STRUCTURE.md`.
- **Дефолт видимості:** `--private`, якщо користувач явно не сказав `--public`.
- **README обов'язковий:** що це, юзкейс, налаштування, секрети, як тестувати.
- **Ніяких секретів у коді.** Додай `.gitignore` (`.env`, `*credentials*.json`, `token.json`). Значення — у GitHub Secrets або Apps Script Script Properties. Дай `.env.example` з плейсхолдерами.
- **Створення+заливка:** `git init -b main && git add -A && git commit -m "..." && gh repo create KSE-codebase/<name> --private --source=. --push`.
- **Workflow-обмеження:** якщо `gh auth status` показує токен **без** scope `workflow`, а в репо є `.github/workflows/*.yml` — заливка впаде. Або попроси користувача `gh auth refresh -h github.com -s workflow`, або поклади файл у `workflow/sync.yml` і додай у README секцію активації. **Ніколи не втрачай сам файл.**
- **Ідемпотентність:** якщо репо вже існує — не роби `gh repo create` вдруге; додай remote і `git push`.
- **Звітуй чесно:** що залито, що лишилось зробити руками, які секрети треба додати.
```
git init -b main
git add -A
git commit -m "Initial import"
gh repo create KSE-codebase/<prefix>-<name> --private --source=. --push
```
