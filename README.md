# Inventory Management System App

A modern inventory management mobile app built with React Native and Expo. The app helps businesses track products, stock levels, suppliers, orders, and reports from a clean dashboard experience.

## Overview

IMS App is designed as a lightweight warehouse management system for keeping inventory organized and easy to monitor. It includes a dashboard overview, product inventory management, supplier records, order tracking, and profile/auth flows.

This project is structured around Expo Router and React Query, with mock local data for quick demo usage without a backend service.

## Features

- User authentication flow with login and signup screens
- Dashboard with key inventory metrics and low-stock alerts
- Product inventory listing and management
- Supplier list and supplier form
- Order tracking with status badges
- Reports overview for stock and category summaries
- Responsive mobile-first UI with modern card-based styling
- Zustand state management for auth session
- TanStack Query for data fetching and local mutation handling

## Tech Stack

- React Native
- Expo
- TypeScript
- Expo Router
- Zustand
- TanStack Query
- React Hook Form + Zod
- React Native Safe Area Context

## Project Structure

```bash
ims-app/
├── app/
│   ├── (auth)/
│   │   ├── login.tsx
│   │   └── signup.tsx
│   ├── (tabs)/
│   │   ├── dashboard.tsx
│   │   ├── inventory.tsx
│   │   ├── orders.tsx
│   │   ├── profile.tsx
│   │   ├── reports.tsx
│   │   └── suppliers.tsx
│   ├── product/
│   │   └── [id].tsx
│   ├── _layout.tsx
│   ├── index.tsx
│   └── ...
├── components/
│   ├── PrimaryButton.tsx
│   └── StatCard.tsx
├── constants/
│   └── theme.ts
├── features/
│   ├── inventory/
│   ├── orders/
│   └── suppliers/
├── lib/
│   ├── query-client.ts
│   └── store.ts
├── types/
│   └── index.ts
├── App.tsx
├── app.json
├── babel.config.js
├── index.ts
├── package.json
├── tsconfig.json
└── ...
```

## Getting Started

### Prerequisites

Make sure you have the following installed:

- Node.js 18+
- npm or yarn
- Expo CLI
- Android Studio / iOS simulator (optional for running on device/emulator)

### Install dependencies

```bash
npm install
```

### Start the app

```bash
npm start
```

Or directly run:

```bash
npx expo start
```

### Run on Android

```bash
npm run android
```

### Run on iOS

```bash
npm run ios
```

### Run in browser

```bash
npm run web
```

## Demo Credentials

The app includes demo user data for quick access.

- Email: maya@ims.app
- Password: password123

## Screens Included

- Login screen
- Signup screen
- Dashboard overview
- Inventory list with add product form
- Supplier management
- Order list
- Reports section
- Profile screen
- Product detail page

## Notes

- This version uses mock/local data and simulates async loading with a short delay.
- Auth state is demo-based and stored in Zustand rather than a real backend.
- The app is designed to be extended easily with a real database or API integration, such as Firebase or a REST backend.

## Future Enhancements

- Real backend integration
- Product edit/delete workflows
- Barcode scanning
- Search and filtering
- Inventory movement tracking
- Charts and analytics improvements
- Notification system for low stock

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Author

Built as a React Native inventory management app project for warehouse and stock operations.
