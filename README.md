# 🚀 Hello Pages · React SPA

<p align="center">
  <b>Одностраничное приложение (SPA) на React + TypeScript с настоящим CI/CD на GitHub Pages</b>
</p>

<p align="center">
  <a href="https://chrisredfield48.github.io/hello-pages/">
    <img alt="Live Site" src="https://img.shields.io/badge/site-live-brightgreen?style=for-the-badge&logo=githubpages">
  </a>
  <img alt="React" src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
  <img alt="Vite" src="https://img.shields.io/badge/Vite-5-646CFF?style=for-the-badge&logo=vite&logoColor=white">
  <img alt="GitHub Actions" src="https://img.shields.io/badge/GitHub_Actions-CI%2FCD-2088FF?style=for-the-badge&logo=githubactions&logoColor=white">
</p>

---

## 🔗 Ссылки

- 🌐 **Живой сайт:** [https://chrisredfield48.github.io/hello-pages/](https://chrisredfield48.github.io/hello-pages/)
- 🚀 **Deployments:** [История развёртываний](https://github.com/ChrisRedfield48/hello-pages/deployments)
- ⚙️ **Environments:** [Настройки окружения](https://github.com/ChrisRedfield48/hello-pages/settings/environments)

---

## ✨ О проекте

**Hello Pages** — это учебный React-проект, созданный для отработки навыков настройки **настоящего Continuous Deployment (CD)**. 

В отличие от простых статических сайтов, здесь код проходит полноценный цикл проверки: линтинг, проверку типов (TypeScript), unit-тесты и production-сборку. И только если **все проверки пройдены успешно**, проект деплоится на GitHub Pages.

### 🖼 Скриншот сайта

![Скриншот Hello Pages](./docs/screenshot-main.png)
*(Замените этот путь на свой скриншот, если добавите его в папку `docs/`)*

---

## 🛠 Технологический стек

| Технология | Роль |
|---|---|
| **React 18** | Библиотека для построения пользовательских интерфейсов |
| **TypeScript** | Статическая типизация для безопасного кода |
| **Vite** | Современный и быстрый сборщик фронтенда |
| **Vitest** | Фреймворк для юнит-тестирования |
| **ESLint** | Линтер для поиска проблем в коде |
| **GitHub Actions** | CI/CD пайплайн |
| **GitHub Pages** | Хостинг и раздача статики через CDN |

---

## 🏗 Архитектура CI/CD

В проекте реализован паттерн **Continuous Deployment**. Деплой (Job 2) не начнётся, пока не пройдут все проверки (Job 1). Это достигается благодаря директиве `needs: ci`.

```mermaid
flowchart TD
    A[Push to main] --> B[Job: CI]
    B --> C[Lint eslint]
    C --> D[Type Check tsc]
    D --> E[Test vitest]
    E --> F[Build vite]
    F --> G[Upload Artifact dist/]
    G --> H[Job: Deploy needs: ci]
    H --> I[GitHub Pages Environment]
    I --> J[Live Site Updated]
    
    C -->|Fail| K[Pipeline Failed]
    D -->|Fail| K
    E -->|Fail| K
    F -->|Fail| K
    
    style A fill:#2088FF,color:#fff
    style J fill:#00d4ff,color:#000
    style K fill:#ff2e88,color:#fff
```

1. **CI (Continuous Integration):** При каждом пуше в `main` запускается линтер, проверка типов, тесты и сборка (`dist/`).
2. **Передача артефакта:** Собранная папка `dist` сохраняется в GitHub Actions.
3. **CD (Continuous Deployment):** Джоба `deploy` скачивает готовый артефакт и публикует его в окружение `github-pages`.

---

## 📁 Структура проекта

```mermaid
graph TD
    A[hello-pages] --> B[.github/workflows/]
    A --> C[src/]
    A --> D[public/]
    A --> E[vite.config.ts]
    A --> F[package.json]
    
    B --> B1[ci-cd.yml]
    
    C --> C1[components/]
    C --> C2[App.tsx]
    C --> C3[App.test.tsx]
    C --> C4[index.css]
    C --> C5[main.tsx]
    
    C1 --> C1a[Header.tsx]
    C1 --> C1b[About.tsx]
    C1 --> C1c[Projects.tsx]

    style A fill:#61DAFB,stroke:#000,color:#000
    style B1 fill:#2088FF,stroke:#000,color:#fff
```

---

## 💻 Локальная разработка

### Предварительные требования
Убедитесь, что у вас установлен Node.js (версия 20+) и Docker (для изолированного запуска тестов).

### 1. Установка зависимостей
```bash
npm ci
```

### 2. Запуск в режиме разработки
```bash
npm run dev
```
Откройте [http://localhost:5173/hello-pages/](http://localhost:5173/hello-pages/) в браузере.

### 3. Запуск проверок (как в CI)
```bash
# Линтинг
npm run lint

# Проверка типов
npm run type-check

# Тесты
npm test -- --run

# Production-сборка
npm run build
```

---

## 🚀 Как обновить сайт

Процесс деплоя полностью автоматизирован и не требует ручного создания тегов (в отличие от GitHub Releases):

1. Внесите изменения в код (например, в `src/components/About.tsx`).
2. Закоммитьте и запушьте изменения:
   ```bash
   git add .
   git commit -m "feat: update content"
   git push origin main
   ```
3. GitHub Actions автоматически запустит проверки. Если CI пройдёт зелёным, новая версия появится на сайте через 1–2 минуты.

---

## ⚠️ Важное примечание по конфигурации

В файле `vite.config.ts` жестко задан базовый путь:
```typescript
base: '/hello-pages/',
```
Это **обязательное условие** для корректной работы на GitHub Pages. Если вы переименуете репозиторий, вам придется изменить это значение, иначе сайт откроется с белым экраном (ошибки 404 на загрузку JS/CSS файлов).

---

## 📄 Лицензия

Этот проект распространяется под лицензией MIT.
