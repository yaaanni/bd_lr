# Ненормализованная схема БД (Лабораторная работа №1)

**СУБД:** PostgreSQL

## Перечень сущностей БД (8 шт.)

1. **users** (Пользователи + Профиль + Адрес)
2. **roles** (Роли)
3. **permissions** (Разрешения)
4. **role_permissions** (Связи ролей и разрешений)
5. **user_logs** (Журнал действий)
6. **products** (Товары + Категории + Производители)
7. **orders** (Заказы)
8. **order_items** (Позиции заказа)

---

## Описание сущностей

### 1. users
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| id | SERIAL | PK | - |
| role_id | INT | NOT NULL | FK -> roles(id) |
| email | VARCHAR(255) | UNIQUE, NOT NULL | - |
| password_hash | VARCHAR(255) | NOT NULL | - |
| full_name | VARCHAR(150) | NOT NULL | - |
| phone | VARCHAR(20) | - | - |
| address_text | TEXT | - | - |
| is_banned | BOOLEAN | DEFAULT FALSE | - |
| deleted_at | TIMESTAMP | DEFAULT NULL | - |

### 2. roles
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| id | SERIAL | PK | - |
| name | VARCHAR(50) | UNIQUE, NOT NULL | - |

### 3. permissions
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| id | SERIAL | PK | - |
| name | VARCHAR(50) | UNIQUE, NOT NULL | - |

### 4. role_permissions
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| role_id | INT | PK | FK -> roles(id) |
| permission_id | INT | PK | FK -> permissions(id) |
| assigned_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | - |
| is_active | BOOLEAN | DEFAULT TRUE | - |

### 5. user_logs
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| id | SERIAL | PK | - |
| user_id | INT | NOT NULL | FK -> users(id) |
| action | VARCHAR(255) | NOT NULL | - |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | - |

### 6. products
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| id | SERIAL | PK | - |
| name | VARCHAR(150) | NOT NULL | - |
| price | DECIMAL(10,2) | NOT NULL, CHECK (price >= 0) | - |
| category_name | VARCHAR(100) | NOT NULL | - |
| manufacturer_name | VARCHAR(100) | - | - |
| manufacturer_country | VARCHAR(100) | - | - |
| deleted_at | TIMESTAMP | DEFAULT NULL | - |

### 7. orders
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| id | SERIAL | PK | - |
| user_id | INT | NOT NULL | FK -> users(id) |
| status | VARCHAR(50) | NOT NULL | - |
| delivery_address | TEXT | NOT NULL | - |
| customer_phone | VARCHAR(20) | NOT NULL | - |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | - |

### 8. order_items
| Поле | Тип | Ограничения | Связь |
| :--- | :--- | :--- | :--- |
| order_id | INT | PK | FK -> orders(id) |
| product_id | INT | PK | FK -> products(id) |
| quantity | INT | NOT NULL, CHECK (quantity > 0) | - |
| fixed_price | DECIMAL(10,2) | NOT NULL | - |