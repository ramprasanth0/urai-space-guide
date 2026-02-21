# Changelog

All notable changes to Urai Space will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.0] - 2026-02-21

### 🚀 New Features
- **Routine Tracking** - Design multi-day challenges with private daily journals.
- **Mobile Enhancements** - Integrated hardware back button support and tactile haptic feedback.
- **Improved Navigation** - Fixed TopBar and BottomNav for better mobile viewport usability.

## [1.2.0] - 2026-02-05

### 🚀 New Features
- **Data Export** - Decrypted JSON backup using secure `verifyPassword` check.
- **Account Deletion** - Permanent account removal using `delete_own_user` RPC.
- **Recovery Fix** - Enabling recovery even when logged out via `get_recovery_blob`.

## [1.1.0] - 2026-02-05

### 🎨 New Features
- **Drawings** - Complete sketching module with pen, eraser, colors, and brush sizes.
- **Gallery View** - Manage sketches in a grid layout.
- **Bulk Actions** - Select and delete multiple sketches at once.
- **Feedback UI** - "Saved" success indicator (green tick) for all database actions.

### 🛠️ Improvements
- **Editor Layout** - Tools moved to left sidebar for better ergonomics.
- **Icons** - Updated clear icon to "Stop" style; improved visibility of selection ticks.
- **Data Integrity** - Fixed bug where editing created duplicates.

## [1.0.0] - 2026-02-05

### 🔐 Security
- **Zero-knowledge encryption** - All data encrypted client-side using AES-256-GCM
- **PBKDF2 key derivation** - 100,000 iterations for password protection
- **12-word recovery phrase** - Secure backup for account recovery
- **No plaintext storage** - Server only stores encrypted blobs

### 🗄️ Database
- **Supabase integration** - Cloud sync with encrypted data
- **Fallback to localStorage** - Works offline when Supabase not configured
- **Email-based account recovery** - Login on new devices with email + password

### ✨ Features
- **Diary** - Date-based encrypted entries with calendar view
- **Notes** - Google Keep-style tiles with 8 color options
- **Multi-account support** - Switch between accounts on same device
- **Dark/Light mode** - Beautiful theme toggle
- **Lock screen** - Password-protected access

### 🎨 UI/UX
- **Glassmorphism design** - Modern frosted glass effects
- **Responsive layout** - Works on desktop and mobile
- **Security info modal** - Explains encryption to users
- **Password strength indicator** - Visual feedback on password quality

### 🔧 Technical
- Built with Next.js 16
- Web Crypto API for encryption
- Supabase for backend storage
- Tailwind CSS for styling
