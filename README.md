# 🚀 Kompletna Dokumentacja API - System Zarządzania Serwerami

![API Version](https://img.shields.io/badge/API%20Version-v1.0-blue)
![License](https://img.shields.io/badge/License-Proprietary-red)
![Status](https://img.shields.io/badge/Status-Production-green)
![Uptime](https://img.shields.io/badge/Uptime-99.9%25-brightgreen)

## 📚 Spis Treści

### 🏗️ Podstawy API
1. [Wprowadzenie](#wprowadzenie)
2. [Szybki Start](#szybki-start)
3. [Uwierzytelnianie](#uwierzytelnianie)
4. [Standardy i Konwencje](#standardy-i-konwencje)
5. [Limity i Błędy](#limity-i-błędy)

### 🖥️ Zarządzanie Serwerami
6. [Serwery - CRUD](#zarządzanie-serwerami)
7. [Uruchamianie i Zatrzymywanie](#power-management)
8. [Konfiguracja Zasobów](#resource-management)
9. [Statystyki i Metryki](#server-metrics)
10. [Backup i Restore](#backup-operations)

### 📁 Zarządzanie Plikami
11. [System Plików](#zarządzanie-plikami)
12. [Upload i Download](#file-operations)
13. [Kompresja i Archiwizacja](#file-compression)
14. [Uprawnienia Plików](#file-permissions)

### 🗄️ Bazy Danych
15. [Zarządzanie Bazami](#bazy-danych)
16. [MySQL Operations](#mysql-operations)
17. [PostgreSQL Operations](#postgresql-operations)
18. [MongoDB Operations](#mongodb-operations)
19. [Redis Operations](#redis-operations)

### 👥 Zarządzanie Użytkownikami
20. [Użytkownicy](#użytkownicy)
21. [Role i Uprawnienia](#roles-permissions)
22. [Grupy Użytkowników](#user-groups)
23. [Sesje i Bezpieczeństwo](#security-management)

### ⏰ Automatyzacja
24. [Harmonogram](#harmonogram)
25. [Cron Jobs](#cron-jobs)
26. [Triggery i Webhooks](#triggers-webhooks)
27. [Notyfikacje](#notifications)

### 📊 Monitorowanie
28. [Logi Systemowe](#logi-i-monitorowanie)
29. [Logi Aplikacji](#application-logs)
30. [Metryki Wydajności](#performance-metrics)
31. [Alerty i Ostrzeżenia](#alerts-warnings)

### 🔄 Real-time
32. [WebSocket](#websocket)
33. [Server-Sent Events](#server-sent-events)
34. [Live Monitoring](#live-monitoring)
35. [Chat i Komunikacja](#chat-communication)

### 🛠️ Narzędzia Deweloperskie
36. [SDK i Biblioteki](#sdk-i-narzędzia)
37. [CLI Tools](#cli-tools)
38. [Postman Collection](#postman-collection)
39. [OpenAPI Specification](#openapi-spec)
40. [Testowanie API](#api-testing)

### 🔧 Zaawansowane
41. [Integracje](#integrations)
42. [Plugins i Rozszerzenia](#plugins-extensions)
43. [API Hooks](#api-hooks)
44. [Custom Scripts](#custom-scripts)
45. [Migracje](#migrations)

### 📖 Przykłady i Tutoriale
46. [Przykłady Użycia](#usage-examples)
47. [Best Practices](#best-practices)
48. [Troubleshooting](#troubleshooting)
49. [FAQ](#faq)
50. [Changelog](#changelog)

---

## 🚀 Wprowadzenie

### Adres Bazowy API
```
http://45.137.70.131:5000/api
```

### 🏃‍♂️ Szybki Start

**Krok 1: Uwierzytelnianie**
```bash
curl -X POST "http://45.137.70.131:5000/api/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "password123"}'
```

**Krok 2: Pobierz listę serwerów**
```bash
curl -X GET "http://45.137.70.131:5000/api/servers" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Krok 3: Uruchom serwer**
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/start" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### 🌐 Standardy i Konwencje

#### Format Odpowiedzi
Wszystkie odpowiedzi API używają standardowego formatu JSON:
```json
{
  "success": true|false,
  "data": {},
  "error": "string",
  "message": "string",
  "meta": {},
  "timestamp": "2024-01-15T16:30:00Z"
}
```

#### Kody Statusów HTTP
- `200` - OK (sukces)
- `201` - Created (utworzono)
- `400` - Bad Request (błędne żądanie)
- `401` - Unauthorized (nieuwierzytelniony)
- `403` - Forbidden (brak uprawnień)
- `404` - Not Found (nie znaleziono)
- `422` - Unprocessable Entity (błąd walidacji)
- `429` - Too Many Requests (limit żądań)
- `500` - Internal Server Error (błąd serwera)

#### Wersjonowanie
- **Aktualna wersja**: v1
- **Format**: `/api/v1/endpoint`
- **Kodowanie**: UTF-8
- **Content-Type**: `application/json`

#### Paginacja
```json
{
  "meta": {
    "pagination": {
      "total": 150,
      "count": 25,
      "per_page": 25,
      "current_page": 1,
      "total_pages": 6,
      "links": {
        "next": "http://45.137.70.131:5000/api/servers?page=2",
        "prev": null,
        "first": "http://45.137.70.131:5000/api/servers?page=1",
        "last": "http://45.137.70.131:5000/api/servers?page=6"
      }
    }
  }
}
```

### 🔒 Uwierzytelnianie

#### Typy Tokenów
- **Access Token**: JWT (czas życia: 24h)
- **Refresh Token**: Token odświeżający (czas życia: 30 dni)
- **API Key**: Długoterminowy klucz API (brak wygaśnięcia)
- **Test Token**: Token testowy (tylko development)

#### Bezpieczeństwo
- **Algorytm podpisu**: HS256
- **Szyfrowanie haseł**: bcrypt (12 rounds)
- **Rate limiting**: Tak
- **CORS**: Skonfigurowany
- **HTTPS**: Zalecane w produkcji

---

## 🔑 Endpointy Uwierzytelniania

### POST `/auth/login`
**Opis**: Uwierzytelnij użytkownika i otrzymaj token dostępu  
**Limit**: 5 żądań/minutę per IP  
**Wymagania**: Brak (endpoint publiczny)

#### Parametry żądania
| Pole | Typ | Wymagane | Walidacja | Opis |
|------|-----|----------|-----------|------|
| username | string | ✅ | 3-50 znaków, alfanumeryczne | Nazwa użytkownika |
| password | string | ✅ | 8-128 znaków | Hasło użytkownika |
| remember_me | boolean | ❌ | true/false | Zapamiętaj sesję (wydłuż ważność tokenu) |
| device_name | string | ❌ | max 100 znaków | Nazwa urządzenia dla audytu |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "password123",
    "remember_me": true,
    "device_name": "iPhone 12"
  }'
```

#### Odpowiedzi

**✅ 200 - Sukces**
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_in": 86400,
    "token_type": "Bearer",
    "scope": ["read", "write", "admin"],
    "user": {
      "id": "usr_1234567890",
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "username": "admin",
      "email": "admin@example.com",
      "role": "admin",
      "permissions": ["*"],
      "first_name": "John",
      "last_name": "Doe",
      "avatar": "https://example.com/avatar.jpg",
      "two_factor_enabled": false,
      "email_verified": true,
      "last_login": "2024-01-15T16:30:00Z",
      "login_count": 42,
      "created_at": "2024-01-01T00:00:00Z",
      "updated_at": "2024-01-15T16:30:00Z",
      "preferences": {
        "theme": "dark",
        "language": "pl",
        "timezone": "Europe/Warsaw",
        "notifications": {
          "email": true,
          "push": false,
          "desktop": true
        },
        "dashboard": {
          "layout": "grid",
          "refresh_interval": 30
        }
      }
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**❌ 400 - Błędne dane**
```json
{
  "success": false,
  "error": "Validation failed",
  "details": [
    {
      "field": "username",
      "message": "Username is required",
      "code": "REQUIRED_FIELD"
    },
    {
      "field": "password",
      "message": "Password must be at least 8 characters",
      "code": "MIN_LENGTH"
    }
  ],
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**❌ 401 - Nieprawidłowe dane**
```json
{
  "success": false,
  "error": "Invalid credentials",
  "message": "The provided username or password is incorrect",
  "code": "INVALID_CREDENTIALS",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**❌ 423 - Konto zablokowane**
```json
{
  "success": false,
  "error": "Account locked",
  "message": "Account has been locked due to too many failed login attempts",
  "unlock_at": "2024-01-15T17:30:00Z",
  "code": "ACCOUNT_LOCKED",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**❌ 429 - Zbyt wiele prób**
```json
{
  "success": false,
  "error": "Rate limit exceeded",
  "message": "Too many login attempts. Try again in 5 minutes.",
  "retry_after": 300,
  "code": "RATE_LIMIT_EXCEEDED",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/auth/register`
**Opis**: Zarejestruj nowego użytkownika  
**Limit**: 3 żądania/godzinę per IP  
**Wymagania**: Brak (jeśli rejestracja jest włączona)

#### Parametry żądania
| Pole | Typ | Wymagane | Walidacja | Opis |
|------|-----|----------|-----------|------|
| username | string | ✅ | 3-30 znaków, alfanumeryczne, podkreślniki | Unikalna nazwa użytkownika |
| email | string | ✅ | Format email | Unikalny adres email |
| password | string | ✅ | min 8 znaków, wielkie/małe litery, cyfry | Hasło |
| password_confirmation | string | ✅ | musi być identyczne z password | Potwierdzenie hasła |
| first_name | string | ✅ | 2-50 znaków | Imię |
| last_name | string | ✅ | 2-50 znaków | Nazwisko |
| terms_accepted | boolean | ✅ | true | Akceptacja regulaminu |
| newsletter | boolean | ❌ | true/false | Zgoda na newsletter |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/auth/register" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "newuser",
    "email": "newuser@example.com",
    "password": "SecurePass123!",
    "password_confirmation": "SecurePass123!",
    "first_name": "Jan",
    "last_name": "Kowalski",
    "terms_accepted": true,
    "newsletter": false
  }'
```

#### Odpowiedzi

**✅ 201 - Użytkownik utworzony**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_9876543210",
      "username": "newuser",
      "email": "newuser@example.com",
      "role": "user",
      "first_name": "Jan",
      "last_name": "Kowalski",
      "email_verified": false,
      "created_at": "2024-01-15T16:30:00Z"
    },
    "verification_email_sent": true
  },
  "message": "Account created successfully. Please check your email to verify your account.",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/auth/verify-email`
**Opis**: Zweryfikuj adres email  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Token weryfikacyjny

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| token | string | ✅ | Token weryfikacyjny z emaila |
| email | string | ✅ | Adres email do weryfikacji |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/auth/verify-email" \
  -H "Content-Type: application/json" \
  -d '{
    "token": "verification_token_123",
    "email": "newuser@example.com"
  }'
```

### POST `/auth/forgot-password`
**Opis**: Zresetuj hasło - wyślij email z linkiem resetującym  
**Limit**: 3 żądania/godzinę per email  
**Wymagania**: Brak

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| email | string | ✅ | Adres email użytkownika |
| callback_url | string | ❌ | URL do przekierowania po resecie |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/auth/forgot-password" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "callback_url": "https://myapp.com/reset-password"
  }'
```

### POST `/auth/reset-password`
**Opis**: Zresetuj hasło używając tokenu z emaila  
**Limit**: 5 żądań/godzinę  
**Wymagania**: Token resetujący

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| token | string | ✅ | Token resetujący z emaila |
| email | string | ✅ | Adres email użytkownika |
| password | string | ✅ | Nowe hasło |
| password_confirmation | string | ✅ | Potwierdzenie nowego hasła |

### POST `/auth/two-factor/enable`
**Opis**: Włącz uwierzytelnianie dwuskładnikowe  
**Limit**: 5 żądań/godzinę  
**Wymagania**: Bearer Token

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/auth/two-factor/enable" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - QR kod wygenerowany**
```json
{
  "success": true,
  "data": {
    "qr_code": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...",
    "secret": "ABCDEFGHIJKLMNOP",
    "backup_codes": [
      "12345678",
      "87654321",
      "11111111",
      "22222222",
      "33333333"
    ]
  },
  "message": "Scan QR code with your authenticator app and confirm with verification code",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/auth/two-factor/confirm`
**Opis**: Potwierdź włączenie 2FA kodem z aplikacji  
**Limit**: 10 żądań/minutę  
**Wymagania**: Bearer Token

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| code | string | ✅ | 6-cyfrowy kod z aplikacji authenticator |

### GET `/auth/sessions`
**Opis**: Pobierz listę aktywnych sesji  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token

#### Odpowiedzi

**✅ 200 - Lista sesji**
```json
{
  "success": true,
  "data": [
    {
      "id": "session_123",
      "device_name": "iPhone 12",
      "ip_address": "192.168.1.100",
      "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 15_0 like Mac OS X)",
      "is_current": true,
      "last_activity": "2024-01-15T16:30:00Z",
      "created_at": "2024-01-15T10:00:00Z",
      "location": {
        "country": "Poland",
        "city": "Warsaw",
        "region": "Mazowieckie"
      }
    }
  ],
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### DELETE `/auth/sessions/:id`
**Opis**: Zakończ konkretną sesję  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token

---

## 🖥️ Zarządzanie Serwerami

### GET `/servers`
**Opis**: Pobierz listę serwerów z zaawansowanym filtrowaniem  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Domyślna | Opis |
|------|-----|----------|----------|------|
| page | number | ❌ | 1 | Numer strony (1-999) |
| limit | number | ❌ | 25 | Liczba serwerów na stronę (1-100) |
| search | string | ❌ | - | Wyszukaj po nazwie, ID lub opisie |
| status | string | ❌ | - | Filtruj po statusie (comma-separated) |
| type | string | ❌ | - | Filtruj po typie serwera |
| node | string | ❌ | - | Filtruj po węźle |
| owner | string | ❌ | - | Filtruj po właścicielu |
| sort_by | string | ❌ | created_at | Sortuj po polu |
| sort_order | string | ❌ | desc | Kierunek sortowania (asc/desc) |
| include | string | ❌ | - | Dodatkowe dane (stats,allocations,users) |
| created_after | string | ❌ | - | Serwery utworzone po dacie (ISO 8601) |
| created_before | string | ❌ | - | Serwery utworzone przed datą |
| tag | string | ❌ | - | Filtruj po tagach |

#### Możliwe wartości parametrów
- **status**: `installing`, `stopped`, `starting`, `running`, `stopping`, `crashed`, `suspended`
- **type**: `minecraft`, `minecraft-bedrock`, `csgo`, `rust`, `ark`, `valheim`, `terraria`, `custom`
- **sort_by**: `name`, `created_at`, `updated_at`, `status`, `memory`, `cpu`, `disk`, `players`
- **include**: `stats`, `allocations`, `users`, `databases`, `backups`, `schedules`

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers?page=1&limit=10&status=running,starting&type=minecraft&sort_by=name&sort_order=asc&include=stats,allocations" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Lista serwerów**
```json
{
  "success": true,
  "data": [
    {
      "id": "srv_1234567890abcdef",
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "external_id": "external_123",
      "name": "Minecraft Production Server",
      "description": "Main production Minecraft server for community events and gameplay",
      "type": "minecraft",
      "status": "running",
      "is_suspended": false,
      "is_installing": false,
      "is_transferring": false,
      "owner": {
        "id": "usr_123",
        "username": "server_admin",
        "email": "admin@example.com"
      },
      "node": {
        "id": "node_1",
        "name": "Germany Node 1",
        "location": "Frankfurt, Germany",
        "fqdn": "node1.hosting.com",
        "public": true,
        "maintenance_mode": false
      },
      "allocation": {
        "id": "alloc_123",
        "ip": "192.168.1.100",
        "alias": "mc.example.com",
        "port": 25565,
        "notes": "Main server port",
        "assigned": true
      },
      "additional_allocations": [
        {
          "id": "alloc_124",
          "ip": "192.168.1.100",
          "port": 25566,
          "notes": "RCON port",
          "assigned": true
        },
        {
          "id": "alloc_125",
          "ip": "192.168.1.100",
          "port": 8080,
          "notes": "Web panel port",
          "assigned": false
        }
      ],
      "resources": {
        "memory": 4096,
        "memory_overages": 0,
        "disk": 10240,
        "cpu": 200,
        "swap": 512,
        "io": 500,
        "threads": null,
        "oom_disabled": false,
        "cpu_pinning": []
      },
      "current_usage": {
        "memory": 2048,
        "memory_percentage": 50.0,
        "cpu": 45.2,
        "disk": 5120,
        "disk_percentage": 50.0,
        "network": {
          "rx_bytes": 1024000,
          "tx_bytes": 512000,
          "rx_packets": 5000,
          "tx_packets": 3000
        },
        "uptime": 86400,
        "players": {
          "online": 5,
          "max": 20,
          "list": ["Player1", "Player2", "Player3", "Player4", "Player5"]
        }
      },
      "environment": {
        "SERVER_JARFILE": "server.jar",
        "VANILLA_VERSION": "1.20.4",
        "MINECRAFT_VERSION": "1.20.4",
        "BUILD_TYPE": "recommended",
        "FORGE_VERSION": "latest",
        "MEMORY": "4096",
        "JAVA_VERSION": "17"
      },
      "egg": {
        "id": "egg_1",
        "uuid": "d3ba4a4c-7cc2-4bd3-9971-15ab80c5b47b",
        "name": "Minecraft Java",
        "author": "support@pterodactyl.io",
        "description": "Minecraft Java Edition server with Forge support",
        "docker_image": "quay.io/pterodactyl/core:java",
        "startup": "java -Xms128M -Xmx{{SERVER_MEMORY}}M -jar {{SERVER_JARFILE}}",
        "config": {
          "stop": "stop",
          "logs": {
            "custom": false,
            "location": "logs/latest.log"
          },
          "file_denylist": []
        }
      },
      "container": {
        "image": "quay.io/pterodactyl/core:java",
        "installed": true,
        "startup_command": "java -Xms128M -Xmx{{SERVER_MEMORY}}M -jar {{SERVER_JARFILE}}",
        "oom_disabled": false,
        "environment": {},
        "pid": 1234,
        "created_at": "2024-01-15T10:30:00Z"
      },
      "feature_limits": {
        "databases": 5,
        "database_used": 2,
        "allocations": 3,
        "allocations_used": 2,
        "backups": 10,
        "backups_used": 3
      },
      "tags": ["production", "minecraft", "community"],
      "created_at": "2024-01-15T10:30:00.000Z",
      "updated_at": "2024-01-15T15:45:00.000Z",
      "last_activity": "2024-01-15T16:30:00.000Z"
    }
  ],
  "meta": {
    "pagination": {
      "total": 150,
      "count": 10,
      "per_page": 10,
      "current_page": 1,
      "total_pages": 15,
      "links": {
        "next": "http://45.137.70.131:5000/api/servers?page=2",
        "prev": null,
        "first": "http://45.137.70.131:5000/api/servers?page=1",
        "last": "http://45.137.70.131:5000/api/servers?page=15"
      }
    },
    "filters_applied": {
      "status": ["running"],
      "type": "minecraft",
      "sort_by": "name",
      "sort_order": "asc"
    },
    "statistics": {
      "total_servers": 150,
      "running": 120,
      "stopped": 25,
      "installing": 3,
      "crashed": 2
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers`
**Opis**: Utwórz nowy serwer  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Walidacja | Opis |
|------|-----|----------|-----------|------|
| name | string | ✅ | 1-255 znaków | Nazwa serwera |
| description | string | ❌ | max 500 znaków | Opis serwera |
| type | string | ✅ | enum | Typ serwera (minecraft, csgo, etc.) |
| egg_id | string | ✅ | uuid | ID jajka (template) |
| node_id | string | ✅ | uuid | ID węzła |
| owner_id | string | ❌ | uuid | ID właściciela (default: current user) |
| allocation | object | ✅ | - | Alokacja IP i portu |
| resources | object | ✅ | - | Limity zasobów |
| environment | object | ❌ | - | Zmienne środowiskowe |
| feature_limits | object | ❌ | - | Limity funkcji |
| tags | array | ❌ | max 10 elementów | Tagi serwera |
| auto_start | boolean | ❌ | true/false | Automatyczne uruchomienie po utworzeniu |

#### Struktura allocation
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| ip | string | ✅ | Adres IP |
| port | number | ✅ | Port główny |
| alias | string | ❌ | Alias domeny |

#### Struktura resources
| Pole | Typ | Wymagane | Min | Max | Opis |
|------|-----|----------|-----|-----|------|
| memory | number | ✅ | 128 | 32768 | Pamięć RAM w MB |
| disk | number | ✅ | 512 | 102400 | Przestrzeń dyskowa w MB |
| cpu | number | ✅ | 10 | 1000 | Limit CPU w % |
| swap | number | ❌ | 0 | 8192 | Swap w MB |
| io | number | ❌ | 10 | 1000 | IO weight |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Minecraft Server",
    "description": "A new Minecraft server for testing",
    "type": "minecraft",
    "egg_id": "d3ba4a4c-7cc2-4bd3-9971-15ab80c5b47b",
    "node_id": "node_1",
    "allocation": {
      "ip": "192.168.1.100",
      "port": 25567,
      "alias": "test.example.com"
    },
    "resources": {
      "memory": 2048,
      "disk": 5120,
      "cpu": 100,
      "swap": 256,
      "io": 500
    },
    "environment": {
      "SERVER_JARFILE": "server.jar",
      "VANILLA_VERSION": "1.20.4",
      "MEMORY": "2048"
    },
    "feature_limits": {
      "databases": 3,
      "allocations": 2,
      "backups": 5
    },
    "tags": ["test", "minecraft"],
    "auto_start": true
  }'
```

#### Odpowiedzi

**✅ 201 - Serwer utworzony**
```json
{
  "success": true,
  "data": {
    "id": "srv_9876543210fedcba",
    "uuid": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
    "name": "My Minecraft Server",
    "description": "A new Minecraft server for testing",
    "type": "minecraft",
    "status": "installing",
    "installation_progress": 0,
    "estimated_install_time": 300,
    "node": {
      "id": "node_1",
      "name": "Germany Node 1"
    },
    "allocation": {
      "ip": "192.168.1.100",
      "port": 25567,
      "alias": "test.example.com"
    },
    "resources": {
      "memory": 2048,
      "disk": 5120,
      "cpu": 100,
      "swap": 256,
      "io": 500
    },
    "created_at": "2024-01-15T16:00:00Z"
  },
  "message": "Server creation started. Installation in progress.",
  "timestamp": "2024-01-15T16:00:00Z"
}
```

**❌ 422 - Błąd walidacji**
```json
{
  "success": false,
  "error": "Validation failed",
  "details": [
    {
      "field": "allocation.port",
      "message": "Port 25565 is already in use",
      "code": "PORT_IN_USE"
    },
    {
      "field": "resources.memory",
      "message": "Not enough memory available on selected node",
      "code": "INSUFFICIENT_RESOURCES"
    }
  ],
  "timestamp": "2024-01-15T16:00:00Z"
}
```

### GET `/servers/:id`
**Opis**: Pobierz szczegóły konkretnego serwera  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + access to server

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| include | string | ❌ | Dodatkowe dane (stats,users,databases,backups,schedules,allocations) |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123?include=stats,users,databases" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Szczegóły serwera**
```json
{
  "success": true,
  "data": {
    // Pełne dane serwera jak w GET /servers, plus:
    "installation_log": [
      {
        "timestamp": "2024-01-15T16:00:00Z",
        "message": "Starting server installation...",
        "level": "info"
      },
      {
        "timestamp": "2024-01-15T16:01:00Z",
        "message": "Downloaded server.jar successfully",
        "level": "info"
      }
    ],
    "performance_history": {
      "cpu": [
        {"timestamp": "2024-01-15T16:25:00Z", "value": 42.1},
        {"timestamp": "2024-01-15T16:26:00Z", "value": 45.2},
        {"timestamp": "2024-01-15T16:27:00Z", "value": 43.8}
      ],
      "memory": [
        {"timestamp": "2024-01-15T16:25:00Z", "value": 2000},
        {"timestamp": "2024-01-15T16:26:00Z", "value": 2048},
        {"timestamp": "2024-01-15T16:27:00Z", "value": 2100}
      ]
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### PUT `/servers/:id`
**Opis**: Aktualizuj serwer  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.update`

#### Parametry żądania (wszystkie opcjonalne)
| Pole | Typ | Opis |
|------|-----|------|
| name | string | Nowa nazwa serwera |
| description | string | Nowy opis |
| tags | array | Nowe tagi |

#### Przykładowe żądanie
```bash
curl -X PUT "http://45.137.70.131:5000/api/servers/srv_123" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Server Name",
    "description": "Updated description",
    "tags": ["production", "minecraft", "updated"]
  }'
```

### DELETE `/servers/:id`
**Opis**: Usuń serwer  
**Limit**: 5 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.delete`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| force | boolean | ❌ | Wymuś usunięcie (pomijaj sprawdzenia) |
| backup_before_delete | boolean | ❌ | Utwórz backup przed usunięciem |

#### Przykładowe żądanie
```bash
curl -X DELETE "http://45.137.70.131:5000/api/servers/srv_123?backup_before_delete=true" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Serwer usunięty**
```json
{
  "success": true,
  "message": "Server deleted successfully",
  "data": {
    "backup_created": true,
    "backup_id": "backup_123",
    "deleted_at": "2024-01-15T16:30:00Z"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

---

## ⚡ Power Management

### POST `/servers/:id/start`
**Opis**: Uruchom serwer  
**Limit**: 30 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.power.start`

#### Parametry żądania (opcjonalne)
| Pole | Typ | Opis |
|------|-----|------|
| wait_for_startup | boolean | Czy czekać na pełne uruchomienie |
| startup_timeout | number | Timeout w sekundach (max 600) |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/start" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "wait_for_startup": true,
    "startup_timeout": 300
  }'
```

#### Odpowiedzi

**✅ 200 - Polecenie wysłane**
```json
{
  "success": true,
  "message": "Server start command sent",
  "data": {
    "previous_status": "stopped",
    "new_status": "starting",
    "estimated_startup_time": 60,
    "command_id": "cmd_123456"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**❌ 409 - Serwer już uruchomiony**
```json
{
  "success": false,
  "error": "Conflict",
  "message": "Server is already running",
  "data": {
    "current_status": "running",
    "uptime": 3600
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/stop`
**Opis**: Zatrzymaj serwer  
**Limit**: 30 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.power.stop`

#### Parametry żądania (opcjonalne)
| Pole | Typ | Opis |
|------|-----|------|
| signal | string | Typ sygnału (SIGTERM, SIGINT, SIGKILL) |
| delay | number | Opóźnienie w sekundach przed zatrzymaniem |
| reason | string | Powód zatrzymania (do logów) |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/stop" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "signal": "SIGTERM",
    "delay": 30,
    "reason": "Scheduled maintenance"
  }'
```

### POST `/servers/:id/restart`
**Opis**: Restartuj serwer  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.power.restart`

#### Parametry żądania (opcjonalne)
| Pole | Typ | Opis |
|------|-----|------|
| delay | number | Opóźnienie przed restartem w sekundach |
| announcement | string | Wiadomość do wysłania przed restartem |
| announcement_delay | number | Kiedy wysłać wiadomość (sekundy przed restartem) |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/restart" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "delay": 300,
    "announcement": "Server restarting in 5 minutes for updates!",
    "announcement_delay": 300
  }'
```

### POST `/servers/:id/kill`
**Opis**: Wymuś zatrzymanie serwera (SIGKILL)  
**Limit**: 10 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.power.kill`

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/kill" \
  -H "Authorization: Bearer your_token_here"
```

### GET `/servers/:id/power-status`
**Opis**: Pobierz aktualny status zasilania serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + access to server

#### Odpowiedzi

**✅ 200 - Status zasilania**
```json
{
  "success": true,
  "data": {
    "status": "running",
    "uptime": 3600,
    "last_status_change": "2024-01-15T15:30:00Z",
    "process_id": 1234,
    "cpu_usage": 45.2,
    "memory_usage": 2048,
    "is_responding": true,
    "startup_progress": 100,
    "power_actions_available": {
      "start": false,
      "stop": true,
      "restart": true,
      "kill": true
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

---

## 🎮 Server Console

### GET `/servers/:id/console`
**Opis**: Pobierz historię konsoli  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.console.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| lines | number | ❌ | Liczba ostatnich linii (1-2000, default: 100) |
| since | string | ❌ | Pobierz logi od daty (ISO 8601) |
| level | string | ❌ | Filtruj po poziomie (info,warn,error,debug) |
| search | string | ❌ | Wyszukaj w treści logów |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/console?lines=50&level=error" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Historia konsoli**
```json
{
  "success": true,
  "data": {
    "lines": [
      {
        "timestamp": "2024-01-15T16:30:00Z",
        "level": "info",
        "message": "[16:30:00] [Server thread/INFO]: Starting minecraft server version 1.20.4",
        "raw": "[16:30:00] [Server thread/INFO]: Starting minecraft server version 1.20.4"
      },
      {
        "timestamp": "2024-01-15T16:30:01Z",
        "level": "info",
        "message": "[16:30:01] [Server thread/INFO]: Loading properties",
        "raw": "[16:30:01] [Server thread/INFO]: Loading properties"
      },
      {
        "timestamp": "2024-01-15T16:30:10Z",
        "level": "info",
        "message": "[16:30:10] [Server thread/INFO]: Done (5.234s)! For help, type \"help\"",
        "raw": "[16:30:10] [Server thread/INFO]: Done (5.234s)! For help, type \"help\""
      }
    ],
    "total_lines": 150,
    "filtered_lines": 3,
    "server_status": "running"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/console`
**Opis**: Wyślij komendę do konsoli serwera  
**Limit**: 60 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.console.send`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| command | string | ✅ | Komenda do wykonania |
| wait_for_response | boolean | ❌ | Czy czekać na odpowiedź (default: false) |
| timeout | number | ❌ | Timeout dla odpowiedzi w sekundach |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/console" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "command": "say Hello World!",
    "wait_for_response": true,
    "timeout": 10
  }'
```

#### Odpowiedzi

**✅ 200 - Komenda wysłana**
```json
{
  "success": true,
  "message": "Command sent to server",
  "data": {
    "command": "say Hello World!",
    "sent_at": "2024-01-15T16:30:00Z",
    "command_id": "cmd_789",
    "response": "[16:30:00] [Server thread/INFO]: [Server] Hello World!"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

---

## 📁 Zarządzanie Plikami

### GET `/servers/:id/files`
**Opis**: Listuj pliki w katalogu serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| path | string | ❌ | Ścieżka katalogu (default: "/") |
| recursive | boolean | ❌ | Rekurencyjne listowanie |
| show_hidden | boolean | ❌ | Pokaż ukryte pliki |
| sort_by | string | ❌ | Sortuj po (name,size,modified,type) |
| sort_order | string | ❌ | Kierunek sortowania (asc,desc) |
| filter_type | string | ❌ | Filtruj po typie (file,directory) |
| extension | string | ❌ | Filtruj po rozszerzeniu |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/files?path=/plugins&sort_by=modified&sort_order=desc" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Lista plików**
```json
{
  "success": true,
  "data": {
    "path": "/plugins",
    "absolute_path": "/home/container/plugins",
    "files": [
      {
        "name": "EssentialsX.jar",
        "type": "file",
        "size": 1048576,
        "size_human": "1.0 MB",
        "permissions": "644",
        "owner": "container",
        "group": "container",
        "modified": "2024-01-15T10:30:00Z",
        "created": "2024-01-10T14:20:00Z",
        "extension": "jar",
        "mime_type": "application/java-archive",
        "is_executable": false,
        "is_symlink": false,
        "checksum": {
          "md5": "5d41402abc4b2a76b9719d911017c592",
          "sha1": "aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d"
        }
      },
      {
        "name": "WorldEdit",
        "type": "directory",
        "size": 0,
        "permissions": "755",
        "owner": "container",
        "group": "container",
        "modified": "2024-01-15T12:15:00Z",
        "created": "2024-01-10T14:20:00Z",
        "items_count": 15,
        "total_size": 2097152
      }
    ],
    "statistics": {
      "total_files": 25,
      "total_directories": 8,
      "total_size": 52428800,
      "disk_usage_percentage": 5.1
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/files/content`
**Opis**: Pobierz zawartość pliku  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| path | string | ✅ | Ścieżka do pliku |
| encoding | string | ❌ | Kodowanie (utf-8,base64) |
| start_line | number | ❌ | Numer pierwszej linii do pobrania |
| end_line | number | ❌ | Numer ostatniej linii do pobrania |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/files/content?path=/server.properties" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Zawartość pliku**
```json
{
  "success": true,
  "data": {
    "content": "server-port=25565\nmax-players=20\nmotd=Welcome to my server!\ndifficulty=normal\ngamemode=survival",
    "encoding": "utf-8",
    "size": 95,
    "lines": 5,
    "file_info": {
      "name": "server.properties",
      "modified": "2024-01-15T10:30:00Z",
      "permissions": "644",
      "checksum": {
        "md5": "5d41402abc4b2a76b9719d911017c592"
      }
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### PUT `/servers/:id/files/content`
**Opis**: Aktualizuj zawartość pliku  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.write`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| path | string | ✅ | Ścieżka do pliku |
| content | string | ✅ | Nowa zawartość pliku |
| encoding | string | ❌ | Kodowanie (utf-8,base64) |
| backup | boolean | ❌ | Utwórz backup przed edycją |
| verify_checksum | string | ❌ | Sprawdź checksum przed zapisem |

#### Przykładowe żądanie
```bash
curl -X PUT "http://45.137.70.131:5000/api/servers/srv_123/files/content" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "path": "/server.properties",
    "content": "server-port=25565\nmax-players=30\nmotd=Updated server message!\ndifficulty=hard\ngamemode=survival",
    "backup": true
  }'
```

#### Odpowiedzi

**✅ 200 - Plik zaktualizowany**
```json
{
  "success": true,
  "message": "File updated successfully",
  "data": {
    "path": "/server.properties",
    "size_before": 95,
    "size_after": 103,
    "backup_created": true,
    "backup_path": "/backups/server.properties.2024-01-15_16-30-00.bak",
    "modified_at": "2024-01-15T16:30:00Z",
    "checksum": {
      "md5": "new_checksum_here"
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/files/upload`
**Opis**: Upload plików na serwer  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.create`

#### Nagłówki
- `Authorization: Bearer <token>`
- `Content-Type: multipart/form-data`

#### Parametry form-data
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| files | file[] | ✅ | Pliki do uploadu (max 10 plików) |
| path | string | ❌ | Katalog docelowy (default: "/") |
| overwrite | boolean | ❌ | Zastąp istniejące pliki |
| extract | boolean | ❌ | Wyodrębnij archiwa (.zip, .tar.gz) |
| chmod | string | ❌ | Ustaw uprawnienia (np. "644") |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/files/upload" \
  -H "Authorization: Bearer your_token_here" \
  -F "files=@plugin.jar" \
  -F "files=@config.yml" \
  -F "path=/plugins" \
  -F "overwrite=true" \
  -F "chmod=644"
```

#### Odpowiedzi

**✅ 200 - Pliki przesłane**
```json
{
  "success": true,
  "message": "Files uploaded successfully",
  "data": {
    "uploaded_files": [
      {
        "filename": "plugin.jar",
        "original_name": "plugin.jar",
        "size": 1048576,
        "path": "/plugins/plugin.jar",
        "checksum": {
          "md5": "5d41402abc4b2a76b9719d911017c592"
        },
        "uploaded_at": "2024-01-15T16:30:00Z"
      },
      {
        "filename": "config.yml",
        "original_name": "config.yml",
        "size": 2048,
        "path": "/plugins/config.yml",
        "checksum": {
          "md5": "another_checksum_here"
        },
        "uploaded_at": "2024-01-15T16:30:00Z"
      }
    ],
    "total_size": 1050624,
    "total_files": 2
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/files/download`
**Opis**: Pobierz plik z serwera  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| path | string | ✅ | Ścieżka do pliku |
| compression | string | ❌ | Kompresja (none,gzip,zip) |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/files/download?path=/world&compression=zip" \
  -H "Authorization: Bearer your_token_here" \
  -o world.zip
```

### POST `/servers/:id/files/create`
**Opis**: Utwórz nowy plik lub katalog  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| path | string | ✅ | Ścieżka do utworzenia |
| type | string | ✅ | Typ (file, directory) |
| content | string | ❌ | Zawartość (tylko dla plików) |
| permissions | string | ❌ | Uprawnienia (np. "755") |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/files/create" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "path": "/configs/new-config.yml",
    "type": "file",
    "content": "# New configuration file\nversion: 1.0\nsetting: value",
    "permissions": "644"
  }'
```

### DELETE `/servers/:id/files`
**Opis**: Usuń pliki lub katalogi  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.delete`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| paths | array | ✅ | Lista ścieżek do usunięcia |
| force | boolean | ❌ | Wymuś usunięcie (katalogi niepuste) |
| backup | boolean | ❌ | Utwórz backup przed usunięciem |

#### Przykładowe żądanie
```bash
curl -X DELETE "http://45.137.70.131:5000/api/servers/srv_123/files" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "paths": ["/old-backup.zip", "/temp-folder"],
    "force": true,
    "backup": false
  }'
```

### POST `/servers/:id/files/operations`
**Opis**: Operacje na plikach (copy, move, rename)  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.write`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| operation | string | ✅ | Typ operacji (copy, move, rename) |
| from | string | ✅ | Ścieżka źródłowa |
| to | string | ✅ | Ścieżka docelowa |
| overwrite | boolean | ❌ | Zastąp istniejący plik |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/files/operations" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "operation": "copy",
    "from": "/world",
    "to": "/world-backup",
    "overwrite": false
  }'
```

---

## File Operations

### POST `/servers/:id/files/compress`
**Opis**: Kompresuj pliki/katalogi  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.files.write`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| paths | array | ✅ | Lista ścieżek do kompresji |
| archive_name | string | ✅ | Nazwa archiwum |
| format | string | ❌ | Format (zip, tar.gz, tar.bz2) |
| compression_level | number | ❌ | Poziom kompresji (1-9) |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/files/compress" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "paths": ["/world", "/plugins"],
    "archive_name": "server-backup.zip",
    "format": "zip",
    "compression_level": 6
  }'
```

### POST `/servers/:id/files/extract`
**Opis**: Wyodrębnij archiwum  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.files.write`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| archive_path | string | ✅ | Ścieżka do archiwum |
| extract_to | string | ❌ | Katalog docelowy (default: same directory) |
| overwrite | boolean | ❌ | Zastąp istniejące pliki |

---

## File Permissions

### GET `/servers/:id/files/permissions`
**Opis**: Pobierz uprawnienia pliku/katalogu  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| path | string | ✅ | Ścieżka do pliku/katalogu |

#### Odpowiedzi

**✅ 200 - Uprawnienia pliku**
```json
{
  "success": true,
  "data": {
    "path": "/server.properties",
    "permissions": {
      "octal": "644",
      "symbolic": "-rw-r--r--",
      "owner": {
        "read": true,
        "write": true,
        "execute": false
      },
      "group": {
        "read": true,
        "write": false,
        "execute": false
      },
      "others": {
        "read": true,
        "write": false,
        "execute": false
      }
    },
    "ownership": {
      "owner": "container",
      "group": "container",
      "uid": 1000,
      "gid": 1000
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### PUT `/servers/:id/files/permissions`
**Opis**: Zmień uprawnienia pliku/katalogu  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.files.write`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| path | string | ✅ | Ścieżka do pliku/katalogu |
| permissions | string | ✅ | Uprawnienia (octal format, np. "755") |
| recursive | boolean | ❌ | Rekurencyjnie (tylko dla katalogów) |

#### Przykładowe żądanie
```bash
curl -X PUT "http://45.137.70.131:5000/api/servers/srv_123/files/permissions" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "path": "/scripts",
    "permissions": "755",
    "recursive": true
  }'
```

---

## 🗄️ Bazy Danych

### GET `/servers/:id/databases`
**Opis**: Pobierz listę baz danych serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| type | string | ❌ | Filtruj po typie (mysql, postgresql, mongodb, redis) |
| include_stats | boolean | ❌ | Dołącz statystyki bazy |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/databases?include_stats=true" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Lista baz danych**
```json
{
  "success": true,
  "data": [
    {
      "id": "db_1234567890",
      "name": "minecraft_data",
      "type": "mysql",
      "username": "mc_user_123",
      "host": "127.0.0.1",
      "port": 3306,
      "database": "s123_minecraft_data",
      "max_connections": 10,
      "current_connections": 2,
      "charset": "utf8mb4",
      "collation": "utf8mb4_unicode_ci",
      "status": "active",
      "size": {
        "data": 52428800,
        "index": 10485760,
        "total": 62914560,
        "human": "60.0 MB"
      },
      "tables_count": 15,
      "created_at": "2024-01-15T10:30:00Z",
      "last_backup": "2024-01-15T06:00:00Z",
      "statistics": {
        "queries_per_second": 12.5,
        "slow_queries": 3,
        "uptime": 86400
      }
    }
  ],
  "meta": {
    "total_databases": 3,
    "used_slots": 3,
    "available_slots": 2,
    "total_size": 167772160
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/databases`
**Opis**: Utwórz nową bazę danych  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.databases.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa bazy (a-z, 0-9, _, max 32 znaki) |
| type | string | ✅ | Typ bazy (mysql, postgresql) |
| username | string | ❌ | Nazwa użytkownika (auto-generated if empty) |
| password | string | ❌ | Hasło (auto-generated if empty) |
| charset | string | ❌ | Kodowanie (default: utf8mb4) |
| collation | string | ❌ | Kolacja (default: utf8mb4_unicode_ci) |
| max_connections | number | ❌ | Max połączeń (default: 10) |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/databases" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "plugin_data",
    "type": "mysql",
    "username": "plugin_user",
    "password": "SecurePass123!",
    "charset": "utf8mb4",
    "max_connections": 5
  }'
```

#### Odpowiedzi

**✅ 201 - Baza utworzona**
```json
{
  "success": true,
  "data": {
    "id": "db_9876543210",
    "name": "plugin_data",
    "type": "mysql",
    "username": "plugin_user",
    "password": "SecurePass123!",
    "host": "127.0.0.1",
    "port": 3306,
    "database": "s123_plugin_data",
    "max_connections": 5,
    "connection_string": "mysql://plugin_user:SecurePass123!@127.0.0.1:3306/s123_plugin_data",
    "created_at": "2024-01-15T16:30:00Z"
  },
  "message": "Database created successfully",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/databases/:dbId`
**Opis**: Pobierz szczegóły bazy danych  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| include_tables | boolean | ❌ | Dołącz listę tabel |
| include_stats | boolean | ❌ | Dołącz statystyki |

#### Odpowiedzi

**✅ 200 - Szczegóły bazy**
```json
{
  "success": true,
  "data": {
    "id": "db_1234567890",
    "name": "minecraft_data",
    "type": "mysql",
    "username": "mc_user_123",
    "host": "127.0.0.1",
    "port": 3306,
    "database": "s123_minecraft_data",
    "status": "active",
    "size": {
      "data": 52428800,
      "index": 10485760,
      "total": 62914560
    },
    "tables": [
      {
        "name": "users",
        "rows": 1250,
        "size": 25165824,
        "engine": "InnoDB",
        "created_at": "2024-01-10T10:00:00Z"
      },
      {
        "name": "player_stats",
        "rows": 15000,
        "size": 20971520,
        "engine": "InnoDB",
        "created_at": "2024-01-10T10:00:00Z"
      }
    ],
    "statistics": {
      "total_queries": 50000,
      "queries_per_second": 12.5,
      "slow_queries": 3,
      "connections": {
        "current": 2,
        "max": 10,
        "total": 1500
      },
      "cache_hit_ratio": 98.5,
      "uptime": 86400
    },
    "created_at": "2024-01-15T10:30:00Z"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### PUT `/servers/:id/databases/:dbId`
**Opis**: Aktualizuj bazę danych  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.update`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| password | string | ❌ | Nowe hasło |
| max_connections | number | ❌ | Nowy limit połączeń |

### DELETE `/servers/:id/databases/:dbId`
**Opis**: Usuń bazę danych  
**Limit**: 5 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.databases.delete`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| backup_before_delete | boolean | ❌ | Utwórz backup przed usunięciem |

#### Przykładowe żądanie
```bash
curl -X DELETE "http://45.137.70.131:5000/api/servers/srv_123/databases/db_123?backup_before_delete=true" \
  -H "Authorization: Bearer your_token_here"
```

---

## MySQL Operations

### POST `/servers/:id/databases/:dbId/execute`
**Opis**: Wykonaj zapytanie SQL  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.execute`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| query | string | ✅ | Zapytanie SQL |
| parameters | array | ❌ | Parametry dla prepared statement |
| timeout | number | ❌ | Timeout w sekundach (max 60) |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/databases/db_123/execute" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "SELECT username, level FROM users WHERE last_login > ? LIMIT 10",
    "parameters": ["2024-01-01 00:00:00"],
    "timeout": 30
  }'
```

#### Odpowiedzi

**✅ 200 - Zapytanie wykonane**
```json
{
  "success": true,
  "data": {
    "query": "SELECT username, level FROM users WHERE last_login > ? LIMIT 10",
    "results": [
      {
        "username": "player1",
        "level": 25
      },
      {
        "username": "player2",
        "level": 18
      }
    ],
    "rows_affected": 0,
    "rows_returned": 2,
    "execution_time": 0.045,
    "query_id": "query_123456"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/databases/:dbId/backup`
**Opis**: Utwórz backup bazy danych  
**Limit**: 5 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.databases.backup`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ❌ | Nazwa backupu (auto-generated if empty) |
| tables | array | ❌ | Lista tabel (wszystkie jeśli puste) |
| structure_only | boolean | ❌ | Tylko struktura (bez danych) |
| compress | boolean | ❌ | Kompresuj backup |

### POST `/servers/:id/databases/:dbId/restore`
**Opis**: Przywróć bazę z backupu  
**Limit**: 3 żądania/godzinę  
**Wymagania**: Bearer Token + permission `servers.databases.restore`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| backup_id | string | ✅ | ID backupu do przywrócenia |
| drop_existing | boolean | ❌ | Usuń istniejące tabele |

---

## PostgreSQL Operations

### GET `/servers/:id/databases/:dbId/schemas`
**Opis**: Pobierz listę schematów PostgreSQL  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.read`

### POST `/servers/:id/databases/:dbId/schemas`
**Opis**: Utwórz nowy schemat  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.create`

---

## MongoDB Operations

### GET `/servers/:id/databases/:dbId/collections`
**Opis**: Pobierz listę kolekcji MongoDB  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.read`

### POST `/servers/:id/databases/:dbId/collections`
**Opis**: Utwórz nową kolekcję  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.create`

### POST `/servers/:id/databases/:dbId/query`
**Opis**: Wykonaj zapytanie MongoDB  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.execute`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| collection | string | ✅ | Nazwa kolekcji |
| operation | string | ✅ | Operacja (find, insert, update, delete) |
| query | object | ❌ | Zapytanie MongoDB |
| options | object | ❌ | Opcje zapytania |

---

## Redis Operations

### GET `/servers/:id/databases/:dbId/keys`
**Opis**: Pobierz listę kluczy Redis  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| pattern | string | ❌ | Wzorzec kluczy (default: "*") |
| limit | number | ❌ | Limit kluczy (max 1000) |

### POST `/servers/:id/databases/:dbId/keys`
**Opis**: Wykonaj operację Redis  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.databases.execute`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| command | string | ✅ | Komenda Redis |
| args | array | ❌ | Argumenty komendy |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/databases/db_123/keys" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "command": "SET",
    "args": ["player:1234:level", "25"]
  }'
```

---

## 👥 Zarządzanie Użytkownikami

### GET `/servers/:id/users`
**Opis**: Pobierz listę użytkowników serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.users.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| role | string | ❌ | Filtruj po roli (owner, admin, moderator, user) |
| permission | string | ❌ | Filtruj po uprawnieniu |
| search | string | ❌ | Wyszukaj po nazwie/emailu |
| sort_by | string | ❌ | Sortuj po (username, role, last_activity) |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/users?role=admin&sort_by=last_activity" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Lista użytkowników**
```json
{
  "success": true,
  "data": [
    {
      "id": "usr_1234567890",
      "username": "player1",
      "email": "player1@example.com",
      "role": "admin",
      "permissions": [
        "servers.console.read",
        "servers.console.send",
        "servers.files.read",
        "servers.files.write",
        "servers.power.start",
        "servers.power.stop"
      ],
      "granted_by": {
        "id": "usr_owner",
        "username": "server_owner"
      },
      "granted_at": "2024-01-10T14:30:00Z",
      "last_activity": "2024-01-15T15:45:00Z",
      "activity_stats": {
        "console_commands": 45,
        "files_modified": 12,
        "logins": 25
      },
      "restrictions": {
        "allowed_ips": ["192.168.1.100", "10.0.0.5"],
        "time_restrictions": {
          "allowed_hours": "08:00-22:00",
          "timezone": "Europe/Warsaw"
        }
      }
    }
  ],
  "meta": {
    "total_users": 5,
    "roles_count": {
      "owner": 1,
      "admin": 2,
      "moderator": 1,
      "user": 1
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/users`
**Opis**: Dodaj użytkownika do serwera  
**Limit**: 20 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.users.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| email | string | ✅ | Adres email użytkownika |
| role | string | ❌ | Rola (admin, moderator, user) |
| permissions | array | ❌ | Lista uprawnień |
| restrictions | object | ❌ | Ograniczenia dostępu |
| send_notification | boolean | ❌ | Wyślij powiadomienie email |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/users" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "newuser@example.com",
    "role": "moderator",
    "permissions": [
      "servers.console.read",
      "servers.files.read",
      "servers.power.restart"
    ],
    "restrictions": {
      "allowed_ips": ["192.168.1.0/24"],
      "time_restrictions": {
        "allowed_hours": "09:00-18:00",
        "timezone": "Europe/Warsaw"
      }
    },
    "send_notification": true
  }'
```

### PUT `/servers/:id/users/:userId`
**Opis**: Aktualizuj uprawnienia użytkownika  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.users.update`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| role | string | ❌ | Nowa rola |
| permissions | array | ❌ | Nowa lista uprawnień |
| restrictions | object | ❌ | Nowe ograniczenia |

### DELETE `/servers/:id/users/:userId`
**Opis**: Usuń użytkownika z serwera  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.users.delete`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| reason | string | ❌ | Powód usunięcia |
| notify_user | boolean | ❌ | Powiadom użytkownika |

---

## Roles & Permissions

### GET `/servers/:id/roles`
**Opis**: Pobierz dostępne role i uprawnienia  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.users.read`

#### Odpowiedzi

**✅ 200 - Role i uprawnienia**
```json
{
  "success": true,
  "data": {
    "roles": [
      {
        "name": "owner",
        "display_name": "Właściciel",
        "description": "Pełne uprawnienia do serwera",
        "permissions": ["*"],
        "is_system": true,
        "color": "#ff0000"
      },
      {
        "name": "admin",
        "display_name": "Administrator",
        "description": "Zarządzanie serwerem i użytkownikami",
        "permissions": [
          "servers.console.*",
          "servers.files.*",
          "servers.power.*",
          "servers.users.read",
          "servers.databases.read"
        ],
        "is_system": true,
        "color": "#ff6600"
      }
    ],
    "permissions": [
      {
        "name": "servers.console.read",
        "display_name": "Odczyt konsoli",
        "description": "Możliwość przeglądania logów konsoli",
        "category": "console"
      },
      {
        "name": "servers.console.send",
        "display_name": "Wysyłanie komend",
        "description": "Możliwość wysyłania komend do konsoli",
        "category": "console"
      }
    ]
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/roles`
**Opis**: Utwórz niestandardową rolę  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.roles.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa roli (unique) |
| display_name | string | ✅ | Wyświetlana nazwa |
| description | string | ❌ | Opis roli |
| permissions | array | ✅ | Lista uprawnień |
| color | string | ❌ | Kolor roli (hex) |

---

## User Groups

### GET `/servers/:id/groups`
**Opis**: Pobierz grupy użytkowników  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.users.read`

### POST `/servers/:id/groups`
**Opis**: Utwórz grupę użytkowników  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.groups.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa grupy |
| description | string | ❌ | Opis grupy |
| permissions | array | ✅ | Uprawnienia grupy |
| members | array | ❌ | Lista ID użytkowników |

---

## Security Management

### GET `/servers/:id/security/sessions`
**Opis**: Pobierz aktywne sesje użytkowników serwera  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.security.read`

### POST `/servers/:id/security/audit-log`
**Opis**: Pobierz logi audytu  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.security.read`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| action | string | ❌ | Filtruj po akcji |
| user_id | string | ❌ | Filtruj po użytkowniku |
| date_from | string | ❌ | Data początkowa |
| date_to | string | ❌ | Data końcowa |

---

## ⚙️ Backup Operations

### GET `/servers/:id/backups`
**Opis**: Pobierz listę backupów serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.backups.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| type | string | ❌ | Typ backupu (manual, scheduled, automatic) |
| status | string | ❌ | Status (pending, processing, completed, failed) |
| limit | number | ❌ | Liczba backupów (max 100) |
| sort_by | string | ❌ | Sortuj po (created_at, size, name) |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/backups?type=manual&status=completed&limit=20" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Lista backupów**
```json
{
  "success": true,
  "data": [
    {
      "id": "backup_1234567890",
      "name": "Manual Backup 2024-01-15",
      "description": "Pre-update backup before plugin installation",
      "type": "manual",
      "status": "completed",
      "size": 1073741824,
      "size_human": "1.0 GB",
      "compression_ratio": 0.65,
      "files_count": 15420,
      "checksum": {
        "md5": "5d41402abc4b2a76b9719d911017c592",
        "sha256": "2cf24dba4f21d4288094e9b8b6c4b8e86fa0e2e8a7636e4b2e04bfa5d9e4b5f5"
      },
      "progress": 100,
      "created_by": {
        "id": "usr_123",
        "username": "admin"
      },
      "created_at": "2024-01-15T10:30:00Z",
      "completed_at": "2024-01-15T10:35:00Z",
      "expires_at": "2024-02-15T10:35:00Z",
      "download_url": "https://example.com/download/backup_123",
      "metadata": {
        "server_version": "1.20.4",
        "plugins": ["EssentialsX", "WorldEdit", "LuckPerms"],
        "world_name": "world",
        "player_count": 25
      },
      "retention_policy": {
        "auto_delete": true,
        "keep_days": 30
      }
    }
  ],
  "meta": {
    "total_backups": 15,
    "total_size": 10737418240,
    "available_space": 53687091200,
    "quota": {
      "used": 15,
      "limit": 20,
      "size_used": 10737418240,
      "size_limit": 21474836480
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/backups`
**Opis**: Utwórz nowy backup  
**Limit**: 5 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.backups.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ❌ | Nazwa backupu (auto-generated if empty) |
| description | string | ❌ | Opis backupu |
| type | string | ❌ | Typ (manual, scheduled) |
| include_files | array | ❌ | Pliki/katalogi do uwzględnienia |
| exclude_files | array | ❌ | Pliki/katalogi do pominięcia |
| include_databases | boolean | ❌ | Uwzględnij bazy danych |
| compression | string | ❌ | Kompresja (none, gzip, bzip2, lzma) |
| encryption | boolean | ❌ | Szyfrowanie backupu |
| stop_server | boolean | ❌ | Zatrzymaj serwer podczas backupu |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/backups" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Pre-update backup",
    "description": "Backup before major plugin update",
    "type": "manual",
    "include_files": ["/world", "/plugins", "/server.properties"],
    "exclude_files": ["/logs", "/cache"],
    "include_databases": true,
    "compression": "gzip",
    "encryption": true,
    "stop_server": false
  }'
```

#### Odpowiedzi

**✅ 202 - Backup rozpoczęty**
```json
{
  "success": true,
  "data": {
    "id": "backup_9876543210",
    "name": "Pre-update backup",
    "type": "manual",
    "status": "pending",
    "estimated_time": 300,
    "estimated_size": 1073741824,
    "created_at": "2024-01-15T16:30:00Z",
    "progress_url": "/api/servers/srv_123/backups/backup_9876543210/progress"
  },
  "message": "Backup creation started",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/backups/:backupId`
**Opis**: Pobierz szczegóły backupu  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.backups.read`

### GET `/servers/:id/backups/:backupId/progress`
**Opis**: Pobierz postęp tworzenia backupu  
**Limit**: 300 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.backups.read`

#### Odpowiedzi

**✅ 200 - Postęp backupu**
```json
{
  "success": true,
  "data": {
    "id": "backup_9876543210",
    "status": "processing",
    "progress": 65,
    "current_stage": "compressing_files",
    "stages": [
      {
        "name": "preparing",
        "status": "completed",
        "started_at": "2024-01-15T16:30:00Z",
        "completed_at": "2024-01-15T16:30:15Z"
      },
      {
        "name": "copying_files",
        "status": "completed",
        "started_at": "2024-01-15T16:30:15Z",
        "completed_at": "2024-01-15T16:32:30Z"
      },
      {
        "name": "compressing_files",
        "status": "processing",
        "started_at": "2024-01-15T16:32:30Z",
        "progress": 65
      }
    ],
    "files_processed": 10000,
    "total_files": 15420,
    "bytes_processed": 671088640,
    "total_bytes": 1073741824,
    "estimated_completion": "2024-01-15T16:35:00Z"
  },
  "timestamp": "2024-01-15T16:33:00Z"
}
```

### POST `/servers/:id/backups/:backupId/restore`
**Opis**: Przywróć serwer z backupu  
**Limit**: 3 żądania/godzinę  
**Wymagania**: Bearer Token + permission `servers.backups.restore`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| restore_files | boolean | ❌ | Przywróć pliki (default: true) |
| restore_databases | boolean | ❌ | Przywróć bazy danych (default: true) |
| stop_server | boolean | ❌ | Zatrzymaj serwer przed przywróceniem |
| backup_before_restore | boolean | ❌ | Utwórz backup przed przywróceniem |
| overwrite_existing | boolean | ❌ | Zastąp istniejące pliki |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/backups/backup_123/restore" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "restore_files": true,
    "restore_databases": true,
    "stop_server": true,
    "backup_before_restore": true,
    "overwrite_existing": true
  }'
```

#### Odpowiedzi

**✅ 202 - Przywracanie rozpoczęte**
```json
{
  "success": true,
  "data": {
    "restore_id": "restore_123456",
    "backup_id": "backup_123",
    "status": "pending",
    "estimated_time": 600,
    "pre_restore_backup": "backup_987654321",
    "created_at": "2024-01-15T16:30:00Z",
    "progress_url": "/api/servers/srv_123/restores/restore_123456/progress"
  },
  "message": "Backup restore started",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/backups/:backupId/download`
**Opis**: Pobierz backup  
**Limit**: 5 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.backups.download`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| format | string | ❌ | Format pobierania (direct, signed_url) |
| expires_in | number | ❌ | Ważność linku w sekundach (max 3600) |

### DELETE `/servers/:id/backups/:backupId`
**Opis**: Usuń backup  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.backups.delete`

### GET `/servers/:id/restores`
**Opis**: Pobierz historię przywracania backupów  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.backups.read`

### GET `/servers/:id/restores/:restoreId/progress`
**Opis**: Pobierz postęp przywracania backupu  
**Limit**: 300 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.backups.read`

---

## ⏰ Harmonogram

### GET `/servers/:id/schedules`
**Opis**: Pobierz zaplanowane zadania serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.schedules.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| active | boolean | ❌ | Filtruj po statusie aktywności |
| type | string | ❌ | Filtruj po typie (backup, restart, command) |
| next_run_before | string | ❌ | Zadania do wykonania przed datą |
| next_run_after | string | ❌ | Zadania do wykonania po dacie |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/schedules?active=true&type=backup" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Lista harmonogramów**
```json
{
  "success": true,
  "data": [
    {
      "id": "schedule_1234567890",
      "name": "Daily Backup",
      "description": "Automatyczny backup codziennie o 3:00",
      "cron": "0 3 * * *",
      "timezone": "Europe/Warsaw",
      "active": true,
      "type": "backup",
      "tasks": [
        {
          "id": "task_1",
          "action": "backup",
          "payload": {
            "name": "Daily automated backup",
            "include_databases": true,
            "compression": "gzip"
          },
          "time_offset": 0,
          "continue_on_failure": false
        }
      ],
      "execution_history": {
        "total_runs": 45,
        "successful_runs": 44,
        "failed_runs": 1,
        "last_success": "2024-01-15T03:00:00Z",
        "last_failure": "2024-01-10T03:00:00Z"
      },
      "next_run": "2024-01-16T03:00:00Z",
      "last_run": {
        "started_at": "2024-01-15T03:00:00Z",
        "completed_at": "2024-01-15T03:05:30Z",
        "status": "success",
        "duration": 330,
        "output": "Backup completed successfully"
      },
      "created_by": {
        "id": "usr_123",
        "username": "admin"
      },
      "created_at": "2024-01-10T10:30:00Z",
      "updated_at": "2024-01-15T09:00:00Z"
    }
  ],
  "meta": {
    "total_schedules": 5,
    "active_schedules": 4,
    "next_execution": "2024-01-15T18:00:00Z"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/schedules`
**Opis**: Utwórz nowy harmonogram  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.schedules.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa harmonogramu |
| description | string | ❌ | Opis harmonogramu |
| cron | string | ✅ | Wyrażenie cron |
| timezone | string | ❌ | Strefa czasowa (default: UTC) |
| active | boolean | ❌ | Czy harmonogram jest aktywny |
| tasks | array | ✅ | Lista zadań do wykonania |

#### Struktura zadania (task)
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| action | string | ✅ | Akcja (backup, restart, stop, start, command) |
| payload | object | ❌ | Parametry akcji |
| time_offset | number | ❌ | Opóźnienie w sekundach |
| continue_on_failure | boolean | ❌ | Kontynuuj przy błędzie |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/schedules" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Weekly Restart",
    "description": "Restart serwera co niedzielę o 4:00",
    "cron": "0 4 * * 0",
    "timezone": "Europe/Warsaw",
    "active": true,
    "tasks": [
      {
        "action": "command",
        "payload": {
          "command": "say Server restart in 5 minutes!"
        },
        "time_offset": 0
      },
      {
        "action": "command",
        "payload": {
          "command": "say Server restart in 1 minute!"
        },
        "time_offset": 240
      },
      {
        "action": "restart",
        "payload": {},
        "time_offset": 300
      }
    ]
  }'
```

#### Odpowiedzi

**✅ 201 - Harmonogram utworzony**
```json
{
  "success": true,
  "data": {
    "id": "schedule_9876543210",
    "name": "Weekly Restart",
    "cron": "0 4 * * 0",
    "timezone": "Europe/Warsaw",
    "active": true,
    "next_run": "2024-01-21T04:00:00Z",
    "created_at": "2024-01-15T16:30:00Z"
  },
  "message": "Schedule created successfully",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/schedules/:scheduleId`
**Opis**: Pobierz szczegóły harmonogramu  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.schedules.read`

### PUT `/servers/:id/schedules/:scheduleId`
**Opis**: Aktualizuj harmonogram  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.schedules.update`

### DELETE `/servers/:id/schedules/:scheduleId`
**Opis**: Usuń harmonogram  
**Limit**: 10 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.schedules.delete`

### POST `/servers/:id/schedules/:scheduleId/run`
**Opis**: Uruchom harmonogram natychmiast  
**Limit**: 10 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.schedules.execute`

### GET `/servers/:id/schedules/:scheduleId/history`
**Opis**: Pobierz historię wykonań harmonogramu  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.schedules.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| status | string | ❌ | Filtruj po statusie (success, failed, running) |
| limit | number | ❌ | Liczba rekordów (max 100) |
| date_from | string | ❌ | Data początkowa |
| date_to | string | ❌ | Data końcowa |

---

## Cron Jobs

### GET `/servers/:id/cron-jobs`
**Opis**: Pobierz listę zadań cron  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.cron.read`

### POST `/servers/:id/cron-jobs`
**Opis**: Utwórz nowe zadanie cron  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.cron.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa zadania |
| command | string | ✅ | Komenda do wykonania |
| cron | string | ✅ | Wyrażenie cron |
| working_directory | string | ❌ | Katalog roboczy |
| environment | object | ❌ | Zmienne środowiskowe |

---

## Triggers & Webhooks

### GET `/servers/:id/webhooks`
**Opis**: Pobierz listę webhooków  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.webhooks.read`

### POST `/servers/:id/webhooks`
**Opis**: Utwórz nowy webhook  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.webhooks.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa webhooka |
| url | string | ✅ | URL docelowy |
| events | array | ✅ | Lista eventów |
| secret | string | ❌ | Sekret do podpisu |
| active | boolean | ❌ | Czy webhook jest aktywny |

#### Dostępne eventy
- `server.started`
- `server.stopped`
- `server.crashed`
- `backup.completed`
- `backup.failed`
- `user.joined`
- `user.left`
- `console.message`

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/webhooks" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Discord Notifications",
    "url": "https://discord.com/api/webhooks/123456789/abcdef",
    "events": ["server.started", "server.stopped", "backup.completed"],
    "secret": "webhook_secret_123",
    "active": true
  }'
```

### POST `/servers/:id/webhooks/:webhookId/test`
**Opis**: Przetestuj webhook  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.webhooks.test`

---

## Notifications

### GET `/notifications`
**Opis**: Pobierz powiadomienia użytkownika  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| read | boolean | ❌ | Filtruj po statusie przeczytania |
| type | string | ❌ | Filtruj po typie powiadomienia |
| server_id | string | ❌ | Filtruj po serwerze |
| limit | number | ❌ | Liczba powiadomień (max 100) |

#### Odpowiedzi

**✅ 200 - Lista powiadomień**
```json
{
  "success": true,
  "data": [
    {
      "id": "notif_1234567890",
      "type": "server_crashed",
      "title": "Server crashed",
      "message": "Your server 'Minecraft Production' has crashed and needs attention",
      "server": {
        "id": "srv_123",
        "name": "Minecraft Production"
      },
      "read": false,
      "priority": "high",
      "action_required": true,
      "actions": [
        {
          "label": "Restart Server",
          "endpoint": "/api/servers/srv_123/start",
          "method": "POST"
        },
        {
          "label": "View Logs",
          "endpoint": "/api/servers/srv_123/logs",
          "method": "GET"
        }
      ],
      "created_at": "2024-01-15T16:25:00Z"
    }
  ],
  "meta": {
    "unread_count": 5,
    "total_count": 25
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### PUT `/notifications/:id/read`
**Opis**: Oznacz powiadomienie jako przeczytane  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token

### POST `/notifications/mark-all-read`
**Opis**: Oznacz wszystkie powiadomienia jako przeczytane  
**Limit**: 10 żądań/minutę  
**Wymagania**: Bearer Token

### GET `/users/notification-settings`
**Opis**: Pobierz ustawienia powiadomień  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token

### PUT `/users/notification-settings`
**Opis**: Aktualizuj ustawienia powiadomień  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| email_notifications | object | ❌ | Ustawienia emaili |
| push_notifications | object | ❌ | Ustawienia push |
| discord_webhook | string | ❌ | URL webhooka Discord |

---

## 📊 Logi i Monitorowanie

### GET `/logs`
**Opis**: Pobierz logi systemowe  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `logs.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| server_id | string | ❌ | Filtruj po ID serwera |
| level | string | ❌ | Poziom logów (debug,info,warning,error,critical) |
| source | string | ❌ | Źródło logów (server,system,api,auth) |
| search | string | ❌ | Wyszukaj w treści |
| date_from | string | ❌ | Data początkowa (ISO 8601) |
| date_to | string | ❌ | Data końcowa (ISO 8601) |
| limit | number | ❌ | Liczba logów (1-1000, default: 100) |
| offset | number | ❌ | Przesunięcie dla paginacji |
| format | string | ❌ | Format odpowiedzi (json, text) |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/logs?level=error&server_id=srv_123&limit=50&date_from=2024-01-15T00:00:00Z" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Logi systemowe**
```json
{
  "success": true,
  "data": [
    {
      "id": "log_1234567890abcdef",
      "timestamp": "2024-01-15T16:30:00.123Z",
      "level": "error",
      "source": "server",
      "category": "power_management",
      "message": "Failed to start server srv_123: insufficient resources",
      "server": {
        "id": "srv_123",
        "name": "Minecraft Production",
        "type": "minecraft"
      },
      "user": {
        "id": "usr_456",
        "username": "admin",
        "ip": "192.168.1.100"
      },
      "metadata": {
        "error_code": "INSUFFICIENT_MEMORY",
        "required_memory": 4096,
        "available_memory": 2048,
        "node_id": "node_1",
        "request_id": "req_789",
        "stack_trace": "java.lang.OutOfMemoryError: Java heap space\n\tat com.example.Server.start(Server.java:123)"
      },
      "tags": ["critical", "resource_management"],
      "fingerprint": "a1b2c3d4e5f6",
      "count": 3,
      "first_occurrence": "2024-01-15T16:28:00Z",
      "last_occurrence": "2024-01-15T16:30:00Z"
    }
  ],
  "meta": {
    "pagination": {
      "total": 1250,
      "count": 50,
      "limit": 50,
      "offset": 0,
      "has_more": true
    },
    "statistics": {
      "total_logs": 1250,
      "levels": {
        "debug": 500,
        "info": 600,
        "warning": 100,
        "error": 45,
        "critical": 5
      },
      "sources": {
        "server": 800,
        "system": 300,
        "api": 100,
        "auth": 50
      }
    },
    "filters_applied": {
      "level": "error",
      "server_id": "srv_123",
      "date_from": "2024-01-15T00:00:00Z"
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/logs`
**Opis**: Pobierz logi konkretnego serwera  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + access to server

#### Parametry zapytania (podobne jak w `/logs` plus dodatkowe)
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| type | string | ❌ | Typ logów (console, system, error, access) |
| tail | boolean | ❌ | Pokaż najnowsze logi (live tail) |
| follow | boolean | ❌ | Śledź logi w czasie rzeczywistym |

#### Odpowiedzi

**✅ 200 - Logi serwera**
```json
{
  "success": true,
  "data": [
    {
      "id": "log_srv123_001",
      "timestamp": "2024-01-15T16:30:00.456Z",
      "type": "console",
      "level": "info",
      "message": "[16:30:00] [Server thread/INFO]: Player1 joined the game",
      "raw_message": "[16:30:00] [Server thread/INFO]: Player1 joined the game",
      "parsed_data": {
        "player": "Player1",
        "action": "join",
        "thread": "Server thread"
      },
      "metadata": {
        "process_id": 1234,
        "memory_usage": 2048,
        "cpu_usage": 45.2
      }
    },
    {
      "id": "log_srv123_002",
      "timestamp": "2024-01-15T16:30:15.789Z",
      "type": "system",
      "level": "warning",
      "message": "High memory usage detected: 95% of allocated memory used",
      "metadata": {
        "memory_used": 3891,
        "memory_total": 4096,
        "memory_percentage": 95.0,
        "alert_threshold": 90.0
      }
    }
  ],
  "meta": {
    "server": {
      "id": "srv_123",
      "name": "Minecraft Production",
      "status": "running"
    },
    "log_types": ["console", "system", "error"],
    "live_tail_url": "/api/servers/srv_123/logs/tail"
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/logs/tail`
**Opis**: Śledź logi w czasie rzeczywistym (Server-Sent Events)  
**Limit**: 10 połączeń jednocześnie  
**Wymagania**: Bearer Token + access to server

#### Nagłówki odpowiedzi
```
Content-Type: text/event-stream
Cache-Control: no-cache
Connection: keep-alive
```

#### Format strumienia
```
data: {"timestamp": "2024-01-15T16:30:00Z", "level": "info", "message": "[16:30:00] [Server thread/INFO]: Player joined"}

data: {"timestamp": "2024-01-15T16:30:01Z", "level": "info", "message": "[16:30:01] [Server thread/INFO]: Saving world..."}

event: status_change
data: {"server_status": "stopping", "timestamp": "2024-01-15T16:30:02Z"}
```

---

## Application Logs

### GET `/servers/:id/logs/application`
**Opis**: Pobierz logi aplikacji/gry  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.logs.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| log_file | string | ❌ | Konkretny plik logu |
| player | string | ❌ | Filtruj po graczu |
| event_type | string | ❌ | Typ eventu (join, leave, chat, death, etc.) |
| contains | string | ❌ | Zawiera tekst |

### GET `/servers/:id/logs/crash`
**Opis**: Pobierz logi crashy  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.logs.read`

### GET `/servers/:id/logs/performance`
**Opis**: Pobierz logi wydajności  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.logs.read`

---

## Performance Metrics

### GET `/servers/:id/metrics`
**Opis**: Pobierz metryki wydajności serwera  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + access to server

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| metric | string | ❌ | Konkretna metryka (cpu,memory,disk,network) |
| interval | string | ❌ | Interwał (1m,5m,15m,1h,6h,1d) |
| period | string | ❌ | Okres (1h,6h,24h,7d,30d) |
| aggregation | string | ❌ | Agregacja (avg,min,max,sum) |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/metrics?metric=cpu,memory&interval=5m&period=24h" \
  -H "Authorization: Bearer your_token_here"
```

#### Odpowiedzi

**✅ 200 - Metryki wydajności**
```json
{
  "success": true,
  "data": {
    "server": {
      "id": "srv_123",
      "name": "Minecraft Production"
    },
    "period": {
      "start": "2024-01-14T16:30:00Z",
      "end": "2024-01-15T16:30:00Z",
      "interval": "5m"
    },
    "metrics": {
      "cpu": {
        "unit": "percentage",
        "values": [
          {"timestamp": "2024-01-15T16:25:00Z", "value": 42.1},
          {"timestamp": "2024-01-15T16:30:00Z", "value": 45.2}
        ],
        "statistics": {
          "min": 15.5,
          "max": 89.3,
          "avg": 45.2,
          "current": 45.2
        }
      },
      "memory": {
        "unit": "bytes",
        "values": [
          {"timestamp": "2024-01-15T16:25:00Z", "value": 2097152000},
          {"timestamp": "2024-01-15T16:30:00Z", "value": 2147483648}
        ],
        "statistics": {
          "min": 1073741824,
          "max": 3221225472,
          "avg": 2147483648,
          "current": 2147483648
        }
      },
      "disk": {
        "unit": "bytes",
        "read": [
          {"timestamp": "2024-01-15T16:25:00Z", "value": 1048576},
          {"timestamp": "2024-01-15T16:30:00Z", "value": 2097152}
        ],
        "write": [
          {"timestamp": "2024-01-15T16:25:00Z", "value": 524288},
          {"timestamp": "2024-01-15T16:30:00Z", "value": 1048576}
        ]
      },
      "network": {
        "unit": "bytes",
        "rx": [
          {"timestamp": "2024-01-15T16:25:00Z", "value": 10485760},
          {"timestamp": "2024-01-15T16:30:00Z", "value": 20971520}
        ],
        "tx": [
          {"timestamp": "2024-01-15T16:25:00Z", "value": 5242880},
          {"timestamp": "2024-01-15T16:30:00Z", "value": 10485760}
        ]
      },
      "players": {
        "unit": "count",
        "values": [
          {"timestamp": "2024-01-15T16:25:00Z", "value": 18},
          {"timestamp": "2024-01-15T16:30:00Z", "value": 20}
        ],
        "statistics": {
          "min": 0,
          "max": 25,
          "avg": 15.8,
          "current": 20
        }
      }
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/servers/:id/metrics/realtime`
**Opis**: Pobierz aktualne metryki w czasie rzeczywistym  
**Limit**: 1000 żądań/minutę  
**Wymagania**: Bearer Token + access to server

#### Odpowiedzi

**✅ 200 - Metryki real-time**
```json
{
  "success": true,
  "data": {
    "timestamp": "2024-01-15T16:30:00Z",
    "cpu": {
      "usage": 45.2,
      "cores": 4,
      "load_average": [1.2, 1.1, 0.9]
    },
    "memory": {
      "used": 2147483648,
      "total": 4294967296,
      "percentage": 50.0,
      "swap_used": 0,
      "swap_total": 1073741824
    },
    "disk": {
      "used": 5368709120,
      "total": 10737418240,
      "percentage": 50.0,
      "iops": {
        "read": 150,
        "write": 75
      }
    },
    "network": {
      "rx_bytes_per_sec": 1048576,
      "tx_bytes_per_sec": 524288,
      "connections": 25
    },
    "application": {
      "players_online": 20,
      "max_players": 25,
      "tps": 19.8,
      "mspt": 42.5,
      "chunks_loaded": 1500,
      "entities": 2500
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

---

## Alerts & Warnings

### GET `/servers/:id/alerts`
**Opis**: Pobierz aktywne alerty serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + access to server

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| severity | string | ❌ | Poziom ważności (low,medium,high,critical) |
| category | string | ❌ | Kategoria (performance,security,resource,application) |
| status | string | ❌ | Status (active,acknowledged,resolved) |

#### Odpowiedzi

**✅ 200 - Lista alertów**
```json
{
  "success": true,
  "data": [
    {
      "id": "alert_1234567890",
      "title": "High Memory Usage",
      "description": "Server memory usage has exceeded 90% for more than 5 minutes",
      "severity": "high",
      "category": "performance",
      "status": "active",
      "metric": "memory_usage",
      "current_value": 95.2,
      "threshold_value": 90.0,
      "condition": "greater_than",
      "duration": 300,
      "first_triggered": "2024-01-15T16:25:00Z",
      "last_triggered": "2024-01-15T16:30:00Z",
      "trigger_count": 6,
      "actions": [
        {
          "type": "notification",
          "target": "email",
          "triggered": true,
          "triggered_at": "2024-01-15T16:25:00Z"
        },
        {
          "type": "webhook",
          "target": "discord",
          "triggered": true,
          "triggered_at": "2024-01-15T16:25:00Z"
        }
      ],
      "resolution_steps": [
        "Check for memory leaks in plugins",
        "Increase server memory allocation",
        "Restart server if issue persists"
      ]
    }
  ],
  "meta": {
    "total_alerts": 3,
    "by_severity": {
      "critical": 0,
      "high": 1,
      "medium": 2,
      "low": 0
    },
    "by_status": {
      "active": 2,
      "acknowledged": 1,
      "resolved": 0
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/alerts`
**Opis**: Utwórz nowy alert  
**Limit**: 20 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.alerts.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa alertu |
| metric | string | ✅ | Metryka do monitorowania |
| condition | string | ✅ | Warunek (gt,lt,eq,ne) |
| threshold | number | ✅ | Wartość progowa |
| duration | number | ❌ | Czas w sekundach przed triggerem |
| severity | string | ✅ | Poziom ważności |
| actions | array | ❌ | Akcje do wykonania |

### PUT `/servers/:id/alerts/:alertId/acknowledge`
**Opis**: Potwierdź alert  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.alerts.acknowledge`

### PUT `/servers/:id/alerts/:alertId/resolve`
**Opis**: Rozwiąż alert  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.alerts.resolve`

---

## 🔄 Real-time & WebSocket

### Połączenie WebSocket
**URL**: `ws://45.137.70.131:5000/ws`  
**Protokół**: WebSocket  
**Uwierzytelnianie**: Query parameter `token` lub header `Authorization`

#### Przykład połączenia
```javascript
const ws = new WebSocket('ws://45.137.70.131:5000/ws?token=your_jwt_token');

ws.onopen = function(event) {
  console.log('WebSocket connected');
  
  // Subscribe to server events
  ws.send(JSON.stringify({
    type: 'subscribe',
    channel: 'server.srv_123',
    events: ['console', 'stats', 'status']
  }));
};

ws.onmessage = function(event) {
  const message = JSON.parse(event.data);
  console.log('Received:', message);
};
```

### Kanały WebSocket

#### 1. Kanał konsoli serwera
**Kanał**: `server.{server_id}.console`  
**Wymagania**: permission `servers.console.read`

##### Subskrypcja
```json
{
  "type": "subscribe",
  "channel": "server.srv_123.console",
  "options": {
    "include_historical": false,
    "filter_level": "info"
  }
}
```

##### Wiadomości przychodzące
```json
{
  "type": "console_output",
  "channel": "server.srv_123.console",
  "data": {
    "timestamp": "2024-01-15T16:30:00Z",
    "level": "info",
    "message": "[16:30:00] [Server thread/INFO]: Player1 joined the game",
    "raw": "[16:30:00] [Server thread/INFO]: Player1 joined the game",
    "parsed": {
      "player": "Player1",
      "action": "join"
    }
  },
  "server_id": "srv_123",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

##### Wysyłanie komend
```json
{
  "type": "console_command",
  "channel": "server.srv_123.console",
  "data": {
    "command": "say Hello World!"
  }
}
```

#### 2. Kanał statystyk serwera
**Kanał**: `server.{server_id}.stats`  
**Wymagania**: access to server

##### Subskrypcja
```json
{
  "type": "subscribe",
  "channel": "server.srv_123.stats",
  "options": {
    "interval": 5,
    "metrics": ["cpu", "memory", "players"]
  }
}
```

##### Wiadomości przychodzące
```json
{
  "type": "stats_update",
  "channel": "server.srv_123.stats",
  "data": {
    "cpu": 45.2,
    "memory": {
      "used": 2147483648,
      "percentage": 50.0
    },
    "players": {
      "online": 20,
      "max": 25
    },
    "network": {
      "rx": 1048576,
      "tx": 524288
    }
  },
  "server_id": "srv_123",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

#### 3. Kanał statusu serwera
**Kanał**: `server.{server_id}.status`  
**Wymagania**: access to server

##### Wiadomości przychodzące
```json
{
  "type": "status_change",
  "channel": "server.srv_123.status",
  "data": {
    "previous_status": "stopping",
    "new_status": "stopped",
    "reason": "manual_stop",
    "user": {
      "id": "usr_123",
      "username": "admin"
    }
  },
  "server_id": "srv_123",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

#### 4. Kanał plików serwera
**Kanał**: `server.{server_id}.files`  
**Wymagania**: permission `servers.files.read`

##### Wiadomości przychodzące
```json
{
  "type": "file_change",
  "channel": "server.srv_123.files",
  "data": {
    "action": "modified",
    "path": "/server.properties",
    "size": 1024,
    "modified_by": {
      "id": "usr_123",
      "username": "admin"
    }
  },
  "server_id": "srv_123",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

#### 5. Kanał backupów
**Kanał**: `server.{server_id}.backups`  
**Wymagania**: permission `servers.backups.read`

##### Wiadomości przychodzące
```json
{
  "type": "backup_progress",
  "channel": "server.srv_123.backups",
  "data": {
    "backup_id": "backup_123",
    "status": "processing",
    "progress": 65,
    "stage": "compressing",
    "estimated_completion": "2024-01-15T16:35:00Z"
  },
  "server_id": "srv_123",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### Globalne kanały

#### 1. Kanał powiadomień użytkownika
**Kanał**: `user.{user_id}.notifications`  
**Wymagania**: Bearer Token (własne powiadomienia)

```json
{
  "type": "notification",
  "channel": "user.usr_123.notifications",
  "data": {
    "id": "notif_789",
    "title": "Server Alert",
    "message": "High CPU usage on Minecraft Production",
    "severity": "warning",
    "server_id": "srv_123",
    "action_required": false
  },
  "user_id": "usr_123",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

#### 2. Kanał systemowy
**Kanał**: `system.events`  
**Wymagania**: permission `system.events.read`

```json
{
  "type": "system_event",
  "channel": "system.events",
  "data": {
    "event": "node_offline",
    "node": {
      "id": "node_2",
      "name": "US Node 1"
    },
    "affected_servers": ["srv_456", "srv_789"],
    "estimated_downtime": 1800
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### Zarządzanie subskrypcjami

#### Subskrypcja
```json
{
  "type": "subscribe",
  "channel": "server.srv_123.console",
  "options": {
    "filter_level": "info",
    "include_historical": true,
    "historical_limit": 100
  }
}
```

#### Anulowanie subskrypcji
```json
{
  "type": "unsubscribe",
  "channel": "server.srv_123.console"
}
```

#### Lista aktywnych subskrypcji
```json
{
  "type": "list_subscriptions"
}
```

#### Odpowiedź z listą subskrypcji
```json
{
  "type": "subscriptions_list",
  "data": {
    "subscriptions": [
      {
        "channel": "server.srv_123.console",
        "subscribed_at": "2024-01-15T16:25:00Z",
        "options": {
          "filter_level": "info"
        }
      }
    ],
    "total": 1
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

---

## Server-Sent Events

### GET `/servers/:id/events`
**Opis**: Stream eventów serwera przez SSE  
**Limit**: 10 połączeń jednocześnie per serwer  
**Wymagania**: Bearer Token + access to server

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| events | string | ❌ | Lista eventów (comma-separated) |
| include_heartbeat | boolean | ❌ | Dołącz heartbeat co 30s |

#### Przykładowe żądanie
```bash
curl -X GET "http://45.137.70.131:5000/api/servers/srv_123/events?events=console,stats,status" \
  -H "Authorization: Bearer your_token_here" \
  -H "Accept: text/event-stream"
```

#### Format odpowiedzi
```
Content-Type: text/event-stream
Cache-Control: no-cache
Connection: keep-alive

event: console
data: {"timestamp": "2024-01-15T16:30:00Z", "message": "Player joined"}

event: stats
data: {"cpu": 45.2, "memory": 50.0, "players": 20}

event: status
data: {"status": "running", "uptime": 3600}

event: heartbeat
data: {"timestamp": "2024-01-15T16:30:30Z"}
```

---

## Live Monitoring

### GET `/monitoring/dashboard`
**Opis**: Pobierz dane do dashboardu monitorowania  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| servers | string | ❌ | Lista ID serwerów (comma-separated) |
| refresh_interval | number | ❌ | Interwał odświeżania w sekundach |

#### Odpowiedzi

**✅ 200 - Dashboard data**
```json
{
  "success": true,
  "data": {
    "overview": {
      "total_servers": 15,
      "running_servers": 12,
      "total_players": 245,
      "total_memory_used": 32212254720,
      "total_memory_allocated": 64424509440,
      "alerts_count": {
        "critical": 0,
        "high": 2,
        "medium": 5,
        "low": 8
      }
    },
    "servers": [
      {
        "id": "srv_123",
        "name": "Minecraft Production",
        "status": "running",
        "players": {
          "online": 20,
          "max": 25
        },
        "resources": {
          "cpu": 45.2,
          "memory": 50.0,
          "disk": 30.5
        },
        "uptime": 86400,
        "last_restart": "2024-01-14T16:30:00Z",
        "alerts": 2
      }
    ],
    "nodes": [
      {
        "id": "node_1",
        "name": "Germany Node 1",
        "status": "online",
        "servers_count": 8,
        "resources": {
          "cpu": 25.5,
          "memory": 60.2,
          "disk": 45.0
        },
        "network": {
          "rx": 10485760,
          "tx": 5242880
        }
      }
    ],
    "recent_events": [
      {
        "timestamp": "2024-01-15T16:29:00Z",
        "type": "server_started",
        "server": "Minecraft Test",
        "user": "admin"
      }
    ]
  },
  "refresh_interval": 30,
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### GET `/monitoring/nodes`
**Opis**: Pobierz informacje o węzłach  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `nodes.read`

### GET `/monitoring/system-health`
**Opis**: Pobierz informacje o zdrowiu systemu  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `system.health.read`

---

## Chat & Communication

### GET `/servers/:id/chat`
**Opis**: Pobierz historię czatu serwera  
**Limit**: 200 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.chat.read`

#### Parametry zapytania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| limit | number | ❌ | Liczba wiadomości (max 500) |
| before | string | ❌ | Wiadomości przed ID |
| after | string | ❌ | Wiadomości po ID |
| player | string | ❌ | Filtruj po graczu |
| contains | string | ❌ | Zawiera tekst |

#### Odpowiedzi

**✅ 200 - Historia czatu**
```json
{
  "success": true,
  "data": [
    {
      "id": "chat_1234567890",
      "timestamp": "2024-01-15T16:30:00Z",
      "player": "Player1",
      "message": "Hello everyone!",
      "type": "public",
      "channel": "global",
      "metadata": {
        "coordinates": {
          "x": 100,
          "y": 64,
          "z": -50,
          "world": "world"
        },
        "prefix": "[Member]",
        "color": "white"
      }
    },
    {
      "id": "chat_1234567891",
      "timestamp": "2024-01-15T16:30:05Z",
      "player": "Player2",
      "message": "Hi Player1!",
      "type": "public",
      "channel": "global",
      "reply_to": "chat_1234567890"
    }
  ],
  "meta": {
    "total_messages": 1500,
    "active_players": 20,
    "channels": ["global", "local", "trade"]
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/chat`
**Opis**: Wyślij wiadomość do czatu serwera  
**Limit**: 60 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.chat.send`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| message | string | ✅ | Treść wiadomości |
| channel | string | ❌ | Kanał czatu |
| as_server | boolean | ❌ | Wyślij jako serwer |

### WebSocket Chat
**Kanał**: `server.{server_id}.chat`

```json
{
  "type": "chat_message",
  "channel": "server.srv_123.chat",
  "data": {
    "id": "chat_1234567892",
    "timestamp": "2024-01-15T16:30:10Z",
    "player": "Player3",
    "message": "Anyone want to trade?",
    "type": "public",
    "channel": "trade"
  },
  "server_id": "srv_123",
  "timestamp": "2024-01-15T16:30:10Z"
}
```

---

## 🛠️ SDK i Narzędzia Deweloperskie

### JavaScript/TypeScript SDK

#### Instalacja
```bash
npm install @pterodactyl/api-client
```

#### Podstawowe użycie
```typescript
import { PterodactylClient } from '@pterodactyl/api-client';

const client = new PterodactylClient({
  baseURL: 'http://45.137.70.131:5000/api',
  token: 'your-jwt-token'
});

// Pobierz serwery
const servers = await client.servers.list({
  page: 1,
  limit: 25,
  status: 'running'
});

// Uruchom serwer
await client.servers.start('srv_123');

// Wyślij komendę
await client.servers.console.send('srv_123', 'say Hello World!');

// WebSocket connection
const ws = client.realtime.connect();
ws.subscribe('server.srv_123.console', (message) => {
  console.log('Console:', message.data.message);
});
```

#### Zaawansowane funkcje
```typescript
// Batch operations
const results = await client.batch([
  client.servers.start('srv_123'),
  client.servers.start('srv_456'),
  client.servers.backup('srv_789')
]);

// File operations
await client.files.upload('srv_123', '/plugins', {
  files: [new File(buffer, 'plugin.jar')],
  overwrite: true
});

const content = await client.files.read('srv_123', '/server.properties');
await client.files.write('srv_123', '/server.properties', modifiedContent);

// Database operations
const databases = await client.databases.list('srv_123');
const result = await client.databases.execute('srv_123', 'db_123', {
  query: 'SELECT * FROM users WHERE last_login > ?',
  parameters: ['2024-01-01']
});
```

### Python SDK

#### Instalacja
```bash
pip install pterodactyl-api-client
```

#### Podstawowe użycie
```python
from pterodactyl_api import PterodactylAPI
import asyncio

api = PterodactylAPI(
    base_url='http://45.137.70.131:5000/api',
    token='your-jwt-token'
)

async def main():
    # Pobierz serwery
    servers = await api.servers.list(status='running')
    
    # Uruchom serwer
    await api.servers.start('srv_123')
    
    # Upload pliku
    with open('plugin.jar', 'rb') as f:
        await api.files.upload('srv_123', '/plugins/', 'plugin.jar', f)
    
    # WebSocket
    async with api.realtime.connect() as ws:
        await ws.subscribe('server.srv_123.console')
        async for message in ws:
            print(f"Console: {message['data']['message']}")

asyncio.run(main())
```

### PHP SDK

#### Instalacja
```bash
composer require pterodactyl/api-client
```

#### Podstawowe użycie
```php
<?php
use Pterodactyl\ApiClient;

$client = new ApiClient([
    'base_url' => 'http://45.137.70.131:5000/api',
    'token' => 'your-jwt-token'
]);

// Pobierz serwery
$servers = $client->servers()->list([
    'status' => 'running',
    'limit' => 25
]);

// Uruchom serwer
$client->servers()->start('srv_123');

// Wyślij komendę
$client->console()->send('srv_123', 'say Hello from PHP!');

// Upload pliku
$client->files()->upload('srv_123', '/plugins', [
    'files' => ['plugin.jar' => file_get_contents('plugin.jar')],
    'overwrite' => true
]);
```

### Go SDK

#### Instalacja
```bash
go get github.com/pterodactyl/api-client-go
```

#### Podstawowe użycie
```go
package main

import (
    "context"
    "fmt"
    "github.com/pterodactyl/api-client-go"
)

func main() {
    client := pterodactyl.NewClient("http://45.137.70.131:5000/api", "your-jwt-token")
    ctx := context.Background()
    
    // Pobierz serwery
    servers, err := client.Servers.List(ctx, &pterodactyl.ServersListOptions{
        Status: "running",
        Limit:  25,
    })
    if err != nil {
        panic(err)
    }
    
    // Uruchom serwer
    err = client.Servers.Start(ctx, "srv_123")
    if err != nil {
        panic(err)
    }
    
    // WebSocket
    ws, err := client.Realtime.Connect(ctx)
    if err != nil {
        panic(err)
    }
    defer ws.Close()
    
    ws.Subscribe("server.srv_123.console")
    for message := range ws.Messages() {
        fmt.Printf("Console: %s\n", message.Data["message"])
    }
}
```

---

## CLI Tools

### Instalacja CLI
```bash
npm install -g @pterodactyl/cli
# lub
curl -L https://github.com/pterodactyl/cli/releases/latest/download/pterodactyl-cli-linux -o /usr/local/bin/pterodactyl
chmod +x /usr/local/bin/pterodactyl
```

### Konfiguracja
```bash
pterodactyl configure
# API URL: http://45.137.70.131:5000/api
# Token: your-jwt-token
```

### Podstawowe komendy
```bash
# Lista serwerów
pterodactyl servers list --status running

# Szczegóły serwera
pterodactyl servers show srv_123

# Uruchom serwer
pterodactyl servers start srv_123

# Zatrzymaj serwer
pterodactyl servers stop srv_123

# Wyślij komendę
pterodactyl console send srv_123 "say Hello from CLI!"

# Śledź konsole
pterodactyl console tail srv_123

# Lista plików
pterodactyl files list srv_123 /plugins

# Upload pliku
pterodactyl files upload srv_123 /plugins plugin.jar

# Download pliku
pterodactyl files download srv_123 /world world.zip

# Backup
pterodactyl backup create srv_123 --name "Manual backup"

# Lista backupów
pterodactyl backup list srv_123

# Przywróć backup
pterodactyl backup restore srv_123 backup_123
```

### Zaawansowane funkcje
```bash
# Batch operations
pterodactyl servers start srv_123 srv_456 srv_789

# Filtrowanie i formatowanie
pterodactyl servers list --format json | jq '.[] | select(.status == "running")'

# Monitoring
pterodactyl servers monitor srv_123 --metrics cpu,memory --interval 5s

# Harmonogramy
pterodactyl schedules list srv_123
pterodactyl schedules create srv_123 --name "Daily restart" --cron "0 4 * * *"

# Logi
pterodactyl logs tail srv_123 --level error --follow
pterodactyl logs search srv_123 --query "OutOfMemoryError" --last 24h
```

---

## Postman Collection

### Import kolekcji
1. **URL kolekcji**: `http://45.137.70.131:5000/api/postman-collection.json`
2. **Import do Postman**:
   - Otwórz Postman
   - Kliknij "Import"
   - Wklej URL kolekcji
   - Kliknij "Import"

### Struktura kolekcji
```
Pterodactyl API
├── Authentication
│   ├── Login
│   ├── Test Login
│   ├── Refresh Token
│   └── Logout
├── Servers
│   ├── List Servers
│   ├── Create Server
│   ├── Get Server Details
│   ├── Update Server
│   ├── Delete Server
│   ├── Power Management
│   │   ├── Start Server
│   │   ├── Stop Server
│   │   ├── Restart Server
│   │   └── Kill Server
│   └── Console
│       ├── Get Console History
│       └── Send Command
├── Files
│   ├── List Files
│   ├── Get File Content
│   ├── Update File
│   ├── Upload Files
│   ├── Download File
│   ├── Create File/Directory
│   ├── Delete Files
│   └── File Operations
├── Databases
│   ├── List Databases
│   ├── Create Database
│   ├── Database Details
│   ├── Update Database
│   ├── Delete Database
│   └── Execute Query
├── Users
│   ├── List Server Users
│   ├── Add User
│   ├── Update User Permissions
│   └── Remove User
├── Backups
│   ├── List Backups
│   ├── Create Backup
│   ├── Backup Details
│   ├── Restore Backup
│   ├── Download Backup
│   └── Delete Backup
├── Schedules
│   ├── List Schedules
│   ├── Create Schedule
│   ├── Schedule Details
│   ├── Update Schedule
│   ├── Delete Schedule
│   └── Run Schedule
├── Monitoring
│   ├── Server Stats
│   ├── System Logs
│   ├── Performance Metrics
│   └── Alerts
└── Admin
    ├── Nodes
    ├── System Health
    └── User Management
```

### Zmienne środowiskowe
```json
{
  "base_url": "http://45.137.70.131:5000/api",
  "token": "{{auth_token}}",
  "server_id": "srv_123",
  "database_id": "db_123",
  "backup_id": "backup_123"
}
```

### Pre-request Scripts
```javascript
// Auto-refresh token if expired
if (pm.environment.get('token_expires_at')) {
  const expiresAt = new Date(pm.environment.get('token_expires_at'));
  const now = new Date();
  
  if (now >= expiresAt) {
    // Token expired, refresh it
    pm.sendRequest({
      url: pm.environment.get('base_url') + '/auth/refresh',
      method: 'POST',
      header: {
        'Content-Type': 'application/json'
      },
      body: {
        mode: 'raw',
        raw: JSON.stringify({
          refresh_token: pm.environment.get('refresh_token')
        })
      }
    }, function(err, response) {
      if (!err && response.json().success) {
        const data = response.json().data;
        pm.environment.set('token', data.token);
        pm.environment.set('token_expires_at', 
          new Date(Date.now() + data.expires_in * 1000).toISOString()
        );
      }
    });
  }
}
```

---

## OpenAPI Specification

### Download specyfikacji
- **URL**: `http://45.137.70.131:5000/api/openapi.json`
- **Format**: OpenAPI 3.0.3
- **Encoding**: UTF-8

### Informacje o API
```yaml
openapi: 3.0.3
info:
  title: Pterodactyl Server Management API
  description: Complete API for managing game servers, files, databases, and more
  version: 1.0.0
  contact:
    name: API Support
    url: https://example.com/support
    email: support@example.com
  license:
    name: Proprietary
    url: https://example.com/license
servers:
  - url: http://45.137.70.131:5000/api
    description: Production server
  - url: http://localhost:5000/api
    description: Development server
```

### Generowanie klientów
```bash
# JavaScript/TypeScript
npx @openapitools/openapi-generator-cli generate \
  -i http://45.137.70.131:5000/api/openapi.json \
  -g typescript-axios \
  -o ./generated-client

# Python
openapi-generator generate \
  -i http://45.137.70.131:5000/api/openapi.json \
  -g python \
  -o ./python-client

# Java
openapi-generator generate \
  -i http://45.137.70.131:5000/api/openapi.json \
  -g java \
  -o ./java-client

# C#
openapi-generator generate \
  -i http://45.137.70.131:5000/api/openapi.json \
  -g csharp \
  -o ./csharp-client
```

---

## API Testing

### Unit Testing
```javascript
// Jest test example
import { PterodactylClient } from '@pterodactyl/api-client';

describe('Pterodactyl API', () => {
  const client = new PterodactylClient({
    baseURL: 'http://45.137.70.131:5000/api',
    token: process.env.TEST_TOKEN
  });

  test('should list servers', async () => {
    const response = await client.servers.list();
    expect(response.success).toBe(true);
    expect(Array.isArray(response.data)).toBe(true);
  });

  test('should start server', async () => {
    const response = await client.servers.start('srv_test_123');
    expect(response.success).toBe(true);
    expect(response.message).toContain('start command sent');
  });

  test('should handle authentication errors', async () => {
    const unauthorizedClient = new PterodactylClient({
      baseURL: 'http://45.137.70.131:5000/api',
      token: 'invalid-token'
    });

    await expect(unauthorizedClient.servers.list())
      .rejects.toThrow('Unauthorized');
  });
});
```

### Integration Testing
```python
import pytest
from pterodactyl_api import PterodactylAPI

@pytest.fixture
def api_client():
    return PterodactylAPI(
        base_url='http://45.137.70.131:5000/api',
        token=os.getenv('TEST_TOKEN')
    )

@pytest.mark.asyncio
async def test_server_lifecycle(api_client):
    # Create server
    server = await api_client.servers.create({
        'name': 'Test Server',
        'type': 'minecraft',
        'node_id': 'node_test',
        'allocation': {'ip': '127.0.0.1', 'port': 25565},
        'resources': {'memory': 1024, 'disk': 2048, 'cpu': 100}
    })
    
    assert server['success'] is True
    server_id = server['data']['id']
    
    try:
        # Start server
        start_result = await api_client.servers.start(server_id)
        assert start_result['success'] is True
        
        # Wait for server to start
        await asyncio.sleep(30)
        
        # Check status
        details = await api_client.servers.get(server_id)
        assert details['data']['status'] == 'running'
        
        # Stop server
        stop_result = await api_client.servers.stop(server_id)
        assert stop_result['success'] is True
        
    finally:
        # Cleanup
        await api_client.servers.delete(server_id)
```

### Load Testing
```javascript
// Artillery load test config
module.exports = {
  config: {
    target: 'http://45.137.70.131:5000/api',
    phases: [
      { duration: 60, arrivalRate: 10 },
      { duration: 120, arrivalRate: 20 },
      { duration: 60, arrivalRate: 5 }
    ],
    defaults: {
      headers: {
        'Authorization': 'Bearer {{$processEnvironment.TEST_TOKEN}}',
        'Content-Type': 'application/json'
      }
    }
  },
  scenarios: [
    {
      name: 'List servers',
      weight: 50,
      flow: [
        { get: { url: '/servers' } }
      ]
    },
    {
      name: 'Get server details',
      weight: 30,
      flow: [
        { get: { url: '/servers/srv_123' } }
      ]
    },
    {
      name: 'Send console command',
      weight: 20,
      flow: [
        {
          post: {
            url: '/servers/srv_123/console',
            json: { command: 'list' }
          }
        }
      ]
    }
  ]
};
```

---

## 🔧 Zaawansowane Funkcje

### Integrations

#### Discord Integration
```bash
# Konfiguracja webhookow Discord
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/webhooks" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Discord Notifications",
    "url": "https://discord.com/api/webhooks/123456789/abcdef",
    "events": ["server.started", "server.stopped", "backup.completed"],
    "format": "discord"
  }'
```

#### Slack Integration
```bash
curl -X POST "http://45.137.70.131:5000/api/integrations/slack" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "webhook_url": "https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX",
    "channel": "#server-alerts",
    "events": ["server.crashed", "backup.failed"]
  }'
```

#### Grafana Integration
```bash
# Export metryk do Grafana
curl -X POST "http://45.137.70.131:5000/api/integrations/grafana" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "prometheus_endpoint": "http://prometheus:9090",
    "metrics": ["cpu", "memory", "disk", "network", "players"],
    "interval": 30
  }'
```

---

## Plugins & Extensions

### GET `/plugins`
**Opis**: Pobierz listę dostępnych pluginów  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `plugins.read`

### GET `/servers/:id/plugins`
**Opis**: Pobierz zainstalowane pluginy serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + access to server

#### Odpowiedzi

**✅ 200 - Lista pluginów**
```json
{
  "success": true,
  "data": [
    {
      "id": "plugin_123",
      "name": "EssentialsX",
      "version": "2.20.1",
      "author": "EssentialsX Team",
      "description": "Podstawowe komendy i funkcje dla serwera Minecraft",
      "status": "enabled",
      "dependencies": ["Vault"],
      "file_name": "EssentialsX-2.20.1.jar",
      "file_size": 1048576,
      "installed_at": "2024-01-15T10:30:00Z",
      "last_updated": "2024-01-15T10:30:00Z",
      "config_files": [
        "/plugins/Essentials/config.yml",
        "/plugins/Essentials/worth.yml"
      ],
      "permissions": [
        "essentials.gamemode",
        "essentials.tp",
        "essentials.home"
      ]
    }
  ],
  "meta": {
    "total_plugins": 15,
    "enabled_plugins": 12,
    "disabled_plugins": 3,
    "total_size": 52428800
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

### POST `/servers/:id/plugins/install`
**Opis**: Zainstaluj plugin na serwerze  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.plugins.install`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| plugin_id | string | ❌ | ID pluginu z marketu |
| file_url | string | ❌ | URL do pliku pluginu |
| file | file | ❌ | Upload pliku pluginu |
| auto_enable | boolean | ❌ | Automatycznie włącz plugin |
| restart_server | boolean | ❌ | Restartuj serwer po instalacji |

### PUT `/servers/:id/plugins/:pluginId`
**Opis**: Zarządzaj pluginem (enable/disable/update)  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.plugins.manage`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| action | string | ✅ | Akcja (enable, disable, update, reload) |
| version | string | ❌ | Wersja do aktualizacji |

### DELETE `/servers/:id/plugins/:pluginId`
**Opis**: Usuń plugin z serwera  
**Limit**: 20 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.plugins.delete`

---

## API Hooks

### GET `/hooks`
**Opis**: Pobierz listę hooks API  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `hooks.read`

### POST `/hooks`
**Opis**: Utwórz nowy hook API  
**Limit**: 10 żądań/godzinę  
**Wymagania**: Bearer Token + permission `hooks.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa hooka |
| endpoint | string | ✅ | Endpoint to hookiem |
| method | string | ✅ | Metoda HTTP |
| events | array | ✅ | Lista eventów |
| script | string | ✅ | Kod JavaScript do wykonania |
| active | boolean | ❌ | Czy hook jest aktywny |

#### Przykład hooka
```javascript
// Hook script przykład - auto backup przed restartem
if (event.type === 'server.restart.before') {
  const backupResult = await api.servers.backup.create(event.server_id, {
    name: `Auto backup before restart - ${new Date().toISOString()}`,
    type: 'automatic'
  });
  
  if (!backupResult.success) {
    // Anuluj restart jeśli backup nie udał się
    return {
      cancel: true,
      reason: 'Backup failed, restart cancelled for safety'
    };
  }
  
  // Czekaj na zakończenie backupu
  await api.waitForBackupCompletion(backupResult.data.id, 600); // 10 min timeout
  
  return { continue: true };
}
```

---

## Custom Scripts

### GET `/servers/:id/scripts`
**Opis**: Pobierz niestandardowe skrypty serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.scripts.read`

### POST `/servers/:id/scripts`
**Opis**: Utwórz nowy skrypt  
**Limit**: 20 żądań/godzinę  
**Wymagania**: Bearer Token + permission `servers.scripts.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| name | string | ✅ | Nazwa skryptu |
| description | string | ❌ | Opis skryptu |
| language | string | ✅ | Język (bash, python, javascript, powershell) |
| content | string | ✅ | Kod skryptu |
| triggers | array | ❌ | Triggery automatyczne |
| parameters | array | ❌ | Parametry skryptu |

#### Przykład skryptu
```bash
#!/bin/bash
# Skrypt do automatycznego czyszczenia logów

echo "Starting log cleanup for server: $SERVER_NAME"

# Parametry
MAX_LOG_SIZE=${1:-100M}
LOG_DIR="/home/container/logs"

# Znajdź i skompresuj duże pliki logów
find "$LOG_DIR" -name "*.log" -size +$MAX_LOG_SIZE -exec gzip {} \;

# Usuń stare skompresowane logi (starsze niż 30 dni)
find "$LOG_DIR" -name "*.log.gz" -mtime +30 -delete

# Wyczyść cache
rm -rf /home/container/cache/*

echo "Log cleanup completed"
```

### POST `/servers/:id/scripts/:scriptId/execute`
**Opis**: Wykonaj skrypt  
**Limit**: 50 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.scripts.execute`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| parameters | object | ❌ | Parametry do przekazania skryptowi |
| async | boolean | ❌ | Wykonaj asynchronicznie |
| timeout | number | ❌ | Timeout w sekundach |

---

## Migrations

### GET `/servers/:id/migrations`
**Opis**: Pobierz listę migracji serwera  
**Limit**: 100 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.migrations.read`

### POST `/servers/:id/migrations`
**Opis**: Rozpocznij migrację serwera  
**Limit**: 3 żądania/godzinę  
**Wymagania**: Bearer Token + permission `servers.migrations.create`

#### Parametry żądania
| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| target_node | string | ✅ | ID węzła docelowego |
| migrate_files | boolean | ❌ | Migruj pliki |
| migrate_databases | boolean | ❌ | Migruj bazy danych |
| downtime_window | object | ❌ | Okno przestoju |
| backup_before | boolean | ❌ | Backup przed migracją |

#### Przykładowe żądanie
```bash
curl -X POST "http://45.137.70.131:5000/api/servers/srv_123/migrations" \
  -H "Authorization: Bearer your_token_here" \
  -H "Content-Type: application/json" \
  -d '{
    "target_node": "node_2",
    "migrate_files": true,
    "migrate_databases": true,
    "downtime_window": {
      "start": "2024-01-16T02:00:00Z",
      "duration": 3600
    },
    "backup_before": true
  }'
```

### GET `/servers/:id/migrations/:migrationId/progress`
**Opis**: Pobierz postęp migracji  
**Limit**: 300 żądań/minutę  
**Wymagania**: Bearer Token + permission `servers.migrations.read`

---

## 📖 Przykłady Użycia

### Automatyzacja Backupów
```javascript
// Automatyczny backup wszystkich serwerów
async function backupAllServers() {
  const client = new PterodactylClient({
    baseURL: 'http://45.137.70.131:5000/api',
    token: process.env.API_TOKEN
  });

  try {
    // Pobierz wszystkie serwery
    const servers = await client.servers.list({
      status: 'running',
      limit: 100
    });

    const backupPromises = servers.data.map(async (server) => {
      try {
        const backup = await client.servers.backup.create(server.id, {
          name: `Automated backup - ${new Date().toISOString()}`,
          type: 'scheduled',
          include_databases: true,
          compression: 'gzip'
        });
        
        console.log(`Backup started for ${server.name}: ${backup.data.id}`);
        return { server: server.name, status: 'started', backup_id: backup.data.id };
      } catch (error) {
        console.error(`Failed to backup ${server.name}:`, error.message);
        return { server: server.name, status: 'failed', error: error.message };
      }
    });

    const results = await Promise.all(backupPromises);
    
    // Wyślij raport
    const successful = results.filter(r => r.status === 'started').length;
    const failed = results.filter(r => r.status === 'failed').length;
    
    console.log(`Backup summary: ${successful} started, ${failed} failed`);
    
    // Opcjonalnie: wyślij powiadomienie na Discord/Slack
    await sendNotification({
      title: 'Backup Report',
      message: `${successful} backups started, ${failed} failed`,
      results: results
    });
    
  } catch (error) {
    console.error('Failed to backup servers:', error);
  }
}

// Uruchom codziennie o 3:00
cron.schedule('0 3 * * *', backupAllServers);
```

### Monitoring i Alerty
```python
import asyncio
from pterodactyl_api import PterodactylAPI
import discord
import smtplib

class ServerMonitor:
    def __init__(self, api_token, discord_webhook=None, email_config=None):
        self.api = PterodactylAPI(
            base_url='http://45.137.70.131:5000/api',
            token=api_token
        )
        self.discord_webhook = discord_webhook
        self.email_config = email_config
        self.thresholds = {
            'cpu': 90.0,
            'memory': 95.0,
            'disk': 90.0,
            'players': 0.95  # 95% of max players
        }

    async def check_server_health(self, server_id):
        try:
            # Pobierz metryki serwera
            metrics = await self.api.servers.metrics.realtime(server_id)
            alerts = []

            # Sprawdź CPU
            if metrics['cpu']['usage'] > self.thresholds['cpu']:
                alerts.append({
                    'type': 'cpu_high',
                    'message': f"High CPU usage: {metrics['cpu']['usage']:.1f}%",
                    'severity': 'warning'
                })

            # Sprawdź pamięć
            if metrics['memory']['percentage'] > self.thresholds['memory']:
                alerts.append({
                    'type': 'memory_high',
                    'message': f"High memory usage: {metrics['memory']['percentage']:.1f}%",
                    'severity': 'critical'
                })

            # Sprawdź dysk
            if metrics['disk']['percentage'] > self.thresholds['disk']:
                alerts.append({
                    'type': 'disk_high',
                    'message': f"High disk usage: {metrics['disk']['percentage']:.1f}%",
                    'severity': 'warning'
                })

            # Sprawdź graczy (tylko dla serwerów gier)
            if 'application' in metrics:
                player_ratio = metrics['application']['players_online'] / metrics['application']['max_players']
                if player_ratio > self.thresholds['players']:
                    alerts.append({
                        'type': 'players_high',
                        'message': f"Server almost full: {metrics['application']['players_online']}/{metrics['application']['max_players']}",
                        'severity': 'info'
                    })

            return alerts

        except Exception as e:
            return [{
                'type': 'monitoring_error',
                'message': f"Failed to check server health: {str(e)}",
                'severity': 'error'
            }]

    async def send_alert(self, server, alert):
        message = f"🚨 Alert for {server['name']}: {alert['message']}"
        
        # Discord notification
        if self.discord_webhook:
            await self.send_discord_alert(server, alert, message)
        
        # Email notification for critical alerts
        if alert['severity'] == 'critical' and self.email_config:
            await self.send_email_alert(server, alert, message)

    async def monitor_all_servers(self):
        servers = await self.api.servers.list(status='running')
        
        for server in servers.data:
            alerts = await self.check_server_health(server['id'])
            
            for alert in alerts:
                await self.send_alert(server, alert)
                
                # Automatyczne akcje naprawcze
                if alert['type'] == 'memory_high' and alert['severity'] == 'critical':
                    await self.auto_restart_server(server['id'])

    async def auto_restart_server(self, server_id):
        try:
            # Wyślij ostrzeżenie graccom
            await self.api.servers.console.send(server_id, 
                "say Server will restart in 2 minutes due to high memory usage!")
            
            await asyncio.sleep(60)
            await self.api.servers.console.send(server_id, 
                "say Server restarting in 1 minute!")
            
            await asyncio.sleep(60)
            await self.api.servers.restart(server_id)
            
            print(f"Auto-restarted server {server_id} due to critical memory usage")
            
        except Exception as e:
            print(f"Failed to auto-restart server {server_id}: {e}")

# Uruchom monitoring
async def main():
    monitor = ServerMonitor(
        api_token=os.getenv('API_TOKEN'),
        discord_webhook=os.getenv('DISCORD_WEBHOOK'),
        email_config={
            'smtp_server': 'smtp.gmail.com',
            'smtp_port': 587,
            'username': os.getenv('EMAIL_USER'),
            'password': os.getenv('EMAIL_PASS'),
            'to_emails': ['admin@example.com']
        }
    )
    
    # Monitoring co 5 minut
    while True:
        await monitor.monitor_all_servers()
        await asyncio.sleep(300)

if __name__ == '__main__':
    asyncio.run(main())
```

### Masowe Zarządzanie Serwerami
```bash
#!/bin/bash
# Skrypt do masowego zarządzania serwerami

API_URL="http://45.137.70.131:5000/api"
TOKEN="your-jwt-token"

# Funkcja do wywołania API
call_api() {
    local method=$1
    local endpoint=$2
    local data=$3
    
    curl -s -X "$method" \
        -H "Authorization: Bearer $TOKEN" \
        -H "Content-Type: application/json" \
        ${data:+-d "$data"} \
        "$API_URL$endpoint"
}

# Masowy restart serwerów z tagiem "production"
mass_restart_production() {
    echo "🔄 Starting mass restart of production servers..."
    
    # Pobierz serwery z tagiem production
    servers=$(call_api GET "/servers?tag=production&status=running" | jq -r '.data[].id')
    
    for server_id in $servers; do
        echo "📢 Sending restart announcement to $server_id"
        call_api POST "/servers/$server_id/console" \
            '{"command": "say Server will restart in 5 minutes for maintenance!"}'
    done
    
    # Czekaj 4 minuty
    sleep 240
    
    for server_id in $servers; do
        echo "📢 Final warning for $server_id"
        call_api POST "/servers/$server_id/console" \
            '{"command": "say Server restarting in 1 minute!"}'
    done
    
    # Czekaj minutę
    sleep 60
    
    # Restart wszystkich serwerów
    for server_id in $servers; do
        echo "🔄 Restarting server $server_id"
        result=$(call_api POST "/servers/$server_id/restart")
        
        if echo "$result" | jq -e '.success' > /dev/null; then
            echo "✅ Successfully restarted $server_id"
        else
            echo "❌ Failed to restart $server_id"
            echo "$result" | jq '.error'
        fi
    done
    
    echo "🎉 Mass restart completed!"
}

# Backup wszystkich serwerów przed aktualizacją
backup_before_update() {
    echo "💾 Creating backups before update..."
    
    servers=$(call_api GET "/servers?status=running" | jq -r '.data[].id')
    
    for server_id in $servers; do
        backup_name="Pre-update backup $(date +%Y-%m-%d_%H-%M)"
        
        echo "💾 Creating backup for $server_id: $backup_name"
        result=$(call_api POST "/servers/$server_id/backups" \
            "{\"name\": \"$backup_name\", \"type\": \"manual\", \"include_databases\": true}")
        
        if echo "$result" | jq -e '.success' > /dev/null; then
            backup_id=$(echo "$result" | jq -r '.data.id')
            echo "✅ Backup started for $server_id: $backup_id"
        else
            echo "❌ Failed to create backup for $server_id"
        fi
    done
}

# Sprawdź status wszystkich serwerów
check_all_servers_status() {
    echo "📊 Checking status of all servers..."
    
    result=$(call_api GET "/servers")
    
    echo "$result" | jq -r '.data[] | "\(.name) (\(.id)): \(.status) - Players: \(.current_usage.players.online // 0)/\(.current_usage.players.max // 0)"' | \
    while read line; do
        echo "🖥️  $line"
    done
    
    # Statystyki
    total=$(echo "$result" | jq '.data | length')
    running=$(echo "$result" | jq '.data | map(select(.status == "running")) | length')
    stopped=$(echo "$result" | jq '.data | map(select(.status == "stopped")) | length')
    
    echo ""
    echo "📈 Summary: $total total, $running running, $stopped stopped"
}

# Menu główne
case "$1" in
    "restart-production")
        mass_restart_production
        ;;
    "backup-all")
        backup_before_update
        ;;
    "status")
        check_all_servers_status
        ;;
    *)
        echo "Usage: $0 {restart-production|backup-all|status}"
        echo ""
        echo "Commands:"
        echo "  restart-production  - Restart all production servers with warnings"
        echo "  backup-all         - Create backups of all running servers"
        echo "  status            - Show status of all servers"
        exit 1
        ;;
esac
```

---

## Best Practices

### 1. Uwierzytelnianie i Bezpieczeństwo
```javascript
// ✅ DOBRE - Bezpieczne przechowywanie tokenów
class SecureApiClient {
    constructor() {
        this.token = process.env.API_TOKEN; // Używaj zmiennych środowiskowych
        this.refreshToken = process.env.REFRESH_TOKEN;
        this.tokenExpiry = null;
    }

    async makeRequest(endpoint, options = {}) {
        // Sprawdź czy token nie wygasł
        if (this.tokenExpiry && Date.now() >= this.tokenExpiry) {
            await this.refreshAccessToken();
        }

        const response = await fetch(`${API_URL}${endpoint}`, {
            ...options,
            headers: {
                'Authorization': `Bearer ${this.token}`,
                'Content-Type': 'application/json',
                ...options.headers
            }
        });

        if (response.status === 401) {
            await this.refreshAccessToken();
            // Retry request
            return this.makeRequest(endpoint, options);
        }

        return response;
    }
}

// ❌ ŹLE - Token w kodzie
const client = new ApiClient({
    token: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' // Nigdy nie rób tego!
});
```

### 2. Rate Limiting i Retry Logic
```python
import asyncio
import time
from typing import Optional

class ResilientApiClient:
    def __init__(self, base_url: str, token: str):
        self.base_url = base_url
        self.token = token
        self.rate_limit_remaining = 100
        self.rate_limit_reset = time.time() + 60

    async def make_request(self, method: str, endpoint: str, 
                          data: Optional[dict] = None, 
                          max_retries: int = 3) -> dict:
        
        for attempt in range(max_retries + 1):
            try:
                # Sprawdź rate limit
                if self.rate_limit_remaining <= 1:
                    wait_time = self.rate_limit_reset - time.time()
                    if wait_time > 0:
                        await asyncio.sleep(wait_time)

                response = await self._make_http_request(method, endpoint, data)
                
                # Aktualizuj informacje o rate limit
                self.rate_limit_remaining = int(response.headers.get('X-RateLimit-Remaining', 100))
                self.rate_limit_reset = int(response.headers.get('X-RateLimit-Reset', time.time() + 60))

                if response.status_code == 429:  # Rate limited
                    retry_after = int(response.headers.get('Retry-After', 60))
                    await asyncio.sleep(retry_after)
                    continue

                if response.status_code >= 500:  # Server error
                    if attempt < max_retries:
                        wait_time = 2 ** attempt  # Exponential backoff
                        await asyncio.sleep(wait_time)
                        continue

                return response.json()

            except Exception as e:
                if attempt < max_retries:
                    wait_time = 2 ** attempt
                    await asyncio.sleep(wait_time)
                    continue
                raise e

        raise Exception(f"Request failed after {max_retries} retries")
```

### 3. Efektywne Batch Operations
```javascript
// ✅ DOBRE - Batch operations
class BatchApiClient {
    constructor(client) {
        this.client = client;
        this.batchSize = 10;
    }

    async batchServerOperation(serverIds, operation, options = {}) {
        const results = [];
        
        // Podziel na batche
        for (let i = 0; i < serverIds.length; i += this.batchSize) {
            const batch = serverIds.slice(i, i + this.batchSize);
            
            // Wykonaj operacje równolegle w batchu
            const batchPromises = batch.map(async (serverId) => {
                try {
                    const result = await this.client.servers[operation](serverId, options);
                    return { serverId, success: true, data: result };
                } catch (error) {
                    return { serverId, success: false, error: error.message };
                }
            });

            const batchResults = await Promise.all(batchPromises);
            results.push(...batchResults);

            // Krótka pauza między batchami
            if (i + this.batchSize < serverIds.length) {
                await new Promise(resolve => setTimeout(resolve, 1000));
            }
        }

        return results;
    }
}

// Użycie
const batchClient = new BatchApiClient(apiClient);
const results = await batchClient.batchServerOperation(
    ['srv_123', 'srv_456', 'srv_789'],
    'start'
);
```

### 4. Error Handling
```javascript
// ✅ DOBRE - Comprehensive error handling
class ApiErrorHandler {
    static handle(error) {
        if (error.response) {
            const { status, data } = error.response;
            
            switch (status) {
                case 400:
                    throw new ValidationError(data.details || data.message);
                case 401:
                    throw new AuthenticationError('Token expired or invalid');
                case 403:
                    throw new PermissionError('Insufficient permissions');
                case 404:
                    throw new NotFoundError('Resource not found');
                case 429:
                    throw new RateLimitError('Rate limit exceeded', data.retry_after);
                case 500:
                    throw new ServerError('Internal server error');
                default:
                    throw new ApiError(`HTTP ${status}: ${data.message}`);
            }
        } else if (error.code === 'ECONNREFUSED') {
            throw new ConnectionError('Unable to connect to API server');
        } else {
            throw new UnknownError('An unexpected error occurred');
        }
    }
}

// Custom error classes
class ApiError extends Error {
    constructor(message, code = null) {
        super(message);
        this.name = this.constructor.name;
        this.code = code;
    }
}

class ValidationError extends ApiError {}
class AuthenticationError extends ApiError {}
class PermissionError extends ApiError {}
class NotFoundError extends ApiError {}
class RateLimitError extends ApiError {
    constructor(message, retryAfter) {
        super(message);
        this.retryAfter = retryAfter;
    }
}
```

### 5. Caching Strategy
```javascript
// ✅ DOBRE - Intelligent caching
class CachedApiClient {
    constructor(client) {
        this.client = client;
        this.cache = new Map();
        this.cacheTTL = {
            servers: 30000,      // 30s dla listy serwerów
            serverDetails: 10000, // 10s dla szczegółów serwera
            stats: 5000,         // 5s dla statystyk
            files: 60000         // 60s dla listowania plików
        };
    }

    async getServers(options = {}) {
        const cacheKey = `servers:${JSON.stringify(options)}`;
        const cached = this.getFromCache(cacheKey, this.cacheTTL.servers);
        
        if (cached) {
            return cached;
        }

        const data = await this.client.servers.list(options);
        this.setCache(cacheKey, data);
        return data;
    }

    async getServerStats(serverId) {
        const cacheKey = `stats:${serverId}`;
        const cached = this.getFromCache(cacheKey, this.cacheTTL.stats);
        
        if (cached) {
            return cached;
        }

        const data = await this.client.servers.stats(serverId);
        this.setCache(cacheKey, data);
        return data;
    }

    getFromCache(key, ttl) {
        const item = this.cache.get(key);
        if (item && Date.now() - item.timestamp < ttl) {
            return item.data;
        }
        return null;
    }

    setCache(key, data) {
        this.cache.set(key, {
            data,
            timestamp: Date.now()
        });
    }

    // Invalidate cache when data changes
    invalidateServerCache(serverId) {
        for (const [key] of this.cache) {
            if (key.includes(serverId) || key.startsWith('servers:')) {
                this.cache.delete(key);
            }
        }
    }
}
```

---

## Troubleshooting

### Częste Problemy i Rozwiązania

#### 1. Błędy Uwierzytelniania

**Problem**: `401 Unauthorized`
```json
{
  "success": false,
  "error": "Invalid or expired token"
}
```

**Rozwiązania**:
```javascript
// Sprawdź ważność tokenu
const jwt = require('jsonwebtoken');
const decoded = jwt.decode(token);
console.log('Token expires at:', new Date(decoded.exp * 1000));

// Odśwież token
const refreshResponse = await fetch('/api/auth/refresh', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ refresh_token: refreshToken })
});
```

#### 2. Rate Limiting

**Problem**: `429 Too Many Requests`
```json
{
  "success": false,
  "error": "Rate limit exceeded",
  "retry_after": 60
}
```

**Rozwiązania**:
```javascript
// Implementuj exponential backoff
async function makeRequestWithBackoff(fn, maxRetries = 3) {
    for (let i = 0; i < maxRetries; i++) {
        try {
            return await fn();
        } catch (error) {
            if (error.status === 429) {
                const delay = Math.min(1000 * Math.pow(2, i), 30000);
                await new Promise(resolve => setTimeout(resolve, delay));
                continue;
            }
            throw error;
        }
    }
    throw new Error('Max retries exceeded');
}
```

#### 3. WebSocket Connection Issues

**Problem**: WebSocket rozłącza się często

**Rozwiązania**:
```javascript
class ResilientWebSocket {
    constructor(url, token) {
        this.url = url;
        this.token = token;
        this.reconnectAttempts = 0;
        this.maxReconnectAttempts = 5;
        this.reconnectInterval = 1000;
        this.subscriptions = new Set();
        this.connect();
    }

    connect() {
        this.ws = new WebSocket(`${this.url}?token=${this.token}`);
        
        this.ws.onopen = () => {
            console.log('WebSocket connected');
            this.reconnectAttempts = 0;
            
            // Przywróć subskrypcje
            this.subscriptions.forEach(sub => {
                this.ws.send(JSON.stringify(sub));
            });
        };

        this.ws.onclose = () => {
            console.log('WebSocket disconnected');
            this.reconnect();
        };

        this.ws.onerror = (error) => {
            console.error('WebSocket error:', error);
        };
    }

    reconnect() {
        if (this.reconnectAttempts < this.maxReconnectAttempts) {
            this.reconnectAttempts++;
            const delay = this.reconnectInterval * Math.pow(2, this.reconnectAttempts);
            
            setTimeout(() => {
                console.log(`Reconnecting... (attempt ${this.reconnectAttempts})`);
                this.connect();
            }, delay);
        }
    }

    subscribe(channel, events) {
        const subscription = {
            type: 'subscribe',
            channel,
            events
        };
        
        this.subscriptions.add(subscription);
        
        if (this.ws.readyState === WebSocket.OPEN) {
            this.ws.send(JSON.stringify(subscription));
        }
    }
}
```

#### 4. Slow API Responses

**Diagnoza**:
```bash
# Sprawdź latencję
curl -w "@curl-format.txt" -s -o /dev/null http://45.137.70.131:5000/api/servers

# Format file (curl-format.txt):
# time_namelookup:  %{time_namelookup}\n
# time_connect:     %{time_connect}\n
# time_appconnect:  %{time_appconnect}\n
# time_pretransfer: %{time_pretransfer}\n
# time_redirect:    %{time_redirect}\n
# time_starttransfer: %{time_starttransfer}\n
# time_total:       %{time_total}\n
```

**Optymalizacje**:
```javascript
// Użyj pagination i filtering
const servers = await client.servers.list({
    limit: 25,        // Nie pobieraj za dużo danych
    include: 'stats', // Pobierz tylko potrzebne dane
    status: 'running' // Filtruj po stronie serwera
});

// Użyj parallel requests dla niezależnych operacji
const [servers, nodes, alerts] = await Promise.all([
    client.servers.list(),
    client.nodes.list(),
    client.alerts.list()
]);
```

#### 5. File Operations Errors

**Problem**: Upload fails
```json
{
  "success": false,
  "error": "File too large",
  "max_size": 104857600
}
```

**Rozwiązania**:
```javascript
// Sprawdź rozmiar pliku przed uploadem
function validateFileSize(file, maxSize = 100 * 1024 * 1024) { // 100MB
    if (file.size > maxSize) {
        throw new Error(`File too large: ${file.size} bytes (max: ${maxSize})`);
    }
}

// Upload w częściach dla dużych plików
async function uploadLargeFile(serverId, path, file) {
    const chunkSize = 5 * 1024 * 1024; // 5MB chunks
    const totalChunks = Math.ceil(file.size / chunkSize);
    
    for (let i = 0; i < totalChunks; i++) {
        const start = i * chunkSize;
        const end = Math.min(start + chunkSize, file.size);
        const chunk = file.slice(start, end);
        
        await client.files.uploadChunk(serverId, path, chunk, i, totalChunks);
    }
}
```

---

## FAQ

### Ogólne

**Q: Jak zdobyć token API?**
A: Zaloguj się przez `/auth/login` endpoint używając swoich danych uwierzytelniających. Token będzie w odpowiedzi.

**Q: Czy mogę używać API bez uwierzytelniania?**
A: Nie, wszystkie endpointy (poza `/auth/login` i `/auth/test-login`) wymagają ważnego Bearer tokenu.

**Q: Jak długo ważny jest token?**
A: Access token jest ważny przez 24 godziny. Refresh token jest ważny przez 30 dni.

**Q: Czy API obsługuje CORS?**
A: Tak, API ma skonfigurowane CORS headers dla cross-origin requests.

### Serwery

**Q: Czy mogę tworzyć serwery programowo?**
A: Tak, użyj `POST /servers` endpoint z odpowiednimi parametrami.

**Q: Jak sprawdzić czy serwer jest dostępny?**
A: Sprawdź pole `status` w odpowiedzi z `GET /servers/:id`. Status `running` oznacza że serwer działa.

**Q: Czy mogę restartować wiele serwerów jednocześnie?**
A: API nie ma endpoint do mass restart, ale możesz wywołać restart dla każdego serwera równolegle w swoim kodzie.

**Q: Jak często mogę pobierać statystyki serwera?**
A: Rate limit wynosi 1000 żądań/minutę dla endpoint `/servers/:id/metrics/realtime`.

### Pliki

**Q: Jaki jest maksymalny rozmiar pliku do uploadu?**
A: Domyślny limit to 100MB per plik. Sprawdź headers odpowiedzi dla dokładnych limitów.

**Q: Czy mogę uploadować wiele plików jednocześnie?**
A: Tak, endpoint `/servers/:id/files/upload` obsługuje multiple files w jednym request.

**Q: Jak downloads działają dla dużych plików?**
A: API generuje signed URLs dla bezpiecznego download. Możesz też użyć streaming download.

### WebSocket

**Q: Czy WebSocket reconnectuje automatycznie?**
A: Nie, musisz zaimplementować reconnect logic w swoim kliencie.

**Q: Ile równoczesnych połączeń WebSocket mogę mieć?**
A: Limit wynosi 10 równoczesnych połączeń per użytkownik.

**Q: Czy mogę subskrybować wiele kanałów jednocześnie?**
A: Tak, możesz subskrybować wiele kanałów w jednym połączeniu WebSocket.

### Bazy Danych

**Q: Jakie typy baz danych są obsługiwane?**
A: MySQL, PostgreSQL, MongoDB i Redis.

**Q: Czy mogę wykonywać bezpośrednie zapytania SQL?**
A: Tak, ale tylko dla MySQL i PostgreSQL przez endpoint `/execute`.

**Q: Jak utworzyć backup bazy danych?**
A: Użyj `POST /servers/:id/databases/:dbId/backup` endpoint.

### Backupy

**Q: Jak długo są przechowywane backupy?**
A: Domyślnie 30 dni, ale możesz to skonfigurować przy tworzeniu backupu.

**Q: Czy backupy są szyfrowane?**
A: Możesz włączyć szyfrowanie przez parametr `encryption: true` przy tworzeniu backupu.

**Q: Czy mogę zaplanować automatyczne backupy?**
A: Tak, użyj schedules endpoint do utworzenia cyklicznych backupów.

### Błędy i Debugging

**Q: Gdzie mogę sprawdzić logi błędów?**
A: Użyj `/logs` endpoint z filtrem `level=error`.

**Q: Jak reportować błędy w API?**
A: Skontaktuj się z supportem, podając request ID z header odpowiedzi.

**Q: Czy jest dostępny test environment?**
A: Tak, użyj `/auth/test-login` dla testowego dostępu.

---

## Changelog

### v1.0.0 (2024-01-15)
**🎉 Initial Release**

**Nowe funkcje:**
- ✨ Pełne API dla zarządzania serwerami
- ✨ System uwierzytelniania JWT
- ✨ WebSocket real-time communication
- ✨ File management system
- ✨ Database operations
- ✨ Backup & restore functionality
- ✨ Scheduled tasks
- ✨ User management
- ✨ Comprehensive logging
- ✨ Performance metrics
- ✨ Alert system

**Endpointy:**
- 🔐 Authentication: `/auth/*`
- 🖥️ Servers: `/servers/*`
- 📁 Files: `/servers/:id/files/*`
- 🗄️ Databases: `/servers/:id/databases/*`
- 👥 Users: `/servers/:id/users/*`
- 💾 Backups: `/servers/:id/backups/*`
- ⏰ Schedules: `/servers/:id/schedules/*`
- 📊 Monitoring: `/logs`, `/servers/:id/metrics/*`
- 🔄 WebSocket: `/ws`

**Limity API:**
- 🚫 Rate limiting per endpoint
- 📊 Usage metrics
- ⚠️ Alert system

**SDK i Narzędzia:**
- 📦 JavaScript/TypeScript SDK
- 🐍 Python SDK
- 🛠️ CLI tools
- 📋 Postman collection
- 📄 OpenAPI specification

### Planowane Updates

#### v1.1.0 (Planowane: 2024-02-01)
- 🔄 Improved WebSocket reliability
- 📈 Enhanced metrics collection
- 🎯 Better error messages
- 🚀 Performance optimizations

#### v1.2.0 (Planowane: 2024-03-01)
- 🤖 AI-powered server optimization
- 📱 Mobile API endpoints
- 🔗 Third-party integrations
- 📊 Advanced analytics

#### v2.0.0 (Planowane: 2024-06-01)
- 🏗️ GraphQL support
- 🔄 Event-driven architecture
- 🌐 Multi-region support
- 🎮 Game-specific features

---

## 📞 Wsparcie i Kontakt

### Dokumentacja i Zasoby
- **📚 Dokumentacja API**: Ten dokument
- **🔗 OpenAPI Spec**: `http://45.137.70.131:5000/api/openapi.json`
- **📋 Postman Collection**: `http://45.137.70.131:5000/api/postman-collection.json`
- **💻 GitHub Repository**: `https://github.com/pterodactyl/api-client`

### Community
- **💬 Discord Server**: `https://discord.gg/pterodactyl`
- **📧 Forum**: `https://community.pterodactyl.io`
- **❓ Stack Overflow**: Tag `pterodactyl-api`

### Support
- **🎫 Support Tickets**: `support@pterodactyl.io`
- **🐛 Bug Reports**: GitHub Issues
- **💡 Feature Requests**: GitHub Discussions

### Rate Limiting Info
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1642267200
```

### Status Page
- **🔍 API Status**: `https://status.pterodactyl.io`
- **📊 Uptime**: 99.9%
- **🏃‍♂️ Response Time**: <200ms average

---

*Ta dokumentacja została wygenerowana automatycznie z OpenAPI specyfikacji. Ostatnia aktualizacja: 2024-01-15T16:30:00Z*

**API Version**: v1.0  
**Documentation Version**: 1.0.0  
**Last Updated**: January 15, 2024  

*© 2024 Pterodactyl Software. All rights reserved.*
