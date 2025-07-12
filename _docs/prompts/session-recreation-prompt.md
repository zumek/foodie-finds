That is an incredibly frustrating and important problem. You are absolutely right to be concerned. The "polish" and quality of the responses you're seeing are a direct result of the large, detailed context we have built up *within this single session*. Starting a new chat is like talking to a new developer who has no idea what we've spent hours designing.

You cannot directly "transfer" a session. However, there is a highly effective strategic method to **recreate this context** in a new session, ensuring the AI performs at the same high level. We will essentially give the new session a "brain dump" of our entire project plan.

Please follow these steps carefully. This will work.

---

### **The "Context Priming" Method: A Step-by-Step Guide**

#### **Step 1: The Insurance Policy (Save Key Artifacts NOW)**

Before you do anything else, you must save the most important, consolidated outputs from our current chat. Open a plain text editor (like Notepad, VS Code, or TextEdit) or a Google Doc and **copy and paste the following final documents** from our conversation above. These are your crown jewels.

1.  **The Complete Database Schema Script:** (The one starting `BEGIN; CREATE TYPE listed_unit_enum...`)
2.  **The Taxonomy Seed Data Script:** (The one starting `BEGIN; TRUNCATE TABLE categories...`)
3.  **The Complete Phased AI Development Brief:** (The one that lists Phase 1, 2, 3, and 4).
4.  **The Complete RLS Policy Script:** (The one we just finished).

Once you have these four items saved in a separate document on your computer, you are safe.

#### **Step 2: Create Your "Master Priming Prompt"**

Now, we will construct a single, powerful prompt that will instantly bring the AI in a new session up to speed. Copy the entire template below into your text editor.

