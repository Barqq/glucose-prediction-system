# UX/UI Design Guidelines for AI-Powered Post-Meal Glucose Prediction System

## Overview

This document outlines the user experience (UX) and user interface (UI) design guidelines for the AI-Powered Post-Meal Glucose Prediction System, specifically tailored for the Saudi Arabian market. The design prioritizes an **Arabic-first, right-to-left (RTL), iOS-style interface** with smooth micro-animations and a minimal, modern aesthetic.

## 1. Design Principles

### 1.1 Arabic-First Approach

The system is designed with **Modern Standard Arabic (MSA)** as the primary language, ensuring that all interface elements, content, and interactions are optimized for Arabic speakers. The interface follows right-to-left (RTL) reading direction, which is natural for Arabic users.

### 1.2 iOS-Style Design Language

The interface adopts iOS design conventions to create a familiar and intuitive experience for users. This includes:

- **Clarity:** Text is legible at every size, icons are precise and lucid, and adornments are subtle and appropriate.
- **Deference:** Fluid motion and crisp, beautiful interface help people understand and interact with content while never competing with it.
- **Depth:** Visual layers and realistic motion convey hierarchy, impart vitality, and facilitate understanding.

### 1.3 Minimal and Modern Aesthetic

The design embraces minimalism with clean lines, ample white space, and a focus on essential elements. This approach reduces cognitive load and allows users to focus on critical health information without distraction.

### 1.4 Smooth Micro-Animations

Micro-animations enhance the user experience by providing visual feedback, guiding attention, and creating a sense of continuity. These animations should be subtle, purposeful, and performant, typically lasting between 200-400ms.

## 2. Right-to-Left (RTL) Design Considerations

### 2.1 Layout and Navigation

Following Apple's Human Interface Guidelines for RTL languages, the interface must be mirrored to match the reading direction of Arabic:

- **Navigation flow:** Back buttons point to the right, forward buttons point to the left.
- **Content alignment:** Text and content are right-aligned by default.
- **Tab bars and toolbars:** Icons and items are arranged from right to left.
- **Swipe gestures:** Swipe right to go back, swipe left to go forward.

### 2.2 Text Alignment

- **Single and two-line text:** Right-aligned to match the RTL context.
- **Paragraphs (3+ lines):** Always aligned based on the language of the text itself, not the interface direction.
- **Lists:** All items in a list maintain consistent right-alignment in the RTL context.

### 2.3 Numbers and Characters

- **Arabic numerals:** Support both Western Arabic numerals (0-9) and Eastern Arabic numerals (٠-٩) based on user preference or regional settings.
- **Number order:** Never reverse the order of digits in a specific number (e.g., phone numbers, glucose readings).
- **Progress indicators:** Flip the direction of progress bars, sliders, and rating controls to move from right to left.

### 2.4 Icons and Images

- **Directional icons:** Flip icons that represent text direction, navigation, or forward/backward motion.
- **Non-directional icons:** Do not flip universal symbols (checkmarks, warnings), logos, or icons representing real-world objects (clocks, medical instruments).
- **SF Symbols:** Utilize SF Symbols with built-in RTL variants for consistency.

## 3. Screen Designs and User Flows

### 3.1 Onboarding Flow

The onboarding experience introduces users to the system's capabilities and collects essential health information.

**Screen 1: Welcome**
- **Content:** Brief introduction to the app with an engaging illustration.
- **Elements:** App logo, welcome message in Arabic, "ابدأ" (Start) button.
- **Animation:** Fade-in effect for text and button.

**Screen 2: Features Overview**
- **Content:** Carousel showcasing key features (Meal Photo Analyzer, Insulin Calculator, Early Warning Engine).
- **Elements:** Feature illustrations, descriptive text, pagination dots, "التالي" (Next) button.
- **Animation:** Horizontal swipe between features (right to left).

**Screen 3: Health Profile Setup**
- **Content:** Form to collect user health information.
- **Elements:** Input fields for age, weight, height, diabetes type, current insulin regimen, Carb Ratio (CR), Correction Factor (CF).
- **Animation:** Slide-up keyboard, smooth transitions between fields.

**Screen 4: Permissions**
- **Content:** Request necessary permissions (camera, notifications).
- **Elements:** Permission descriptions, "السماح" (Allow) and "لاحقاً" (Later) buttons.
- **Animation:** Modal presentation with backdrop blur.

### 3.2 Dashboard (Home Screen)

The dashboard provides an at-a-glance view of the user's glucose status, recent meals, and insulin logs.

