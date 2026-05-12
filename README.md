# 🚗 Автошкола СТАРТ — Лендинг

Лендинг-страница автошколы «СТАРТ» в Алматы. Сайт с формой записи на уроки вождения, отправкой заявок в Telegram, поддержкой тем (тёмная/светлая) и двуязычностью (RU/KZ).

## Возможности

- 📱 Адаптивный дизайн (мобильный, планшет, десктоп)
- 🌙 Тёмная / светлая тема
- 🌐 Двуязычность: русский и казахский
- 📨 Отправка заявок в Telegram-бот
- ✅ Валидация формы (клиент + сервер)
- 🎨 Анимированный фон с частицами

## Структура проекта

```
├── public/                  # Статические файлы (фронтенд)
│   ├── index.html           # Главная страница
│   ├── style.css            # Стили
│   ├── theme.js             # Переключение темы
│   ├── i18n.js              # Интернационализация (RU/KZ)
│   └── assets/              # Изображения, иконки
├── api/                     # Vercel Serverless Functions
│   └── submit.js            # API эндпоинт (для Vercel)
├── server.js                # Express-сервер (для VPS/hoster.kz)
├── ecosystem.config.js      # PM2 конфигурация
├── nginx.conf               # Nginx конфигурация (шаблон)
├── Dockerfile               # Docker-образ
├── vercel.json              # Конфигурация Vercel
├── .env.example             # Шаблон переменных окружения
└── package.json
```

## Быстрый старт (локальная разработка)

```bash
# 1. Клонировать репозиторий
git clone https://github.com/ВАШ_ЛОГИН/start-avtoshkola.git
cd start-avtoshkola

# 2. Установить зависимости
npm install

# 3. Настроить переменные окружения
cp .env.example .env
# Отредактировать .env — вписать токен бота и chat_id

# 4. Запустить сервер
npm start
# Сервер доступен на http://localhost:3000
```

---

## Деплой на Hoster.kz (VPS)

### 1. Подготовка сервера

```bash
# Подключиться к VPS
ssh root@ВАШ_IP

# Обновить систему
sudo apt update && sudo apt upgrade -y

# Установить NVM и Node.js
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install --lts

# Установить PM2 и Nginx
npm install -g pm2
sudo apt install nginx -y
```

### 2. Загрузка проекта

```bash
# Клонировать репозиторий
cd /var/www
git clone https://github.com/ВАШ_ЛОГИН/start-avtoshkola.git
cd start-avtoshkola

# Установить зависимости
npm install --production
```

### 3. Настройка переменных окружения

```bash
cp .env.example .env
nano .env
```

Вписать реальные значения:
```
TELEGRAM_BOT_TOKEN=ваш_токен_бота
TELEGRAM_CHAT_ID=ваш_chat_id
PORT=3000
```

### 4. Запуск через PM2

```bash
# Запустить приложение
pm2 start ecosystem.config.js

# Настроить автозапуск при перезагрузке сервера
pm2 startup
pm2 save

# Проверить статус
pm2 status
pm2 logs start-avtoshkola
```

### 5. Настройка Nginx

```bash
# Скопировать конфигурацию
sudo cp nginx.conf /etc/nginx/sites-available/start-avtoshkola

# Отредактировать server_name (вписать ваш домен)
sudo nano /etc/nginx/sites-available/start-avtoshkola

# Активировать
sudo ln -s /etc/nginx/sites-available/start-avtoshkola /etc/nginx/sites-enabled/

# Удалить дефолтный конфиг (если нужно)
sudo rm /etc/nginx/sites-enabled/default

# Проверить и перезапустить
sudo nginx -t
sudo systemctl reload nginx
```

### 6. SSL-сертификат (HTTPS)

```bash
# Установить Certbot
sudo apt install certbot python3-certbot-nginx -y

# Получить сертификат
sudo certbot --nginx -d start-avtoshkola.kz -d www.start-avtoshkola.kz

# Автообновление (проверить)
sudo certbot renew --dry-run
```

### 7. Файрволл

```bash
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

### Обновление сайта

```bash
cd /var/www/start-avtoshkola
git pull
npm install --production
pm2 restart start-avtoshkola
```

---

## Деплой на Vercel

```bash
# Установить Vercel CLI
npm i -g vercel

# Деплой
vercel --prod
```

Добавить Environment Variables в Vercel Dashboard:
- `TELEGRAM_BOT_TOKEN`
- `TELEGRAM_CHAT_ID`

---

## Деплой через Docker

```bash
# Собрать образ
docker build -t start-avtoshkola .

# Запустить контейнер
docker run -d -p 3000:3000 --env-file .env --name avtoshkola start-avtoshkola
```

---

## Переменные окружения

| Переменная | Описание | Обязательно |
|---|---|---|
| `TELEGRAM_BOT_TOKEN` | Токен Telegram-бота | ✅ |
| `TELEGRAM_CHAT_ID` | ID чата для заявок | ✅ |
| `PORT` | Порт сервера (по умолчанию 3000) | ❌ |

## Технологии

- **Frontend:** HTML5, CSS3, Vanilla JS
- **Backend:** Node.js, Express
- **API:** Telegram Bot API
- **Деплой:** PM2, Nginx, Docker, Vercel

---

© 2025 Автошкола СТАРТ, Алматы