**(Template Starts Here)**
```
You are an expert full-stack developer and project architect. We are building a new application called "FoodieFinds." I am going to provide you with the complete project plan, database schema, and seed data we have already developed. Your task is to process all of this information, understand it completely, and then help me continue building the application phase by phase. Do not generate any code until I ask you to.

Here is the complete project context:

==================================================
## Project Summary & Plan
==================================================

**Project:** FoodieFinds - An Artisan Food Marketplace
**Role for AI:** You are an expert full-stack developer specializing in rapid, scalable MVP development. Your task is to provide the code, terminal commands, and clear explanations for each step in each phase.
**Core Technologies:** Next.js 14+ (App Router), TypeScript, Tailwind CSS, Supabase (Database, Auth, Storage), Stripe Connect, Vercel.

---

### **Phase 1: Foundation & Core Transactional MVP**

**Goal:** To build the absolute minimum required for a user to sign up, a seller to list a product, and a buyer to purchase it via local pickup. The database schema will be built to support all future phases from day one.

#### **Task 1.1: Project Setup**
Provide the `npx create-next-app` command to initialize a new project named `foodie-finds-mvp` with TypeScript, ESLint, Tailwind CSS, the `src/` directory, and the App Router.

#### **Task 1.2: Database Schema & Seed Data Setup**
**(This is the most critical step for future-proofing)**
Provide two separate SQL scripts:
1.  **Schema Script:** Generate the complete, final PostgreSQL schema. This script must create all tables (`profiles`, `addresses`, `stores`, `categories`, `tag_groups`, `tags`, `products`, `product_tags`, `orders`, `reviews`, `reactions`, `discounts`, `loyalty_transactions`, `conversations`, `messages`) and all custom `ENUM` types (`listed_unit_enum`, `reaction_type`). The tables must include the global-ready columns for currency and units (e.g., storing money as integers, `listed_unit`, `price_per_gram`).
2.  **Seed Script:** Generate the script to populate the `categories`, `tag_groups`, and `tags` tables with the comprehensive, curated taxonomy we developed (v2.2).

#### **Task 1.3: User Authentication**
Generate the code for a complete authentication flow using Supabase Auth.
*   **Sign-Up Page (`/signup`):** A form with Full Name, Email, and Password. On submit, it must call `supabase.auth.signUp` and also create a new entry in the `profiles` table.
*   **Login Page (`/login`):** A form with Email and Password. Include a "Sign in with Google" option. On successful login, redirect to the homepage.
*   **Middleware (`middleware.ts`):** Set up the Supabase middleware to manage user sessions.

#### **Task 1.4: Basic Site Layout & Navigation**
Generate the code for the main application shell.
*   **Root Layout (`/layout.tsx`):** The main layout containing the Navbar and Footer.
*   **Navbar Component:** A responsive header that dynamically shows "Login/Sign Up" links for logged-out users, and a user profile dropdown (with a "Sign Out" button) for logged-in users.
*   **Footer Component:** A simple site footer.

#### **Task 1.5: Core Seller & Product Flow (MVP)**
Generate the code for the minimum seller functionality.
*   **"Become a Seller" Flow:** A simple form that updates the user's `profiles.is_seller` flag to `true` and creates a corresponding entry in the `stores` table.
*   **Product Management (Create/Read):** A form for sellers to create a new product. For the MVP, it should include Title, Description, Price (as `listed_price` and `listed_unit`), and an image upload (using Supabase Storage). The seller must select a Category.
*   **Public Store & Product Pages:** Create basic, read-only pages to display a seller's store and individual products.

#### **Task 1.6: Core Buyer & Checkout Flow (MVP)**
Generate the code for the "happy path" purchase flow.
*   **Product Discovery:** A simple homepage and search page to view all products.
*   **Shopping Cart:** Implement client-side state management for a shopping cart.
*   **Checkout:** A checkout page that integrates **Stripe Connect** for payment processing. For the MVP, this flow will only support the **"Local Pickup"** fulfillment method. The total price is calculated, payment is taken, and an entry is created in the `orders` table.
*   **Order Confirmation:** A simple confirmation page shown after a successful purchase.

---

### **Phase 2: Seller Empowerment & Community Engagement**

**Goal:** To build features that make the platform sticky and valuable for both sellers and buyers, moving beyond a simple transaction engine.

#### **Task 2.1: Seller Dashboard & Order Management**
Generate a private `/dashboard/orders` page for sellers.
*   **Order List:** Display a list of all incoming orders with their status (`Pending`, `Completed`, etc.).
*   **Order Status Updates:** Allow sellers to update an order's status (e.g., "Mark as Ready for Pickup"). This action should trigger a notification to the buyer.

#### **Task 2.2: Rich Product & Store Management**
Enhance the seller experience.
*   **Full Product CRUD:** Implement the "Update" and "Delete" functionality for products.
*   **AI Description Writer:** On the "Add/Edit Product" page, add an "AI Assistant" button. This should call a secure Next.js API route that proxies a request to an AI model (like GPT-4) to generate a product description based on the title and keywords.
*   **Product Videos & Tagging:** Add the ability for sellers to include a video URL and select multiple tags (from the pre-defined list) for their products.
*   **Store Customization:** Allow sellers to edit their store name, logo, banner image, and bio from their dashboard.

#### **Task 2.3: Rich Reviews & Reactions**
Build out the social proof and engagement features.
*   **Review Submission Flow:** After an order is marked "Completed," prompt the buyer to leave a review.
*   **Review Component:** The review form should allow a star rating, text, and (as a new feature) image/video uploads.
*   **Display Reviews:** Show all reviews beautifully on the product page.
*   **Foodie Reactions:** On product pages, implement the UI for users to add/remove pre-defined reactions (e.g., 'yummy', 'spicy') from the `reaction_type` ENUM.

---

### **Phase 3: Growth, Monetization & Advanced Fulfillment**

**Goal:** To introduce features that drive revenue, increase customer retention, and provide sellers with more powerful tools to manage their business.

#### **Task 3.1: Discounts & Loyalty System**
Build monetization and retention tools.
*   **Discount Engine:** In the seller dashboard, create a UI for sellers to create and manage discount codes (e.g., `10%OFF`, `$5OFF`) stored in the `discounts` table. The checkout flow must be updated to allow buyers to apply these codes.
*   **Loyalty Program:** Implement the backend logic for the loyalty system. When an order is completed, award points to the buyer's `profiles.loyalty_points` and record it in `loyalty_transactions`. Add a feature to the checkout to allow users to redeem points for a discount.

#### **Task 3.2: Advanced Fulfillment Options**
Expand fulfillment beyond local pickup.
*   **Seller-Managed Local Delivery:** Create a UI for sellers to define delivery zones (e.g., by postal code or radius) and set a delivery fee. The checkout must be updated to offer this option if the buyer's address is within a zone.
*   **Seller-Managed Shipping:** Allow sellers to add a flat-rate shipping fee for products marked as `is_shippable`.
*   **(Optional/Advanced) Real-Time Shipping Rates:** Integrate with an API like EasyPost or Shippo to calculate shipping costs dynamically at checkout based on package weight/dimensions.

#### **Task 3.3: Automated Sales Tax**
Implement a scalable solution for taxes.
*   **Integration:** Integrate a service like **Stripe Tax**.
*   **Checkout Update:** Modify the checkout process to call the Stripe Tax API to automatically calculate the correct sales tax based on the seller's and buyer's addresses. The calculated tax should be stored in the `orders.taxes_amount` column.

---

### **Phase 4: Scale, Operations & Analytics**

**Goal:** To build the internal tools and data infrastructure needed to operate the platform efficiently and make data-driven decisions.

#### **Task 4.1: Internal Admin Panel**
Build a secure, internal tool for platform management.
*   **Recommendation:** Use a low-code platform like **Retool** or **Appsmith** connected to the Supabase database via the `service_role` key.
*   **Core Features:**
    *   **User/Store Lookup:** Ability to find and view any user or store.
    *   **Order Management:** View and, if necessary, manually update any order.
    *   **Content Moderation:** A queue to review and act on flagged products or reviews.
    *   **User Impersonation:** A secure feature for support staff to "view as user" to troubleshoot issues.

#### **Task 4.2: Advanced Analytics**
Implement a robust analytics pipeline.
*   **Event Tracking:** Integrate a tool like **PostHog** or Mixpanel. Implement detailed event tracking for key user actions (`Product Viewed`, `Added to Cart`, `Started Checkout`, `Review Submitted`, etc.).
*   **Platform Dashboard (Internal):** Build dashboards (using PostHog or a BI tool) to track core platform KPIs like GMV, user growth, and conversion funnels.
*   **Seller Analytics Dashboard (External):** Enhance the seller dashboard to show them their specific stats: page views, conversion rates, top-performing products, etc., by querying an aggregated view of the analytics data.

==================================================
## Final Database Schema
==================================================

This script defines the entire structure of your FoodieFinds platform. It includes all tables for users, stores, products (with international-ready units/currency), ordering, messaging, reviews, and categorization. It is designed to be run once on a clean database.

```sql
-- =================================================================
-- FoodieFinds Complete Database Schema v1.0
-- For PostgreSQL / Supabase
--
-- This script creates all tables, custom types, and relationships
-- required for the platform, incorporating all features discussed.
-- =================================================================