**Layout:**
- **Top section:** Greeting message with user name, current date in Arabic format.
- **Glucose chart:** Line chart showing simulated CGM data for the past 24 hours with color-coded zones (hypo, normal, hyper).
- **Quick actions:** Large, prominent buttons for "تصوير الوجبة" (Photograph Meal) and "حساب الأنسولين" (Calculate Insulin).
- **Recent activity:** Scrollable list of recent meals and insulin doses with timestamps.
- **Bottom navigation:** Tab bar with icons for Dashboard, History, Insights, Profile.

**Visual Design:**
- **Color scheme:** Soft blues and greens for normal glucose, yellows for warnings, reds for alerts. White background with subtle gradients.
- **Typography:** SF Arabic font family for consistency with iOS. Large, readable font sizes (minimum 17pt for body text).
- **Cards:** Rounded corners (12pt radius), subtle shadows for depth.

**Animations:**
- **Chart:** Animated line drawing on load.
- **Cards:** Fade-in with slight upward motion (staggered timing).
- **Buttons:** Scale effect on tap (0.95x).

### 3.3 Meal Photo Analyzer Screen

This screen allows users to capture or upload a photo of their meal for AI-based analysis.

**Layout:**
- **Camera view:** Full-screen camera preview with overlay guides.
- **Capture button:** Large circular button at the bottom center.
- **Gallery button:** Small button in the bottom-right corner to access photo library.
- **Flash and camera switch:** Icons in the top corners.

**Post-Capture Flow:**
- **Preview screen:** Display captured image with "إعادة التقاط" (Retake) and "تحليل" (Analyze) buttons.
- **Analysis screen:** Loading indicator with progress message, then results display.

**Results Display:**
- **Meal identification:** Recognized dishes with confidence scores.
- **Nutritional breakdown:** Estimated carbohydrates, proteins, fats, calories displayed in cards.
- **Portion size:** Visual representation (small, medium, large) with adjustment slider.
- **Predicted glucose:** Chart showing expected postprandial glucose curve over 2-3 hours.
- **Actions:** "حفظ" (Save) and "حساب الأنسولين" (Calculate Insulin) buttons.

**Animations:**
- **Camera shutter:** Quick flash effect on capture.
- **Loading:** Circular progress indicator with rotating animation.
- **Results:** Fade-in with slide-up motion for each card.

### 3.4 Insulin Calculator Screen

This screen calculates the recommended insulin dose based on meal carbohydrates and current glucose level.

**Layout:**
- **Input section:**
  - Current glucose reading (manual entry or from CGM).
  - Total carbohydrates (auto-filled from meal analysis or manual entry).
  - Target glucose level (pre-set in profile, adjustable).
- **Calculation display:**
  - Carb coverage dose: (Total carbs ÷ Carb Ratio).
  - Correction dose: (Current glucose - Target glucose) ÷ Correction Factor.
  - **Total recommended dose:** Large, prominent display with units.
- **Confirmation:** "تأكيد الجرعة" (Confirm Dose) button.

**Visual Design:**
- **Input fields:** Large, touch-friendly with clear labels and units.
- **Calculation breakdown:** Step-by-step display with mathematical notation.
- **Total dose:** Bold, large font (32pt+) with color-coded background (green for safe range, yellow for caution).

**Animations:**
- **Calculation:** Number counting animation from 0 to final value.
- **Confirmation:** Haptic feedback and checkmark animation.

### 3.5 Alert Screen (Early Warning)

This screen displays alerts for predicted hypo/hyperglycemia based on AI analysis.

**Layout:**
- **Alert banner:** Full-width banner at the top with icon and message.
- **Severity indicator:** Color-coded (yellow for warning, red for critical).
- **Details section:** Explanation of the alert, predicted glucose level, time frame.
- **Recommendations:** Actionable steps (e.g., "تناول 15 جرام من الكربوهيدرات" - Consume 15g of carbohydrates).
- **Actions:** "تم الفهم" (Understood) and "تأجيل التذكير" (Snooze Reminder) buttons.

**Animations:**
- **Alert appearance:** Slide-down from top with bounce effect.
- **Icon:** Pulsing animation for critical alerts.

### 3.6 History Screen

This screen provides a comprehensive view of past meals, insulin doses, and glucose trends.

**Layout:**
- **Filter options:** Date range selector, meal type filter.
- **Timeline view:** Chronological list of entries with meal photos, carb counts, insulin doses, and glucose readings.
- **Expandable entries:** Tap to view detailed information.

**Visual Design:**
- **Timeline:** Vertical line with nodes for each entry.
- **Entry cards:** Compact cards with thumbnail image, key metrics, and timestamp.

**Animations:**
- **Expand/collapse:** Smooth height transition with fade-in for additional content.

### 3.7 Profile Screen

This screen allows users to manage their account, health settings, and app preferences.

