# MoCoreX Frontend

Frontend application for MoCoreX - Next-Generation DeFi Platform.

## 🚀 Quick Start

```bash
# 1. Clone the repository
git clone https://bitbucket.org/tech-org/mocorex.git
cd mocorex

# 2. Install dependencies
npm install

# 3. Run the application (frontend)
npm run dev
```

App runs on: **http://localhost:8080**

---

## 📋 Table of Contents

- [For Product Managers](#for-product-managers)
- [For Frontend Engineers](#for-frontend-engineers)
- [For Data Engineers](#for-data-engineers)
- [For Marketing Specialists](#for-marketing-specialists)
- [For Backend Engineers](#for-backend-engineers)
- [For QA Engineers](#for-qa-engineers)
- [For DevOps Engineers](#for-devops-engineers)

---

## 👔 For Product Managers

### Product Overview

MoCoreX is a **Decentralized Perpetual Exchange** platform that enables users to trade perpetual futures contracts with leverage. The platform operates in **demo mode** by default, allowing users to explore features without wallet connection.

### Key Features

1. **Trading Interface**
   - Perpetual futures trading with leverage (up to 100x)
   - Market and limit orders
   - Real-time order book and recent trades
   - Multiple trading pairs (BTC/USDT, ETH/USDT, etc.)

2. **Markets Page**
   - Live market data with price changes
   - Top gainers/losers
   - Market search and filtering
   - Volume-based sorting

3. **Wallet Management**
   - Multi-asset balance tracking
   - Transaction history
   - Deposit/withdrawal (demo mode)
   - Profit/loss tracking

4. **Dashboard**
   - Portfolio overview
   - Trading statistics
   - Performance metrics

5. **Demo Mode**
   - No wallet connection required
   - Full feature access with mock data
   - Perfect for user onboarding and testing

### User Flows

#### New User Journey
1. Land on homepage → View features and stats
2. Click "Start Trading" → Redirected to `/login`
3. Click "Continue as Demo User" → Authenticated with demo token
4. Access protected routes: Dashboard, Trading, Wallet, Settings

#### Trading Flow
1. Navigate to `/trading`
2. Select trading pair (e.g., BTC/USDT)
3. Choose trade type (Long/Short)
4. Set leverage (1x-100x)
5. Enter amount and place order
6. View order in order book

### Demo Mode Behavior

- **Authentication**: Uses `demo-token` in localStorage
- **Data**: All API calls return mock data when `demo-token` is present
- **User Display**: Shows "Demo User" in header
- **Persistence**: Demo session persists across page refreshes
- **Logout**: Clears demo token and redirects to login

### Feature Priorities

**High Priority:**
- Trading execution
- Market data display
- Wallet balance tracking

**Medium Priority:**
- Advanced order types
- Chart integration
- Notification system

**Low Priority:**
- Social features
- Referral program
- Advanced analytics

---

## 💻 For Frontend Engineers

### Tech Stack

- **Framework**: React 18.3.1 with TypeScript
- **Build Tool**: Vite 5.4.19
- **Styling**: Tailwind CSS 3.4.17
- **UI Components**: Shadcn/ui (Radix UI primitives)
- **Routing**: React Router DOM 6.30.1
- **State Management**: React Context API
- **HTTP Client**: Axios 1.7.9
- **Data Fetching**: TanStack Query 5.83.0
- **Icons**: Lucide React 0.462.0

### Project Structure

```
src/
├── components/          # Reusable UI components
│   ├── ui/             # Shadcn/ui components (Button, Card, Input, etc.)
│   ├── Header.tsx      # Top navigation bar
│   ├── Footer.tsx      # Footer with links
│   ├── Hero.tsx        # Landing page hero section
│   ├── Navigation.tsx  # Navigation menu
│   └── ProtectedRoute.tsx  # Route protection wrapper
├── contexts/           # React Context providers
│   ├── AuthContext.tsx # Authentication state
│   └── WalletContext.tsx # Wallet state (stubbed for demo mode)
├── pages/              # Page components
│   ├── Index.tsx       # Landing page
│   ├── Login.tsx       # Login/demo authentication
│   ├── Trading.tsx     # Trading interface
│   ├── Markets.tsx     # Markets listing
│   ├── Dashboard.tsx   # User dashboard
│   ├── Wallet.tsx      # Wallet management
│   └── Settings.tsx    # User settings
├── services/           # API services
│   └── api.ts          # Axios instance and API endpoints
├── hooks/              # Custom React hooks
├── lib/                # Utility functions
└── main.tsx            # Application entry point
```

### Development Setup

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Lint code
npm run lint
```

### Environment Variables

Create a `.env` file in the root directory:

```env
VITE_API_URL=http://localhost:3000/api
```

### Key Components

#### Authentication Flow
- `AuthContext` manages demo user state
- `ProtectedRoute` wraps protected pages
- Demo mode uses `demo-token` in localStorage

#### API Integration
- All API calls in `src/services/api.ts`
- Demo mode automatically returns mock data
- Axios interceptors handle authentication tokens

#### Styling Guidelines
- Use Tailwind CSS utility classes
- Transparent backgrounds: `bg-transparent`
- Gradient text: `bg-gradient-to-r from-blue-400 via-cyan-400 to-purple-500 bg-clip-text text-transparent`
- Consistent spacing: `p-6`, `px-[50px]` for page-level padding
- Dark theme: `bg-slate-950`, `text-slate-400`

### Component Patterns

#### Page Layout
```tsx
<MainLayout>
  <div className="pl-[50px] pr-[50px]">
    {/* Page content */}
  </div>
</MainLayout>
```

#### Protected Routes
```tsx
<ProtectedRoute>
  <YourPage />
</ProtectedRoute>
```

#### API Calls with Demo Mode
```tsx
const { data } = await marketsAPI.getAllMarkets();
// Automatically returns mock data in demo mode
```

### Common Tasks

**Adding a new page:**
1. Create component in `src/pages/`
2. Add route in `src/App.tsx`
3. Add navigation link in `src/components/Header.tsx`

**Adding a new API endpoint:**
1. Add function to appropriate API object in `src/services/api.ts`
2. Implement `handleDemoMode` for mock data
3. Use in component with React Query

**Styling components:**
- Use transparent backgrounds with borders: `bg-transparent border border-slate-700/30`
- Soft colors: `bg-slate-700/30`, `text-slate-400`
- Hover effects: `hover:bg-slate-700/40`

---

## 📊 For Data Engineers

### API Structure

The application uses a centralized API service (`src/services/api.ts`) with automatic demo mode handling.

### API Base Configuration

```typescript
const API_BASE_URL = import.meta.env.VITE_API_URL || "http://localhost:3000/api";
```

### Data Models

#### Market Data
```typescript
interface Market {
  pair: string;              // "BTC/USDT"
  name: string;              // "BTC"
  price: number;             // Current price
  priceChange24h: number;    // 24h price change (absolute)
  priceChangePercent24h: number;  // 24h price change (%)
  high24h: number;
  low24h: number;
  volume24h: number;
  marketCap: number;
}
```

#### Trading Data
```typescript
interface OrderBookEntry {
  price: string;
  amount: string;
}

interface Trade {
  id: number;
  price: string;
  amount: string;
  type: "buy" | "sell";
  timestamp: number;
}

interface Order {
  id: number;
  pair: string;
  type: "buy" | "sell";
  orderType: "market" | "limit";
  amount: string;
  price: string;
  status: "open" | "filled" | "cancelled";
}
```

#### Wallet Data
```typescript
interface Balance {
  coin: string;
  available: number;
  locked: number;
  usdValue: number;
}

interface Transaction {
  id: number;
  type: "deposit" | "withdraw";
  coin: string;
  amount: string;
  status: "completed" | "pending" | "failed";
  createdAt: number;
  txHash: string;
}
```

### Demo Mode Data Generation

Mock data is generated in `src/services/api.ts`:

```typescript
const generateMockData = () => {
  const pairs = ["BTC/USDT", "ETH/USDT", "BNB/USDT", ...];
  return pairs.map(pair => ({
    pair,
    name: pair.split("/")[0],
    price: Math.random() * 50000 + 1000,
    priceChange24h: (Math.random() - 0.5) * 1000,
    // ... more fields
  }));
};
```

### API Endpoints

#### Markets API
- `GET /markets` - Get all markets
- `GET /markets/:pair` - Get specific market
- `GET /markets/stats/overview` - Market statistics
- `GET /markets/top-gainers` - Top gaining markets
- `GET /markets/top-losers` - Top losing markets
- `GET /markets/by-volume` - Markets sorted by volume
- `GET /markets/search?q=query` - Search markets
- `GET /markets/:pair/ticker` - Market ticker data
- `GET /markets/:pair/history` - Price history

#### Trading API
- `GET /trading/orderbook/:pair` - Order book
- `GET /trading/trades/:pair` - Recent trades
- `POST /trading/orders` - Place order
- `GET /trading/orders` - Get user orders
- `GET /trading/orders/:id` - Get order details
- `DELETE /trading/orders/:id` - Cancel order
- `GET /trading/stats/:pair` - Trading statistics
- `GET /trading/history` - User trading history

#### Wallet API
- `GET /wallet/balances` - Get all balances
- `GET /wallet/summary` - Wallet summary
- `GET /wallet/balance/:coin` - Get specific coin balance
- `GET /wallet/transactions` - Transaction history
- `GET /wallet/transactions/:id` - Transaction details
- `POST /wallet/deposit` - Create deposit
- `POST /wallet/withdraw` - Create withdrawal

#### Auth API
- `POST /auth/register` - User registration
- `POST /auth/login` - User login
- `POST /auth/logout` - User logout
- `GET /auth/verify` - Verify token
- `POST /auth/change-password` - Change password

### Demo Mode Detection

```typescript
const handleDemoMode = (mockData: any) => {
  const token = localStorage.getItem("token");
  if (token === "demo-token") {
    return Promise.resolve({ data: mockData });
  }
  return null;
};
```

### Data Flow

1. Component calls API function
2. API function checks for demo mode
3. If demo mode: return mock data immediately
4. If not demo mode: make HTTP request to backend
5. Response handled by React Query

### Integration Points

- **Real-time Updates**: Consider WebSocket integration for live market data
- **Caching**: React Query handles caching automatically
- **Error Handling**: Axios interceptors handle 401 errors (redirect to login)

---

## 📢 For Marketing Specialists

### Brand Identity

**MoCoreX** - Next-Generation DeFi Platform

**Logo Design:**
- Gradient "MoCore" text: Blue → Cyan → Purple
- Bold white "X" for emphasis
- SVG logo available in `/public/logo.svg`

### Content Strategy

#### Landing Page Sections
1. **Hero Section** (`src/components/Hero.tsx`)
   - Headline: "MoCoreX is Decentralized Perpetual Exchange"
   - Subtitle: Small font size matching DeFi platforms
   - CTA: "START TRADING" button
   - Statistics: TVL, Users, Volume, Markets

2. **Features Section** (`src/components/Features.tsx`)
   - Low Cost trading
   - Multi Collateral support
   - Transparency (open-source)

3. **Chain Section** (`src/components/ChainSection.tsx`)
   - Supported blockchain networks

4. **Ecosystem Tokens** (`src/components/EcosystemTokens.tsx`)
   - Token listings

### SEO Optimization

**Meta Tags** (in `index.html`):
- Title: "MoCoreX - Next-Generation DeFi Platform | Decentralized Finance"
- Description: Complete DeFi ecosystem description
- Keywords: DeFi, DEX, yield farming, staking, Web3, blockchain
- Open Graph tags for social sharing
- Twitter Card metadata

### Navigation Links

**Header Navigation:**
- Markets
- Trading
- Dashboard (protected)
- Wallet (protected)
- Settings (protected)
- Doc (opens PDF document)

**Footer Sections:**
- Brand (logo, description, social links)
- Product (links to features)
- Resources (documentation, support)
- Legal (terms, privacy, disclaimer)

### Call-to-Action Buttons

- **Primary CTA**: "START TRADING" (Hero section)
- **Secondary CTA**: "Continue as Demo User" (Login page)
- **Tertiary CTA**: "Connect Wallet" (redirects to login)

### Color Scheme

- **Primary**: Blue to Cyan to Purple gradients
- **Background**: Dark slate (slate-950, slate-900)
- **Text**: Slate-400 (secondary), White (primary)
- **Accents**: Blue-500, Cyan-400, Purple-500

### Content Updates

**To update hero text:**
- Edit `src/components/Hero.tsx`

**To update features:**
- Edit `src/components/Features.tsx`

**To update footer links:**
- Edit `src/components/Footer.tsx`

**To update meta tags:**
- Edit `index.html` in root directory

### Analytics Integration

**Recommended:**
- Google Analytics 4
- Mixpanel for user behavior
- Hotjar for heatmaps

**Implementation:**
Add tracking scripts to `index.html` or create a tracking component.

### Social Media

**Footer Social Links:**
- Twitter
- Discord
- Telegram
- GitHub

Update links in `src/components/Footer.tsx`.

---

## 🔧 For Backend Engineers

### API Integration

The frontend expects a RESTful API at the base URL specified in `VITE_API_URL`.

### Authentication

**Token-based Authentication:**
- Token stored in `localStorage` as `"token"`
- Sent in `Authorization: Bearer <token>` header
- Demo mode uses `"demo-token"` (no backend validation needed)

### Request/Response Format

**Standard Request:**
```http
GET /api/markets
Authorization: Bearer <token>
Content-Type: application/json
```

**Standard Response:**
```json
{
  "data": [...]
}
```

**Error Response:**
```json
{
  "error": "Error message",
  "statusCode": 400
}
```

### Required Endpoints

See [Data Engineers Section](#for-data-engineers) for complete endpoint list.

### CORS Configuration

Backend must allow:
- Origin: `http://localhost:8080` (development)
- Methods: GET, POST, DELETE
- Headers: Authorization, Content-Type

### Demo Mode Handling

Frontend automatically handles demo mode. Backend should:
- Accept `demo-token` as valid token (optional)
- Or return 401 and let frontend handle with mock data

### WebSocket Integration (Future)

Consider WebSocket endpoints for:
- Real-time order book updates
- Live trade feed
- Price ticker updates

**Suggested Endpoints:**
- `ws://api.mocorex.com/orderbook/:pair`
- `ws://api.mocorex.com/trades/:pair`
- `ws://api.mocorex.com/ticker/:pair`

### Rate Limiting

Recommended limits:
- Public endpoints: 100 requests/minute
- Authenticated endpoints: 1000 requests/minute
- Trading endpoints: 10 requests/second

### Error Codes

- `401 Unauthorized`: Invalid/missing token → Frontend redirects to login
- `400 Bad Request`: Invalid request data
- `404 Not Found`: Resource not found
- `500 Internal Server Error`: Server error

---

## 🧪 For QA Engineers

### Testing Strategy

#### Manual Testing Checklist

**Authentication:**
- [ ] Demo user login works
- [ ] Demo session persists on refresh
- [ ] Logout clears session
- [ ] Protected routes redirect when not authenticated

**Trading Page:**
- [ ] Order book displays correctly
- [ ] Recent trades show up
- [ ] Trade type selection (Long/Short) works
- [ ] Leverage slider functions
- [ ] Order placement (demo mode)
- [ ] Price and amount inputs validate

**Markets Page:**
- [ ] Market list loads
- [ ] Search functionality works
- [ ] Sorting by volume/price works
- [ ] Market cards display correctly
- [ ] Price changes show correct colors

**Wallet Page:**
- [ ] Balance display
- [ ] Transaction history loads
- [ ] Total balance calculation
- [ ] Coin filter works

**Dashboard:**
- [ ] Statistics display
- [ ] Charts render (if implemented)
- [ ] Performance metrics

**Responsive Design:**
- [ ] Mobile view (< 768px)
- [ ] Tablet view (768px - 1024px)
- [ ] Desktop view (> 1024px)

**Browser Compatibility:**
- [ ] Chrome/Edge (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)

### Demo Mode Testing

**Test Scenarios:**
1. Login as demo user → Verify mock data appears
2. Navigate all pages → Verify no API errors
3. Place orders → Verify demo success messages
4. Check wallet → Verify demo balances
5. Logout → Verify redirect to login

### Known Limitations

- **No Real Wallet Connection**: Application operates in demo mode only
- **Mock Data**: All data is generated client-side
- **No Persistence**: Demo data resets on page refresh (except auth token)

### Bug Reporting Template

```markdown
**Page**: [Trading/Markets/Wallet/etc.]
**Browser**: [Chrome/Firefox/Safari]
**Steps to Reproduce**:
1. ...
2. ...
3. ...

**Expected Behavior**:
...

**Actual Behavior**:
...

**Screenshots**:
[Attach if applicable]
```

### Performance Testing

**Key Metrics:**
- Initial page load: < 3 seconds
- API response time: < 500ms (mock data)
- Time to interactive: < 5 seconds

**Tools:**
- Chrome DevTools Lighthouse
- React DevTools Profiler

---

## 🚀 For DevOps Engineers

### Build Process

```bash
# Development
npm run dev          # Starts Vite dev server on port 8080

# Production Build
npm run build        # Outputs to /dist directory

# Preview Production Build
npm run preview      # Serves /dist for testing
```

### Build Output

Production build creates:
- `dist/index.html` - Entry HTML
- `dist/assets/` - Bundled JS/CSS files
- `dist/` - Static assets (images, fonts)

### Environment Variables

**Development:**
```env
VITE_API_URL=http://localhost:3000/api
```

**Production:**
```env
VITE_API_URL=https://api.mocorex.com/api
```

### Deployment

**Static Hosting Options:**
- Vercel
- Netlify
- AWS S3 + CloudFront
- GitHub Pages
- Cloudflare Pages

**Deployment Steps:**
1. Set `VITE_API_URL` environment variable
2. Run `npm run build`
3. Deploy `dist/` directory to hosting service

### Docker (Optional)

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Nginx Configuration

```nginx
server {
    listen 80;
    server_name mocorex.com;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api {
        proxy_pass http://backend:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### CI/CD Pipeline

**GitHub Actions Example:**
```yaml
name: Build and Deploy
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - run: npm run lint
      - uses: actions/upload-artifact@v3
        with:
          name: dist
          path: dist
```

### Monitoring

**Recommended Tools:**
- Sentry for error tracking
- LogRocket for session replay
- Google Analytics for user analytics

### Performance Optimization

**Already Implemented:**
- Code splitting via Vite
- Tree shaking
- Minification in production build

**Additional Optimizations:**
- Image optimization (WebP format)
- CDN for static assets
- Service worker for offline support (PWA)

### Security

- **Content Security Policy**: Configure CSP headers
- **HTTPS**: Always use HTTPS in production
- **Environment Variables**: Never commit `.env` files
- **Dependencies**: Regularly update with `npm audit`

---

## 📝 Additional Notes

### Demo Mode

The application is designed to work entirely in demo mode without backend connection. All API calls return mock data when `demo-token` is present in localStorage.

### Contributing

1. Create feature branch
2. Make changes
3. Test thoroughly
4. Submit pull request

### Support

For questions or issues, contact the development team or create an issue in the repository.

---

**Last Updated**: 2024