takas-api/
├── app/
│   ├── __init__.py          # create_app() fabrikası
│   ├── config.py            # Dev / Test / Prod ayarları
│   ├── extensions.py        # db, migrate, jwt gibi uzantı nesneleri
│   ├── models/              # SQLAlchemy modelleri (user.py, item.py, trade.py ...)
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── routes.py
│   │   │   ├── schemas.py
│   │   │   └── services.py
│   │   ├── users/
│   │   ├── items/
│   │   ├── trades/
│   │   ├── messages/
│   │   ├── credits/
│   │   └── admin/
│   ├── common/
│   │   ├── errors.py        # Ortak hata yönetimi
│   │   ├── decorators.py    # rol, engelleme kontrolü
│   │   └── pagination.py
│   └── tasks/               # Celery görevleri
├── migrations/
├── tests/
├── docker/
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── .env.example
└── wsgi.py