**Layout:**
- **User info:** Profile picture, name, age, diabetes type.
- **Health settings:** Carb Ratio, Correction Factor, target glucose range.
- **App settings:** Language, notifications, data sync.
- **Support:** Help center, privacy policy, terms of service.
- **Logout:** "تسجيل الخروج" (Logout) button.

**Visual Design:**
- **Sections:** Grouped lists with headers.
- **Settings rows:** Right-aligned labels with left-aligned values/controls.

**Animations:**
- **Navigation:** Push/pop transitions for sub-screens.

## 4. Typography

### 4.1 Font Family

- **Primary font:** SF Arabic (system font for iOS with Arabic support).
- **Fallback:** SF Pro Text for Latin characters and numbers.

### 4.2 Font Sizes and Weights

- **Large Title:** 34pt, Bold (for screen titles).
- **Title 1:** 28pt, Regular (for section headers).
- **Title 2:** 22pt, Regular (for card titles).
- **Headline:** 17pt, Semibold (for emphasis).
- **Body:** 17pt, Regular (for main content).
- **Callout:** 16pt, Regular (for secondary content).
- **Subhead:** 15pt, Regular (for labels).
- **Footnote:** 13pt, Regular (for captions).
- **Caption 1:** 12pt, Regular (for timestamps).

### 4.3 Line Spacing

- **Body text:** 1.4x line height for readability.
- **Headings:** 1.2x line height for compactness.

### 4.4 Visual Balancing

When Arabic text appears next to uppercased Latin text, increase the Arabic font size by approximately 2 points to achieve visual balance.

## 5. Color Palette

### 5.1 Primary Colors

- **Primary Blue:** #007AFF (iOS system blue, for primary actions and links).
- **Success Green:** #34C759 (for positive feedback, normal glucose range).
- **Warning Yellow:** #FF9500 (for caution, borderline glucose levels).
- **Error Red:** #FF3B30 (for alerts, critical glucose levels).

### 5.2 Neutral Colors

- **Background:** #FFFFFF (white for main background).
- **Secondary Background:** #F2F2F7 (light gray for cards and sections).
- **Tertiary Background:** #E5E5EA (for subtle dividers).
- **Label (Primary):** #000000 (black for main text).
- **Label (Secondary):** #3C3C43 (60% opacity, for secondary text).
- **Label (Tertiary):** #3C3C43 (30% opacity, for tertiary text).

### 5.3 Glucose Range Colors

- **Hypoglycemia:** #FF3B30 (red, <70 mg/dL).
- **Normal:** #34C759 (green, 70-180 mg/dL).
- **Hyperglycemia:** #FF9500 (orange, >180 mg/dL).

### 5.4 Accessibility

All color combinations must meet WCAG 2.1 Level AA contrast ratios (4.5:1 for normal text, 3:1 for large text).

## 6. Iconography

### 6.1 Icon Style

- **SF Symbols:** Use SF Symbols for consistency with iOS design language.
- **Custom icons:** If custom icons are needed, follow SF Symbols design principles (simple, geometric, 2pt stroke weight).

### 6.2 RTL Considerations

- **Directional icons:** Use RTL variants of SF Symbols for navigation, text alignment, and motion-related icons.
- **Non-directional icons:** Use standard versions for universal symbols, medical icons, and real-world objects.

### 6.3 Icon Sizes

- **Tab bar:** 28x28pt.
- **Navigation bar:** 22x22pt.
- **Inline:** 17x17pt (matching body text size).
- **Large feature icons:** 60x60pt or larger.

## 7. Micro-Animations

### 7.1 Principles

- **Purposeful:** Every animation should have a clear purpose (feedback, guidance, delight).
- **Subtle:** Animations should enhance, not distract.
- **Performant:** Maintain 60fps for smooth experience.
- **Consistent:** Use consistent timing and easing functions throughout the app.

### 7.2 Animation Types

- **Fade:** Opacity transition for appearing/disappearing elements (200-300ms).
- **Slide:** Position transition for screen navigation (300-400ms).
- **Scale:** Size transition for button presses (100-150ms, scale to 0.95x).
- **Bounce:** Spring animation for alerts and confirmations (400-500ms).
- **Progress:** Animated progress indicators for loading states.

### 7.3 Easing Functions

- **Ease-out:** For elements entering the screen (fast start, slow end).
- **Ease-in:** For elements leaving the screen (slow start, fast end).
- **Ease-in-out:** For elements moving within the screen (smooth start and end).
- **Spring:** For playful, natural motion (bounce effect).

## 8. Interaction Patterns

### 8.1 Gestures

- **Tap:** Primary interaction for buttons, links, and selectable items.
- **Swipe (right):** Navigate back in navigation stack.
- **Swipe (left):** Navigate forward or reveal actions.
- **Swipe (vertical):** Scroll content, dismiss modals.
- **Long press:** Reveal contextual actions or additional information.
- **Pinch:** Zoom in/out on charts and images.

