# Команды установки (урок 1.3)

Копируйте по одной, чтобы не набирать руками и не ловить опечатки.

---

## Проверить Python

```bash
python --version
```

Если команда не найдена — попробуйте:

```bash
python3 --version
```

Нужно 3.10 или выше.

---

## Установить OpenCode 2

**Linux / macOS / WSL:**

```bash
curl -fsSL https://opencode.ai/v2/install | bash
```

**macOS через Homebrew:**

```bash
brew install anomalyco/tap/opencode-v2
```

**Windows:** скачать OpenCode Desktop с <https://opencode.ai/download>
(простой путь) или поставить WSL и выполнить команду для Linux.

---

## Проверить версию

```bash
opencode --version
```

Должно быть `2.x`.

---

## Если раньше стоял OpenCode 1

```bash
opencode upgrade --major
```

Или сначала удалить V1 пакетным менеджером, потом поставить V2.

---

## Открыть Web-интерфейс

```bash
opencode pair
```

Команда печатает одноразовую ссылку и QR-код. Ссылка живёт 5 минут,
вход в браузере сохраняется 30 дней.

---

## Если что-то не открылось

```bash
opencode service status
```

```bash
opencode service set port 4096
opencode service restart
opencode pair
```

---

## Python без Anaconda

Достаточно любого Python 3.10+:

- **Anaconda** — <https://www.anaconda.com/download>, если хочется сразу
  с научными библиотеками (≈ 600 МБ) — рекомендуемый путь;
- **python.org/downloads** — обычный установщик, «далее-далее».

Библиотеки (pandas, matplotlib, seaborn, mne) в курсе ставит сам агент
в отдельное окружение `.venv/` — заранее их устанавливать не нужно.
