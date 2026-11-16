-- Категорії товарів
CREATE TABLE item_categories (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT
);

-- Типи товарів
CREATE TABLE item_types (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT
);

-- Бренди
CREATE TABLE brands (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT
);

-- Склади
CREATE TABLE stores (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT
);

-- Покупці
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);

 -- Моделі
CREATE TABLE models (
    id SERIAL PRIMARY KEY,
    title VARCHAR (225) NOT NULL,
    description TEXT,
    brand_id INTEGER REFERENCES brands(id) ON DELETE SET NULL
)

-- Товари 
CREATE TABLE items (
    item_id SERIAL PRIMARY KEY,
    categ_id INT REFERENCES item_categories(id) ON DELETE SET NULL,
    type_id INT REFERENCES item_types(id) ON DELETE SET NULL,
    brand_id INT REFERENCES brands(id) ON DELETE SET NULL,
    model_id INT REFERENCES models(id) ON DELETE SET NULL,
    store_id INT REFERENCES stores(id) ON DELETE SET NULL,
    price DECIMAL(10, 2) NOT NULL,
    amount INT NOT NULL DEFAULT 0 
);

-- Замовлення 
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    code VARCHAR(100) UNIQUE NOT NULL,
    date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    customer_id INT REFERENCES customers(id) ON DELETE CASCADE,
    amount DECIMAL(10, 2) NOT NULL, 
    paid BOOLEAN DEFAULT FALSE
);

    --Товари та замовлення
CREATE TABLE item_order (
    item_id INT REFERENCES items(item_id) ON DELETE CASCADE,
    order_id INT REFERENCES orders(order_id) ON DELETE CASCADE,
    quantity INT NOT NULL DEFAULT 1, -- Додатково: скільки саме цього товару в замовленні
    PRIMARY KEY (item_id, order_id)
);