### 8.2 Feedback

- **Visual:** Button state changes (pressed, disabled), loading indicators.
- **Haptic:** Subtle vibrations for confirmations, errors, and significant events.
- **Audio:** Optional sound effects for alerts (must be culturally appropriate).

### 8.3 Accessibility

- **VoiceOver:** All interactive elements must have descriptive labels in Arabic.
- **Dynamic Type:** Support for user-adjustable text sizes.
- **Reduce Motion:** Provide alternatives to animations for users with motion sensitivity.
- **High Contrast:** Ensure readability in high contrast mode.

## 9. Privacy and Security Indicators

### 9.1 Data Encryption

Display a lock icon and "مشفر" (Encrypted) label when transmitting sensitive health data.

### 9.2 Permissions

Clearly explain why each permission is needed before requesting it, using plain Arabic language.

### 9.3 Data Usage

Provide transparency about how user data is collected, stored, and used, with links to privacy policy.

## 10. Localization and Cultural Considerations

### 10.1 Language

- **Modern Standard Arabic (MSA):** Use formal, professional Arabic suitable for medical contexts.
- **Avoid colloquialisms:** Ensure language is understood across all Arabic-speaking regions.

### 10.2 Date and Time Formats

- **Date:** Use Gregorian calendar by default, with option for Hijri calendar.
- **Time:** 12-hour format with Arabic AM/PM indicators (ص/م).

### 10.3 Units

- **Glucose:** mg/dL (common in Saudi Arabia), with option for mmol/L.
- **Weight:** Kilograms (kg).
- **Height:** Centimeters (cm).

### 10.4 Cultural Sensitivity

- **Meal times:** Consider Saudi meal patterns (breakfast, lunch, dinner, and potential snacks during Ramadan).
- **Ramadan support:** Provide special features or guidance for fasting periods.
- **Gender considerations:** Ensure interface is appropriate for all users, respecting cultural norms.

## 11. Responsive Design

### 11.1 Device Support

- **iPhone:** Optimize for various iPhone sizes (SE, standard, Plus/Max, Pro).
- **iPad:** Adapt layout for larger screens with multi-column layouts where appropriate.
- **Orientation:** Support both portrait and landscape orientations.

### 11.2 Safe Areas

Respect safe areas for notched devices, ensuring content is not obscured by device features.

## 12. Design System and Component Library

### 12.1 Reusable Components

Create a comprehensive design system with reusable components:

- **Buttons:** Primary, secondary, tertiary, destructive.
- **Input fields:** Text, number, date picker, dropdown.
- **Cards:** Info card, meal card, glucose reading card.
- **Charts:** Line chart, bar chart, pie chart.
- **Alerts:** Banner, modal, toast.
- **Navigation:** Tab bar, navigation bar, segmented control.

### 12.2 Documentation

Maintain detailed documentation for each component, including:

- **Usage guidelines:** When and how to use the component.
- **Variants:** Different states and configurations.
- **Code examples:** Implementation snippets for developers.

## 13. Implementation Notes

### 13.1 Technology Stack

- **Frontend:** React with RTL support (using libraries like `react-i18next` and `styled-components` with RTL capabilities).
- **UI Framework:** Custom components styled to match iOS design language, or use libraries like `react-native` for native feel.
- **Animation:** Framer Motion or React Spring for smooth animations.
- **Charts:** Recharts or Chart.js with RTL support.

### 13.2 RTL Implementation

- **CSS:** Use `direction: rtl` on the root element.
- **Flexbox:** Utilize `flex-direction: row-reverse` for RTL layouts.
- **Logical properties:** Use `margin-inline-start`, `padding-inline-end` instead of left/right for automatic RTL adaptation.
- **Testing:** Thoroughly test all screens in RTL mode to ensure proper layout and functionality.

### 13.3 Accessibility

- **ARIA labels:** Provide Arabic ARIA labels for all interactive elements.
- **Keyboard navigation:** Ensure all functionality is accessible via keyboard.
- **Screen reader testing:** Test with VoiceOver in Arabic mode.

## 14. Next Steps

With these design guidelines established, the next phase will focus on:

1. **Creating detailed mockups and prototypes** using design tools (Figma, Sketch).
2. **User testing** with Arabic-speaking users to validate design decisions.
3. **Developing the component library** for implementation.
4. **Preparing design assets** (icons, illustrations, color swatches) for developers.

This comprehensive UX/UI design guideline ensures that the AI-Powered Post-Meal Glucose Prediction System will provide a culturally appropriate, accessible, and delightful experience for Saudi Arabian users, while adhering to iOS design conventions and RTL best practices.
