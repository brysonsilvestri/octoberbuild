# StudioSix - Product Photo Studio

AI-powered product photography tool with white background removal and studio lighting.

## Features

- 🎨 AI-powered white background removal
- ✨ Professional studio lighting
- 🆓 Anonymous users get 1 free photo generation (IP-tracked)
- 📊 Credit-based subscription system
- 💳 Stripe integration for payments
- 📱 Mobile-responsive design

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/brysonsilvestri/octoberbuild.git
cd octoberbuild
```

### 2. Create Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

**Note:** If you get an error about `google-genai`, make sure you're using the updated `requirements.txt` which includes `google-generativeai` (the correct package name).

### 4. Set Up Environment Variables

Create a `.env` file in the project root:

```bash
# Required
GOOGLE_API_KEY=your_google_api_key_here
SECRET_KEY=your_secret_key_here

# Stripe (for payments)
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_PRICE_ID_STARTER=price_xxxxx
STRIPE_PRICE_ID_CREATOR=price_xxxxx
STRIPE_PRICE_ID_ENTERPRISE=price_xxxxx
STRIPE_PRICE_ID_STARTER_ANNUAL=price_xxxxx
STRIPE_PRICE_ID_CREATOR_ANNUAL=price_xxxxx
STRIPE_PRICE_ID_ENTERPRISE_ANNUAL=price_xxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxx
STRIPE_CREATOR_COUPON=coupon_id_optional

# App Configuration
APP_BASE_URL=http://localhost:5000
```

**To get a Google API Key:**
1. Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Add it to your `.env` file

### 5. Initialize the Database

The database will be automatically created when you first run the app. The SQLite database will be created at `instance/users.db`.

### 6. Run the Application

```bash
python app.py
```

The app will be available at `http://localhost:5000`

## Project Structure

```
octoberbuild/
├── app.py                 # Main Flask application
├── requirements.txt       # Python dependencies
├── .env                   # Environment variables (create this)
├── .gitignore            # Git ignore rules
├── instance/             # SQLite database location
│   └── users.db          # Database (auto-created)
├── static/               # Static files
│   ├── uploads/          # User uploaded images (runtime)
│   └── outputs/          # Generated images (runtime)
└── templates/            # HTML templates
    ├── index.html        # Main dashboard
    ├── signup.html       # 3-step signup form
    ├── signup_tier.html  # Pricing page signup variant
    ├── login.html        # Login page
    ├── pricing.html      # Pricing page
    └── account.html      # Account dashboard
```

## User Flows

### Anonymous User Flow
1. Visit site → Upload image
2. Generate 1 free photo (IP tracked)
3. See result with signup prompt
4. Create account to continue

### Signup Flow (Default)
1. **Step 1:** Enter name, email, password
2. **Step 2:** Select plan (Starter/Creator) or continue with free account
3. **Step 3:** Account creation + redirect to Stripe (for paid plans)

### Signup Flow (From Pricing Page)
1. Click "Get Started" on any paid plan
2. See plan details on left, form on right
3. Enter name, email, password
4. Redirect to Stripe checkout with 7-day free trial

## Database Models

### User
- `id`, `name`, `email`, `password_hash`
- `plan_tier`: 'free', 'starter', 'creator', 'enterprise'
- `credits_remaining`, `credits_limit`
- `stripe_customer_id`, `is_subscribed`
- `generation_count`, `created_at`

### Generation
- User's image generation history
- Links to input/output image paths

### AnonymousGeneration
- IP-based tracking for non-authenticated users
- Prevents multiple free generations from same IP

## Troubleshooting

### `ModuleNotFoundError: No module named 'google.genai'`

**Solution:** The requirements.txt has been updated to use `google-generativeai` (the correct package name). Make sure you have the latest requirements.txt and run:

```bash
pip install --upgrade -r requirements.txt
```

### Missing `static` folder

**Solution:** The repository now includes `.gitkeep` files to preserve the folder structure. If you cloned an older version, create the folders manually:

```bash
mkdir -p static/uploads static/outputs
```

### Database errors on first run

**Solution:** Delete the existing database and let it recreate:

```bash
rm instance/users.db
python app.py  # Will auto-create fresh database
```

### Environment variables not loading

**Solution:** Make sure you have `python-dotenv` installed and your `.env` file is in the project root (same directory as `app.py`).

## Credits System

- **Free Plan:** 7,500 credits (15 images) per month
- **Starter Plan:** 60,000 credits (120 images) per month - $6/month
- **Creator Plan:** 200,000 credits (400 images) per month - $11/month
- **Enterprise Plan:** 800,000 credits (1,600 images) per month - $99/month

Each image generation costs **500 credits**.

## Tech Stack

- **Backend:** Flask, Flask-Login, Flask-SQLAlchemy
- **Database:** SQLite
- **AI:** Google Generative AI (Gemini)
- **Payments:** Stripe
- **Image Processing:** Pillow (PIL)
- **Frontend:** HTML, CSS (Bootstrap 5), Vanilla JavaScript

## License

Proprietary - All rights reserved
