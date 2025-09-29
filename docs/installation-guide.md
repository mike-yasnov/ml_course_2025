# Запуск Jupyter Notebook в VS Code (macOS / Windows / Linux)

Готовый README для репозитория: как настроить окружение, выбрать ядро (kernel) и запускать ноутбуки `*.ipynb` прямо в Visual Studio Code.

---

## Содержание
- [Что понадобится](#что-понадобится)
- [Быстрый старт (универсально)](#быстрый-старт-универсально)
- [Установка по ОС](#установка-по-ос)
  - [macOS](#macos)
  - [Windows](#windows)
  - [Linux](#linux)
- [Окружения: venv и Conda](#окружения-venv-и-conda)
- [Работа с ноутбуками в VS Code](#работа-с-ноутбуками-в-vs-code)
- [Подключение к внешнему/удалённому Jupyter-серверу](#подключение-к-внешнемуудалённому-jupyter-серверу)
- [WSL (Linux в Windows)](#wsl-linux-в-windows)
- [Частые проблемы и решения](#частые-проблемы-и-решения)
- [Полезные приёмы](#полезные-приёмы)
- [Мини-чеклист](#мини-чеклист)
- [Шаблоны для проекта](#шаблоны-для-проекта)

---

## Что понадобится

- **VS Code** (актуальная версия)
- Расширения VS Code:
  - **Python** (Microsoft)
  - **Jupyter** (Microsoft)
- **Python 3.9+** (рекомендовано 3.10–3.12)
- (Опционально) **Conda/Miniconda/Mambaforge** — для conda-окружений

---

## Быстрый старт (универсально)

1. Установите VS Code и Python.
2. В VS Code установите расширения **Python** и **Jupyter**.
3. В корне проекта создайте и активируйте виртуальное окружение, затем установите ядро:
   ```bash
   # Создать окружение
   python -m venv .venv

   # Активировать:
   # macOS/Linux:
   source .venv/bin/activate
   # Windows (PowerShell):
   .venv\Scripts\Activate.ps1

   # Обновить pip и поставить зависимости для ядра
   python -m pip install --upgrade pip
   pip install ipykernel jupyter

   # Зарегистрировать kernel в системе
   python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
   ```
4. Создайте/откройте `*.ipynb` и в правом верхнем углу выберите ядро "Python (myenv)".
5. Запускайте ячейки `Shift+Enter`.

---

## Установка по ОС

### macOS

Через Homebrew (рекомендуется):

```bash
# Установить Homebrew, если не установлен: см. инструкцию на сайте
# https://brew.sh
brew install --cask visual-studio-code
brew install python
```

Окружение и ядро:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install ipykernel jupyter
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

Полезно: VS Code → Command Palette (`⇧⌘P`) → "Shell Command: Install 'code' command in PATH", чтобы открывать проект командой `code .`.

---

### Windows

Установка:

```powershell
# Python (убедитесь, что при установке стоит "Add Python to PATH")
winget install -e --id Python.Python.3.12

# Visual Studio Code
winget install -e --id Microsoft.VisualStudioCode
```

Окружение и ядро (PowerShell):

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
py -m pip install --upgrade pip
pip install ipykernel jupyter
py -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

Если PowerShell блокирует скрипты активации, выполните (однократно) от имени текущего пользователя:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

---

### Linux

Ubuntu/Debian:

```bash
sudo apt update
sudo apt install -y python3 python3-venv python3-pip
# VS Code (через Snap — проще всего)
sudo snap install code --classic
```

Fedora:

```bash
sudo dnf install -y python3 python3-venv python3-pip code
```

Arch:

```bash
sudo pacman -Syu python python-pip code
```

Окружение и ядро (общие шаги):

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install ipykernel jupyter
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

---

## Окружения: venv и Conda

**Вариант A: venv (минимализм)**

```bash
python -m venv .venv
# Активировать: см. раздел по вашей ОС
pip install ipykernel jupyter
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

**Вариант B: Conda/Miniconda/Mambaforge (часто для Data/ML)**

```bash
# Создать окружение с нужной версией Python
conda create -n cv python=3.11 -y
conda activate cv

# Поставить ядро
pip install ipykernel jupyter
python -m ipykernel install --user --name cv --display-name "Python (cv)"
```

VS Code обычно сам видит conda-окружения. Если не видит — установите `ipykernel` как выше и выберите ядро вручную.

---

## Работа с ноутбуками в VS Code

- **Создать**: File → New File… → Jupyter Notebook (или создайте `*.ipynb`).
- **Открыть**: клик по `*.ipynb` — появится интерфейс ноутбука.
- **Выбор ядра**: правый верхний угол → Select Kernel → «Python (myenv)» (или ваше).
- **Запуск ячеек**: кнопка ▶ рядом с ячейкой или `Shift+Enter`.
- **Перезапуск ядра**: иконка ↻ (Restart Kernel).
- **Экспорт**: меню … вверху → Export as (HTML, PDF*, Python).  
  \* Для PDF может понадобиться LaTeX/виртуальный принтер.

---

## Подключение к внешнему/удалённому Jupyter-серверу

Если сервер уже запущен (локально/удалённо):

```bash
jupyter lab   # или jupyter notebook
# URL обычно вида: http://127.0.0.1:8888/?token=...
```

В VS Code: Command Palette → "Jupyter: Specify Jupyter server for connections" → вставьте URL.  
Удобно для удалённых машин/GPU-серверов.

---

## WSL (Linux в Windows)

```powershell
wsl --install
```

Далее внутри WSL (Ubuntu и т.п.):

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install ipykernel jupyter
python -m ipykernel install --user --name wsl-env --display-name "Python (WSL)"
```

Откройте папку WSL в VS Code через расширение Remote - WSL и выберите ядро "Python (WSL)".

---

## Частые проблемы и решения

- **Нет доступных ядер / окружение не видно**  
  Убедитесь, что установлены `ipykernel` и ядро зарегистрировано:
  ```bash
  pip install ipykernel
  python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
  ```
  В VS Code также выполните "Python: Select Interpreter" (выберите интерпретатор из `.venv`/conda), затем "Jupyter: Select Kernel".

- **Застревает на “Starting kernel…”**  
  Обновите зависимости:
  ```bash
  python -m pip install --upgrade pip jupyter ipykernel traitlets jupyter_client
  ```
  Перезапустите VS Code. Проверьте, что выбранный интерпретатор существует.

- **Конфликт системного Python и окружений**  
  Работайте в отдельном окружении (`.venv`/conda) и выберите его в VS Code.

- **Прокси/корпсеть**  
  Настройте `HTTP_PROXY`/`HTTPS_PROXY` на уровне системы и в VS Code (`settings.json`), при необходимости установите корпоративные сертификаты.

- **Windows PowerShell блокирует активатор**  
  Выполните:
  ```powershell
  Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
  ```

- **WSL: пакеты не видны**  
  Устанавливайте пакеты внутри WSL и регистрируйте там ядро.

- **GPU-библиотеки (PyTorch/TensorFlow) не подхватываются**  
  Ставьте их в конкретное окружение и выбирайте соответствующее ядро. Следуйте матрице совместимости CUDA/драйверов у фреймворка.

---

## Полезные приёмы

- **Python-файлы как ноутбук**: размечайте ячейки комментариями `# %%`, используйте Run Cell / Interactive Window.
- **Автовыбор интерпретатора для рабочего места**: добавьте `.vscode/settings.json`.

macOS/Linux:

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python"
}
```

Windows:

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}\\.venv\\Scripts\\python.exe"
}
```

- **Фиксация зависимостей**:
  ```bash
  pip freeze > requirements.txt
  pip install -r requirements.txt
  # conda:
  conda env export > env.yml
  conda env create -f env.yml
  ```

---

## Мини-чеклист

- Установлены VS Code + расширения Python и Jupyter
- Создано и активировано окружение (`.venv`/conda)
- Установлены `ipykernel` (+ при необходимости `jupyter`)
- Зарегистрировано ядро (`ipykernel install …`)
- В ноутбуке выбрано корректное ядро
- Ячейки исполняются (`Shift+Enter`)

---

## Шаблоны для проекта

Структура:

```text
project/
├─ .vscode/
│  └─ settings.json
├─ notebooks/
├─ src/
├─ data/
├─ .venv/              # не коммитить
├─ requirements.txt
└─ README.md
```

Пример `.vscode/settings.json`:

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "jupyter.askForKernelRestart": true,
  "jupyter.interactiveWindowMode": "multiple",
  "python.analysis.typeCheckingMode": "basic"
}
```
