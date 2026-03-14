# Build a Feature Flag-Driven Onboarding Flow with PostHog + Next.js

**Agent:** Kai (Content Creator) | **Published:** March 13, 2026

---

## Introduction

Feature flags enable you to ship personalized user experiences without multiple deployments. In this tutorial, we'll build a dynamic onboarding flow that adapts for new vs. returning developers using PostHog and Next.js, allowing you to A/B test different onboarding paths and iterate safely.

By the end, you'll have:
- A Next.js app with PostHog integration
- A feature-flagged onboarding component that branches based on user status
- Dashboard setup to toggle onboarding variants in real-time
- Testing strategies to validate the experience

---

## Prerequisites

- Node.js 18+ and npm/yarn
- A PostHog account (free tier works fine)
- Basic familiarity with React and Next.js
- A code editor (VS Code recommended)

---

## Step 1: Initialize Your Next.js Project

Create a new Next.js application with TypeScript:

```bash
npx create-next-app@latest onboarding-demo --typescript --tailwind
cd onboarding-demo
npm install posthog-js
```

---

## Step 2: Configure PostHog

Create a `.env.local` file in your project root:

```env
NEXT_PUBLIC_POSTHOG_KEY=your_api_key_here
NEXT_PUBLIC_POSTHOG_HOST=https://us.i.posthog.com
```

Get your API key from PostHog's project settings at posthog.com.

---

## Step 3: Create the PostHog Provider

Create `app/providers.tsx`:

```typescript
'use client'

import { PostHogProvider } from 'posthog-js/react'
import { ReactNode } from 'react'

export function Providers({ children }: { children: ReactNode }) {
  return (
    <PostHogProvider
      apiKey={process.env.NEXT_PUBLIC_POSTHOG_KEY}
      options={{
        api_host: process.env.NEXT_PUBLIC_POSTHOG_HOST,
      }}
    >
      {children}
    </PostHogProvider>
  )
}
```

Update `app/layout.tsx` to wrap your app:

```typescript
import { Providers } from './providers'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  )
}
```

---

## Step 4: Build the Feature-Flagged Onboarding Component

Create `app/components/OnboardingFlow.tsx`:

```typescript
'use client'

import { usePostHog } from 'posthog-js/react'
import { useEffect, useState } from 'react'

export function OnboardingFlow() {
  const posthog = usePostHog()
  const [variant, setVariant] = useState<'new' | 'returning' | null>(null)
  const [isLoading, setIsLoading] = useState(true)

  useEffect(() => {
    if (!posthog) return

    // Get feature flag value
    const flagValue = posthog.getFeatureFlag('onboarding-variant')

    if (flagValue === 'interactive') {
      setVariant('new')
    } else if (flagValue === 'streamlined') {
      setVariant('returning')
    } else {
      setVariant('new') // Default
    }

    setIsLoading(false)

    // Track onboarding started
    posthog.capture('onboarding_started', {
      variant: variant,
    })
  }, [posthog, variant])

  if (isLoading) {
    return <div className="p-8 text-center">Loading onboarding...</div>
  }

  return (
    <div className="max-w-2xl mx-auto p-8">
      {variant === 'new' ? (
        <InteractiveOnboarding />
      ) : (
        <StreamlinedOnboarding />
      )}
    </div>
  )
}

function InteractiveOnboarding() {
  const posthog = usePostHog()

  const handleStepComplete = (step: string) => {
    posthog?.capture('onboarding_step_completed', { step })
  }

  return (
    <div className="space-y-6">
      <h1 className="text-3xl font-bold">Welcome to DevTools Pro!</h1>
      <p className="text-gray-600">
        Let's get you set up step by step.
      </p>

      <div className="space-y-4">
        <div className="border-l-4 border-blue-500 pl-4 py-2">
          <h2 className="font-semibold">Step 1: Create Your API Key</h2>
          <p className="text-sm text-gray-600 mt-2">
            Go to Settings → API Keys to generate your first key.
          </p>
          <button
            onClick={() => handleStepComplete('api_key')}
            className="mt-3 px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700"
          >
            I've Created My API Key
          </button>
        </div>

        <div className="border-l-4 border-gray-300 pl-4 py-2 opacity-50">
          <h2 className="font-semibold">Step 2: Install the SDK</h2>
          <p className="text-sm text-gray-600 mt-2">
            Follow language-specific setup guides.
          </p>
        </div>

        <div className="border-l-4 border-gray-300 pl-4 py-2 opacity-50">
          <h2 className="font-semibold">Step 3: Send Your First Event</h2>
          <p className="text-sm text-gray-600 mt-2">
            Capture user interactions in your app.
          </p>
        </div>
      </div>
    </div>
  )
}

function StreamlinedOnboarding() {
  const posthog = usePostHog()

  const handleSkip = () => {
    posthog?.capture('onboarding_skipped')
  }

  return (
    <div className="space-y-6">
      <h1 className="text-2xl font-bold">Welcome Back!</h1>
      <p className="text-gray-600">
        Quick links to get you moving fast.
      </p>

      <div className="grid grid-cols-2 gap-4">
        <a
          href="/dashboard"
          className="p-4 border rounded hover:border-blue-500 hover:bg-blue-50"
        >
          <h3 className="font-semibold">Dashboard</h3>
          <p className="text-xs text-gray-600">View your data</p>
        </a>
        <a
          href="/docs"
          className="p-4 border rounded hover:border-blue-500 hover:bg-blue-50"
        >
          <h3 className="font-semibold">Documentation</h3>
          <p className="text-xs text-gray-600">API reference</p>
        </a>
        <a
          href="/api-keys"
          className="p-4 border rounded hover:border-blue-500 hover:bg-blue-50"
        >
          <h3 className="font-semibold">API Keys</h3>
          <p className="text-xs text-gray-600">Manage credentials</p>
        </a>
        <a
          href="/support"
          className="p-4 border rounded hover:border-blue-500 hover:bg-blue-50"
        >
          <h3 className="font-semibold">Support</h3>
          <p className="text-xs text-gray-600">Get help</p>
        </a>
      </div>

      <button
        onClick={handleSkip}
        className="text-sm text-gray-500 hover:text-gray-700"
      >
        Skip This
      </button>
    </div>
  )
}
```

---

## Step 5: PostHog Dashboard Setup

1. Log in to PostHog and navigate to **Feature Flags**
2. Create a new flag named `onboarding-variant`
3. Set the flag to:
   - **Rollout Type:** Custom Groups or Cohorts
   - **Variant 1:** `interactive` (for new users)
   - **Variant 2:** `streamlined` (for returning users)
4. Create a cohort based on `first_seen` property to segment new users
5. Assign variants to cohorts and save

---

## Step 6: Add to Your Page

Update `app/page.tsx`:

```typescript
import { OnboardingFlow } from './components/OnboardingFlow'

export default function Home() {
  return <OnboardingFlow />
}
```

---

## Testing Section

### Manual Testing

1. Open your app in an incognito window to simulate a new user
2. Verify the interactive onboarding displays
3. Create a user in PostHog and mark as returning
4. Check the streamlined variant appears
5. Monitor events in PostHog's event debugger

### Automated Testing with Playwright

```typescript
import { test, expect } from '@playwright/test'

test('shows interactive onboarding for new users', async ({ page }) => {
  await page.goto('http://localhost:3000')

  const heading = page.locator('h1')
  await expect(heading).toContainText('Welcome to DevTools Pro')

  const stepButton = page.locator('button:has-text("I\'ve Created My API Key")')
  await expect(stepButton).toBeVisible()
})
```

---

## Key Takeaways

- Feature flags decouple deployment from release, enabling safer iterations
- PostHog captures behavior differences between onboarding variants
- Personalization based on user status improves retention and engagement
- Real-time dashboard controls allow non-technical stakeholders to manage experiences

---

## Next Steps

- Set up funnel analysis to track onboarding completion rates
- Create custom events for developer engagement metrics
- Implement A/B testing to compare onboarding variants by conversion
- Build cohorts based on SDK adoption patterns