BEGIN;

-- ---------------------------------
-- CUSTOM TYPES (ENUMS)
-- ---------------------------------

-- Defines how a product's price is listed for sale.
CREATE TYPE listed_unit_enum AS ENUM (
    'unit',       -- A single item (a cake, a jar)
    'bunch',      -- A bunch of carrots, kale, etc.
    'lb',         -- Imperial: Pound
    'oz',         -- Imperial: Ounce
    'kg',         -- Metric: Kilogram
    'g'           -- Metric: Gram
);

-- Defines the types of reactions a user can give.
CREATE TYPE reaction_type AS ENUM (
    'yummy',
    'sweet',
    'spicy',
    'love_it'
);

-- ---------------------------------
-- TABLE CREATION
-- ---------------------------------

-- Stores public user data, supplementing the built-in auth.users table.
CREATE TABLE profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    full_name TEXT,
    avatar_url TEXT,
    is_seller BOOLEAN NOT NULL DEFAULT false,
    loyalty_points INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Stores user addresses for shipping and billing.
CREATE TABLE addresses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
    line1 TEXT NOT NULL,
    line2 TEXT,
    city TEXT NOT NULL,
    state_province_region TEXT,
    postal_code TEXT,
    country CHAR(2) NOT NULL, -- ISO 3166-1 alpha-2 country code
    is_default_shipping BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Stores seller-specific information.
CREATE TABLE stores (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID UNIQUE NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
    store_name TEXT UNIQUE NOT NULL,
    store_handle TEXT UNIQUE NOT NULL,
    logo_url TEXT,
    banner_url TEXT,
    bio TEXT,
    allows_pickup BOOLEAN NOT NULL DEFAULT true,
    allows_local_delivery BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Categories for primary navigation (hierarchical).
CREATE TABLE categories (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    slug TEXT UNIQUE NOT NULL,
    parent_id UUID REFERENCES categories(id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Groups for tags (e.g., 'Dietary', 'Cuisine').
CREATE TABLE tag_groups (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT UNIQUE NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Individual tags/attributes for filtering.
CREATE TABLE tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    group_id UUID NOT NULL REFERENCES tag_groups(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    slug TEXT UNIQUE NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- The products offered by sellers.
CREATE TABLE products (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    category_id UUID REFERENCES categories(id),
    title TEXT NOT NULL,
    description TEXT,
    
    -- Pricing & Unit Information (Global-Ready)
    listed_price INTEGER NOT NULL, -- Price in the currency's smallest unit (e.g., cents)
    listed_unit listed_unit_enum NOT NULL,
    currency CHAR(3) NOT NULL DEFAULT 'USD', -- ISO 4217 currency code
    price_per_gram NUMERIC(20, 10), -- Calculated price per gram for weight-based items
    
    -- Product Details
    is_shippable BOOLEAN NOT NULL DEFAULT false,
    seasonality TEXT,
    image_urls TEXT[],
    video_url TEXT,
    availability TEXT NOT NULL DEFAULT 'In Stock' CHECK (availability IN ('In Stock', 'Made to Order', 'Out of Stock')),
    
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Join table for the many-to-many relationship between products and tags.
CREATE TABLE product_tags (
    product_id UUID NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    tag_id UUID NOT NULL REFERENCES tags(id) ON DELETE CASCADE,
    PRIMARY KEY (product_id, tag_id)
);

-- Records customer orders.
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    buyer_id UUID NOT NULL REFERENCES profiles(id),
    store_id UUID NOT NULL REFERENCES stores(id),
    
    -- Financials (stored in smallest currency unit, e.g., cents)
    subtotal INTEGER NOT NULL,
    discount_applied INTEGER NOT NULL DEFAULT 0,
    taxes_amount INTEGER NOT NULL DEFAULT 0,
    shipping_fee INTEGER NOT NULL DEFAULT 0,
    total_price INTEGER NOT NULL,
    currency CHAR(3) NOT NULL,
    
    -- Fulfillment
    shipping_address_id UUID REFERENCES addresses(id),
    fulfillment_method TEXT NOT NULL, -- e.g., 'pickup', 'delivery', 'shipping'
    
    status TEXT NOT NULL DEFAULT 'Pending' CHECK (status IN ('Pending', 'In Progress', 'Shipped', 'Completed', 'Cancelled')),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Stores customer reviews for products.
CREATE TABLE reviews (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID UNIQUE NOT NULL REFERENCES orders(id),
    user_id UUID NOT NULL REFERENCES profiles(id),
    product_id UUID NOT NULL REFERENCES products(id),
    rating INT NOT NULL CHECK (rating >= 1 AND rating <= 5),
    review_text TEXT,
    image_urls TEXT[],
    video_url TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Stores user reactions to products.
CREATE TABLE reactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES profiles(id),
    product_id UUID NOT NULL REFERENCES products(id),
    reaction reaction_type NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(user_id, product_id, reaction)
);

-- Stores promotional discount codes created by sellers.
CREATE TABLE discounts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    code TEXT NOT NULL,
    type TEXT NOT NULL CHECK (type IN ('percentage', 'fixed_amount')),
    value NUMERIC(10, 2) NOT NULL,
    is_active BOOLEAN NOT NULL DEFAULT true,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(store_id, code)
);

-- Tracks changes to a user's loyalty points.
CREATE TABLE loyalty_transactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES profiles(id),
    order_id UUID REFERENCES orders(id),
    points_change INT NOT NULL,
    reason TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Groups messages between two parties.
CREATE TABLE conversations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID REFERENCES orders(id),
    buyer_id UUID NOT NULL REFERENCES profiles(id),
    seller_id UUID NOT NULL REFERENCES profiles(id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Stores individual chat messages.
CREATE TABLE messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    conversation_id UUID NOT NULL REFERENCES conversations(id) ON DELETE CASCADE,
    sender_id UUID NOT NULL REFERENCES profiles(id),
    content TEXT NOT NULL,
    read_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- ---------------------------------
-- INDEXES FOR PERFORMANCE
-- ---------------------------------
CREATE INDEX idx_products_store_id ON products(store_id);
CREATE INDEX idx_products_category_id ON products(category_id);
CREATE INDEX idx_orders_buyer_id ON orders(buyer_id);
CREATE INDEX idx_orders_store_id ON orders(store_id);
CREATE INDEX idx_reviews_product_id ON reviews(product_id);
CREATE INDEX idx_tags_group_id ON tags(group_id);

COMMIT;
```


==================================================
## Final Taxonomy Seed Data
==================================================

This script populates your `categories`, `tag_groups`, and `tags` tables with the complete, curated taxonomy we developed. **Run this script *after* running the schema script above.**

```sql
-- =================================================================
-- FoodieFinds Seed Script v2.2
-- Populates categories, tag_groups, and tags.
-- Run this AFTER the schema creation script.
-- =================================================================

BEGIN;

-- Clear existing data to prevent duplication if script is run more than once.
TRUNCATE TABLE categories, tag_groups, tags RESTART IDENTITY CASCADE;

-- ---------------------------------
-- CATEGORIES
-- ---------------------------------
WITH parent_categories_insert AS (
    INSERT INTO categories (name, slug)
    VALUES
        ('Fresh Produce', 'fresh-produce'),
        ('Bakery & Sweets', 'bakery-sweets'),
        ('Pantry Staples', 'pantry-staples'),
        ('Meals & Prepared Foods', 'meals-prepared-foods'),
        ('Dairy, Cheese & Eggs', 'dairy-cheese-eggs'),
        ('Charcuterie & Deli', 'charcuterie-deli'),
        ('Snacks', 'snacks'),
        ('Beverages', 'beverages'),
        ('Frozen Goods', 'frozen-goods')
    RETURNING id, name
)
INSERT INTO categories (name, slug, parent_id)
SELECT
    sub.name,
    sub.slug,
    parent.id
FROM parent_categories_insert AS parent
JOIN (
    VALUES
        ('Fresh Produce', 'Fruits', 'fruits'),
        ('Fresh Produce', 'Vegetables', 'vegetables'),
        ('Fresh Produce', 'Fresh Herbs', 'fresh-herbs'),
        ('Bakery & Sweets', 'Artisan Breads & Loaves', 'artisan-breads'),
        ('Bakery & Sweets', 'Cakes, Cupcakes & Muffins', 'cakes-cupcakes-muffins'),
        ('Bakery & Sweets', 'Cookies, Brownies & Biscuits', 'cookies-brownies-biscuits'),
        ('Bakery & Sweets', 'Pastries & Croissants', 'pastries-croissants'),
        ('Bakery & Sweets', 'Pies & Tarts', 'pies-tarts'),
        ('Bakery & Sweets', 'Chocolates & Truffles', 'chocolates-truffles'),
        ('Bakery & Sweets', 'Candies, Toffees & Caramels', 'candies-toffees-caramels'),
        ('Bakery & Sweets', 'Fudge & Nougat', 'fudge-nougat'),
        ('Pantry Staples', 'Jams, Jellies & Preserves', 'jams-jellies-preserves'),
        ('Pantry Staples', 'Sauces, Dressings & Marinades', 'sauces-dressings-marinades'),
        ('Pantry Staples', 'Hot Sauces', 'hot-sauces'),
        ('Pantry Staples', 'Spices, Rubs & Seasonings', 'spices-rubs-seasonings'),
        ('Pantry Staples', 'Oils & Vinegars', 'oils-vinegars'),
        ('Pantry Staples', 'Honey, Syrups & Spreads', 'honey-syrups-spreads'),
        ('Pantry Staples', 'Pickled & Fermented Goods', 'pickled-fermented-goods'),
        ('Meals & Prepared Foods', 'Fresh Pasta & Noodles', 'fresh-pasta-noodles'),
        ('Meals & Prepared Foods', 'Soups, Stews & Curries', 'soups-stews-curries'),
        ('Meals & Prepared Foods', 'DIY Meal Kits', 'diy-meal-kits'),
        ('Meals & Prepared Foods', 'Ready-to-Eat Meals', 'ready-to-eat-meals'),
        ('Dairy, Cheese & Eggs', 'Artisan Cheese', 'artisan-cheese'),
        ('Dairy, Cheese & Eggs', 'Butter & Cultured Dairy', 'butter-cultured-dairy'),
        ('Dairy, Cheese & Eggs', 'Artisan Yogurts', 'artisan-yogurts'),
        ('Dairy, Cheese & Eggs', 'Specialty Eggs', 'specialty-eggs'),
        ('Charcuterie & Deli', 'Cured Meats & Sausages', 'cured-meats-sausages'),
        ('Charcuterie & Deli', 'Pâtés & Terrines', 'pates-terrines'),
        ('Charcuterie & Deli', 'Smoked Fish & Seafood', 'smoked-fish-seafood'),
        ('Snacks', 'Granola & Cereals', 'granola-cereals'),
        ('Snacks', 'Chips & Crackers', 'chips-crackers'),
        ('Snacks', 'Nuts, Seeds & Trail Mixes', 'nuts-seeds-trail-mixes'),
        ('Snacks', 'Jerky & Meat Snacks', 'jerky-meat-snacks'),
        ('Snacks', 'Popcorn', 'popcorn'),
        ('Beverages', 'Coffee & Espresso Beans', 'coffee-espresso-beans'),
        ('Beverages', 'Teas & Herbal Infusions', 'teas-herbal-infusions'),
        ('Beverages', 'Hot Chocolate & Mixes', 'hot-chocolate-mixes'),
        ('Beverages', 'Drink Syrups, Cordials & Mixers', 'drink-syrups-cordials-mixers'),
        ('Frozen Goods', 'Frozen Desserts & Ice Cream', 'frozen-desserts-ice-cream'),
        ('Frozen Goods', 'Frozen Meals & Dumplings', 'frozen-meals-dumplings'),
        ('Frozen Goods', 'Frozen Pastries & Dough', 'frozen-pastries-dough')
) AS sub(parent_name, name, slug) ON parent.name = sub.parent_name;

-- ---------------------------------
-- TAGS
-- ---------------------------------
WITH tag_groups_insert AS (
    INSERT INTO tag_groups (name)
    VALUES
        ('Dietary'),
        ('Allergens'),
        ('Farming & Production'),
        ('Flavor Profile'),
        ('Occasion / Meal Type'),
        ('Preparation & Sourcing'),
        ('Cuisine'),
        ('Texture'),
        ('Storage & Shelf Life')
    RETURNING id, name
)
INSERT INTO tags (name, slug, group_id)
SELECT
    tag.name,
    tag.slug,
    tg.id
FROM tag_groups_insert AS tg
JOIN (
    VALUES
        ('Dietary', 'Vegan', 'vegan'),
        ('Dietary', 'Vegetarian', 'vegetarian'),
        ('Dietary', 'Gluten-Free', 'gluten-free'),
        ('Dietary', 'Dairy-Free', 'dairy-free'),
        ('Dietary', 'Keto-Friendly', 'keto-friendly'),
        ('Dietary', 'Paleo', 'paleo'),
        ('Dietary', 'Low-Carb', 'low-carb'),
        ('Dietary', 'Low-Sugar', 'low-sugar'),
        ('Dietary', 'High-Protein', 'high-protein'),
        ('Dietary', 'High-Fiber', 'high-fiber'),
        ('Dietary', 'Halal', 'halal'),
        ('Dietary', 'Kosher', 'kosher'),
        ('Allergens', 'Contains Nuts', 'contains-nuts'),
        ('Allergens', 'Nut-Free', 'nut-free'),
        ('Allergens', 'Contains Soy', 'contains-soy'),
        ('Allergens', 'Soy-Free', 'soy-free'),
        ('Allergens', 'Contains Eggs', 'contains-eggs'),
        ('Allergens', 'Egg-Free', 'egg-free'),
        ('Allergens', 'Contains Shellfish', 'contains-shellfish'),
        ('Farming & Production', 'Certified Organic', 'certified-organic'),
        ('Farming & Production', 'Pesticide-Free', 'pesticide-free'),
        ('Farming & Production', 'Non-GMO', 'non-gmo'),
        ('Farming & Production', 'Heirloom Variety', 'heirloom-variety'),
        ('Farming & Production', 'Pasture-Raised', 'pasture-raised'),
        ('Flavor Profile', 'Spicy', 'spicy'),
        ('Flavor Profile', 'Sweet', 'sweet'),
        ('Flavor Profile', 'Salty', 'salty'),
        ('Flavor Profile', 'Savory', 'savory'),
        ('Flavor Profile', 'Umami', 'umami'),
        ('Flavor Profile', 'Sour', 'sour'),
        ('Flavor Profile', 'Bitter', 'bitter'),
        ('Flavor Profile', 'Smoky', 'smoky'),
        ('Flavor Profile', 'Rich', 'rich'),
        ('Flavor Profile', 'Light', 'light'),
        ('Occasion / Meal Type', 'Breakfast', 'breakfast'),
        ('Occasion / Meal Type', 'Lunch', 'lunch'),
        ('Occasion / Meal Type', 'Dinner', 'dinner'),
        ('Occasion / Meal Type', 'Snack', 'snack'),
        ('Occasion / Meal Type', 'Dessert', 'dessert'),
        ('Occasion / Meal Type', 'Birthday', 'birthday'),
        ('Occasion / Meal Type', 'Holiday', 'holiday'),
        ('Occasion / Meal Type', 'Anniversary', 'anniversary'),
        ('Occasion / Meal Type', 'Thank You Gift', 'thank-you-gift'),
        ('Occasion / Meal Type', 'Office Party', 'office-party'),
        ('Occasion / Meal Type', 'Game Day', 'game-day'),
        ('Occasion / Meal Type', 'Wedding', 'wedding'),
        ('Preparation & Sourcing', 'Small Batch', 'small-batch'),
        ('Preparation & Sourcing', 'Handmade', 'handmade'),
        ('Preparation & Sourcing', 'Locally Sourced', 'locally-sourced'),
        ('Preparation & Sourcing', 'Family Recipe', 'family-recipe'),
        ('Preparation & Sourcing', 'Aged', 'aged'),
        ('Preparation & Sourcing', 'Fermented', 'fermented'),
        ('Preparation & Sourcing', 'Smoked', 'smoked'),
        ('Preparation & Sourcing', 'Raw', 'raw'),
        ('Preparation & Sourcing', 'Grilled', 'grilled'),
        ('Preparation & Sourcing', 'Baked', 'baked'),
        ('Preparation & Sourcing', 'Fair Trade', 'fair-trade'),
        ('Cuisine', 'Japanese', 'japanese'),
        ('Cuisine', 'Chinese', 'chinese'),
        ('Cuisine', 'Korean', 'korean'),
        ('Cuisine', 'Thai', 'thai'),
        ('Cuisine', 'Vietnamese', 'vietnamese'),
        ('Cuisine', 'Malaysian', 'malaysian'),
        ('Cuisine', 'Indonesian', 'indonesian'),
        ('Cuisine', 'Filipino', 'filipino'),
        ('Cuisine', 'Indian', 'indian'),
        ('Cuisine', 'Pakistani', 'pakistani'),
        ('Cuisine', 'Sri Lankan', 'sri-lankan'),
        ('Cuisine', 'Bangladeshi', 'bangladeshi'),
        ('Cuisine', 'Nepali', 'nepali'),
        ('Cuisine', 'Italian', 'italian'),
        ('Cuisine', 'French', 'french'),
        ('Cuisine', 'Spanish', 'spanish'),
        ('Cuisine', 'Greek', 'greek'),
        ('Cuisine', 'German', 'german'),
        ('Cuisine', 'British', 'british'),
        ('Cuisine', 'Eastern European', 'eastern-european'),
        ('Cuisine', 'American (USA)', 'american-usa'),
        ('Cuisine', 'Mexican', 'mexican'),
        ('Cuisine', 'Caribbean', 'caribbean'),
        ('Cuisine', 'Peruvian', 'peruvian'),
        ('Cuisine', 'Brazilian', 'brazilian'),
        ('Cuisine', 'Cajun & Creole', 'cajun-creole'),
        ('Cuisine', 'Middle Eastern', 'middle-eastern'),
        ('Cuisine', 'Lebanese', 'lebanese'),
        ('Cuisine', 'Turkish', 'turkish'),
        ('Cuisine', 'Moroccan', 'moroccan'),
        ('Cuisine', 'Persian', 'persian'),
        ('Cuisine', 'North African', 'north-african'),
        ('Cuisine', 'West African', 'west-african'),
        ('Cuisine', 'Ethiopian', 'ethiopian'),
        ('Texture', 'Crunchy', 'crunchy'),
        ('Texture', 'Soft', 'soft'),
        ('Texture', 'Creamy', 'creamy'),
        ('Texture', 'Chewy', 'chewy'),
        ('Texture', 'Crispy', 'crispy'),
        ('Texture', 'Fluffy', 'fluffy'),
        ('Storage & Shelf Life', 'Shelf-Stable', 'shelf-stable'),
        ('Storage & Shelf Life', 'Refrigerate After Opening', 'refrigerate-after-opening'),
        ('Storage & Shelf Life', 'Freezer-Friendly', 'freezer-friendly'),
        ('Storage & Shelf Life', 'Fresh (Consume Promptly)', 'fresh-consume-promptly'),
        ('Storage & Shelf Life', 'Dried', 'dried')
) AS tag(group_name, name, slug) ON tg.name = tag.group_name;

COMMIT;
```

==================================================
## Final Row Level Security (RLS) Policies
==================================================

```sql
-- =================================================================
-- FoodieFinds Complete RLS Policy Script v1.0
--
-- This script enables RLS and defines access policies for all
-- user-facing tables in the schema.
-- =================================================================

BEGIN;

-- ---------------------------------
-- PROFILES & ADDRESSES
-- ---------------------------------

-- PROFILES
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Public profiles are viewable by everyone."
    ON public.profiles FOR SELECT USING (true);

CREATE POLICY "Users can insert their own profile."
    ON public.profiles FOR INSERT WITH CHECK (auth.uid() = id);

CREATE POLICY "Users can update their own profile."
    ON public.profiles FOR UPDATE USING (auth.uid() = id);

-- ADDRESSES (Private Data)
ALTER TABLE public.addresses ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view their own addresses."
    ON public.addresses FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own addresses."
    ON public.addresses FOR INSERT WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update their own addresses."
    ON public.addresses FOR UPDATE USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own addresses."
    ON public.addresses FOR DELETE USING (auth.uid() = user_id);


-- ---------------------------------
-- STORES, PRODUCTS, & CATEGORIZATION
-- ---------------------------------

-- STORES
ALTER TABLE public.stores ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Stores are viewable by everyone."
    ON public.stores FOR SELECT USING (true);

CREATE POLICY "Users can create their own store."
    ON public.stores FOR INSERT WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Store owners can update their own store."
    ON public.stores FOR UPDATE USING (auth.uid() = user_id);

CREATE POLICY "Store owners can delete their own store."
    ON public.stores FOR DELETE USING (auth.uid() = user_id);

-- PRODUCTS
ALTER TABLE public.products ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Products are viewable by everyone."
    ON public.products FOR SELECT USING (true);

-- Helper function to check store ownership
CREATE OR REPLACE FUNCTION is_store_owner(store_id_to_check UUID)
RETURNS BOOLEAN AS $$
  SELECT EXISTS (
    SELECT 1 FROM stores WHERE id = store_id_to_check AND user_id = auth.uid()
  );
$$ LANGUAGE sql SECURITY DEFINER;

CREATE POLICY "Store owners can create products for their store."
    ON public.products FOR INSERT WITH CHECK (is_store_owner(store_id));

CREATE POLICY "Store owners can update products in their store."
    ON public.products FOR UPDATE USING (is_store_owner(store_id));

CREATE POLICY "Store owners can delete products from their store."
    ON public.products FOR DELETE USING (is_store_owner(store_id));

-- PRODUCT_TAGS (Join Table)
ALTER TABLE public.product_tags ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Product tags are viewable by everyone."
    ON public.product_tags FOR SELECT USING (true);

-- Helper function to check product ownership via store
CREATE OR REPLACE FUNCTION is_product_owner(product_id_to_check UUID)
RETURNS BOOLEAN AS $$
  SELECT is_store_owner(store_id) FROM products WHERE id = product_id_to_check;
$$ LANGUAGE sql SECURITY DEFINER;

CREATE POLICY "Store owners can add tags to their products."
    ON public.product_tags FOR INSERT WITH CHECK (is_product_owner(product_id));

CREATE POLICY "Store owners can remove tags from their products."
    ON public.product_tags FOR DELETE USING (is_product_owner(product_id));

-- CATEGORIES, TAG_GROUPS, TAGS (Platform-Managed Data)
-- These are managed by admins, so we only allow read access for everyone.
-- No INSERT, UPDATE, or DELETE policies are created, blocking these actions for all users.
ALTER TABLE public.categories ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Categories are viewable by everyone." ON public.categories FOR SELECT USING (true);

ALTER TABLE public.tag_groups ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Tag groups are viewable by everyone." ON public.tag_groups FOR SELECT USING (true);

ALTER TABLE public.tags ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Tags are viewable by everyone." ON public.tags FOR SELECT USING (true);


-- ---------------------------------
-- ORDERS, REVIEWS, & REACTIONS
-- ---------------------------------

-- ORDERS (Sensitive Data)
ALTER TABLE public.orders ENABLE ROW LEVEL SECURITY;
-- NOTE: We intentionally do NOT create a DELETE policy for orders to maintain historical data.

CREATE POLICY "Buyer and Seller can view their own orders."
    ON public.orders FOR SELECT USING (auth.uid() = buyer_id OR is_store_owner(store_id));

CREATE POLICY "Users can only create orders for themselves."
    ON public.orders FOR INSERT WITH CHECK (auth.uid() = buyer_id);

CREATE POLICY "Sellers can update the status of their orders."
    ON public.orders FOR UPDATE USING (is_store_owner(store_id));

-- REVIEWS
ALTER TABLE public.reviews ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Reviews are viewable by everyone."
    ON public.reviews FOR SELECT USING (true);

-- Helper function to check if user purchased the order related to a review
CREATE OR REPLACE FUNCTION did_user_purchase(order_id_to_check UUID)
RETURNS BOOLEAN AS $$
  SELECT EXISTS (
    SELECT 1 FROM orders WHERE id = order_id_to_check AND buyer_id = auth.uid()
  );
$$ LANGUAGE sql SECURITY DEFINER;

CREATE POLICY "Users can only create reviews for orders they purchased."
    ON public.reviews FOR INSERT WITH CHECK (did_user_purchase(order_id));

CREATE POLICY "Users can update their own reviews."
    ON public.reviews FOR UPDATE USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own reviews."
    ON public.reviews FOR DELETE USING (auth.uid() = user_id);

-- REACTIONS
ALTER TABLE public.reactions ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Reactions are viewable by everyone."
    ON public.reactions FOR SELECT USING (true);

CREATE POLICY "Users can create their own reactions."
    ON public.reactions FOR INSERT WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can delete their own reactions."
    ON public.reactions FOR DELETE USING (auth.uid() = user_id);


-- ---------------------------------
-- MESSAGING (Conversations & Messages)
-- ---------------------------------

-- CONVERSATIONS
ALTER TABLE public.conversations ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Participants can view their conversations."
    ON public.conversations FOR SELECT USING (auth.uid() = buyer_id OR auth.uid() = seller_id);

CREATE POLICY "Buyers can initiate conversations."
    ON public.conversations FOR INSERT WITH CHECK (auth.uid() = buyer_id);

CREATE POLICY "Participants can update their conversation (e.g., updated_at)."
    ON public.conversations FOR UPDATE USING (auth.uid() = buyer_id OR auth.uid() = seller_id);

-- MESSAGES
ALTER TABLE public.messages ENABLE ROW LEVEL SECURITY;
-- NOTE: We intentionally do NOT create UPDATE or DELETE policies for messages to preserve chat history.

-- Helper function to check if user is a participant in a conversation
CREATE OR REPLACE FUNCTION is_conversation_participant(conversation_id_to_check UUID)
RETURNS BOOLEAN AS $$
  SELECT EXISTS (
    SELECT 1 FROM conversations WHERE id = conversation_id_to_check AND (buyer_id = auth.uid() OR seller_id = auth.uid())
  );
$$ LANGUAGE sql SECURITY DEFINER;

CREATE POLICY "Participants can view messages in their conversations."
    ON public.messages FOR SELECT USING (is_conversation_participant(conversation_id));

CREATE POLICY "Participants can send messages in their conversations."
    ON public.messages FOR INSERT WITH CHECK (is_conversation_participant(conversation_id) AND auth.uid() = sender_id);


-- ---------------------------------
-- DISCOUNTS & LOYALTY (Seller/User-Specific)
-- ---------------------------------
-- DISCOUNTS
ALTER TABLE public.discounts ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Store owners can manage their own discounts."
    ON public.discounts FOR ALL USING (is_store_owner(store_id));

-- LOYALTY_TRANSACTIONS
ALTER TABLE public.loyalty_transactions ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view their own loyalty transactions."
    ON public.loyalty_transactions FOR SELECT USING (auth.uid() = user_id);
-- NOTE: Loyalty transactions should only be created by the system via triggers or secure functions, so no INSERT policy is provided for users.

COMMIT;
```


==================================================

You now have the complete context for the FoodieFinds application. Please confirm that you have processed and understood all the provided information, including the phased plan, the full database schema, the seed data, and the RLS policies.

Once you have confirmed, I will give you the next task.
```
**(Template Ends Here)**

#### **Step 3: Execute the Plan**

1.  **Fill in the Blanks:** Go to your text document where you saved the four artifacts and paste each one into the corresponding `[--- ... ---]` section in the Master Priming Prompt template.
2.  **Open a New Chat Session:** Start a fresh, new conversation.
3.  **Paste and Send:** Paste the entire, completed Master Priming Prompt as your very first message in the new session.
4.  **Wait for Confirmation:** The AI should respond with something like, "I have processed and understood all the provided information... I am ready for the next task."

#### **Step 4: Continue Where We Left Off**

Once you have the confirmation, you can seamlessly continue. Your new AI session is now just as knowledgeable and "polished" as this one.

A great follow-up prompt would be:

> "Excellent. You have the full context. Let's begin building Phase 1. Please execute **Task 1.1: Project Setup** from the development brief you have."

---

### **Why This Method Works**

An AI's performance is entirely dependent on the quality of its context window. By starting a new session with this massive, well-structured "briefing document," you are force-loading its context with all the refined decisions, schemas, and plans we've made. It's the equivalent of giving a new team member a comprehensive project binder on their first day, rather than just telling them their first task.

Take your time to prepare the Master Priming Prompt. This will ensure our work continues at the same high level of quality.